## 执行程序 my_app 并重定向日志输出到文件 my_app.log 时发现程序中的 printf 打印缺失或者有延迟
**主要的原因是 标准输出（stdout）的缓冲机制（Buffering）**  
在 Linux 中，标准输出有三种缓冲模式：
* 行缓冲（Line Buffered）：遇到换行符 \n 就输出（程序连接到终端/屏幕时默认此模式）。
* 全缓冲（Fully Buffered）：缓冲区满了（通常 4KB）才输出（输出重定向到文件时默认此模式）。
* 无缓冲（Unbuffered）：立即输出（标准错误 stderr 默认此模式）。
现象解释： 当使用 ./my_app > my_app.log 时，系统识别到输出目标是文件，会自动从“行缓冲”切换到“全缓冲”。如果程序打印频率不高，或者没有手动刷新，数据会一直卡在内存缓冲区里，直到程序正常结束或缓冲区填满。
**优化方案**  
方案 A：在 C 代码中强制刷新（最稳妥）
如果可以修改代码，在 printf 后调用 fflush，或者在程序初始化处更改缓冲模式。
```
// 方式 1：打印后立即刷新
printf("Message\n");
fflush(stdout);

// 方式 2：在 main 函数开头设置 stdout 为无缓冲（推荐）
setvbuf(stdout, NULL, _IONBF, 0);
```
方案 B：使用 stdbuf 命令（无需改代码，最推荐）
在执行命令前加上 stdbuf -oL，强制将标准输出设为行缓冲。
```
// 原命令 nohup stdbuf -oL my_app > my_app.log 2>&1 &
# -oL 表示输出流使用行缓冲 (Line buffered)
nohup stdbuf -oL my_app > my_app.log 2>&1 &
```
方案 C：使用环境变量（针对 ASAN/GLIBC）
有些环境下，可以通过环境变量尝试改变行为，但 stdbuf 通常更有效。

**补充说明：为什么 stderr 总是能看到？**  
可能会发现 ASAN 的报错信息（属于 stderr）是能实时看到的。这是因为 stderr 默认就是无缓冲的，系统为了保证错误信息不丢失，会立即将其写入文件。
