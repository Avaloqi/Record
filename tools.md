# Tools
## AddressSanitizer (ASAN)


### 一、历史背景与来源
#### 1. 来源
ASAN 最初由 Google 的工程师 Konstantin Serebryany 领导开发。它最早作为 LLVM/Clang 项目的一部分在 2011 年发布，随后在 2012 年被集成到 GCC (从 4.8 版本开始)。

#### 2. 开发背景
在 ASAN 出现之前，开发者主要依赖：  
Valgrind: 模拟 CPU 指令执行，极慢，且无法检测栈溢出。 
Electric Fence: 通过保护页检测，但非常消耗内存。        
手动检查: 效率极低。           
Google 为了解决 Chrome 浏览器中复杂的内存安全问题，开发了基于影子内存 (Shadow Memory) 算法的 ASAN，实现了速度与检测能力的平衡。

---

### 二、核心原理简述
ASAN 的核心逻辑分为两部分：
* 编译时插桩 (Instrumentation)：编译器在所有内存访问指令前插入检查逻辑。 
* 运行时库 (Runtime Library)：替换 malloc/free 等函数，并在内存块周围设立 Redzones（红区）。

---

### 三、部署支持方式
#### 1. 静态链接 vs 动态链接
* 动态链接 (推荐): 默认情况下，ASAN 会链接 libasan.so。这要求运行环境有对应的动态库。  
  (编译时链接动态库，运行前export LD_LIBRARY_PATH 指定asan的so路径)
* 静态链接: 在一些嵌入式或受限环境下，可以使用 -static-libasan 将库打入二进制文件。

#### 2. 共享库 (Shared Library) 调试
如果程序通过 dlopen 加载插件，需要确保：
主程序和插件都用 ASAN 编译。
或者在运行前通过 LD_PRELOAD 强制加载 ASAN 运行库：
```
export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libasan.so.5
./my_app
```

---

### 四、使用方法详解
#### 1. 编译选项
在编译和链接阶段，都需要加入 -fsanitize=address。为了获得可读的堆栈信息，必须加入 -g。
```
# 基本编译
gcc -fsanitize=address -g -O1 -o my_app main.c

# 推荐的最佳实践组合
gcc -fsanitize=address \
    -fno-omit-frame-pointer \
    -g -O1 \
    -o my_app main.c
```
* -fno-omit-frame-pointer: 确保函数调用栈（Stack Trace）在崩溃时能被正确打印。

* -O1: 开启基础优化，既能保持代码逻辑清晰，又能让插桩逻辑运行更快。

#### 2. 常用运行时参数 (ASAN_OPTIONS)
可以通过环境变量动态调整 ASAN 的行为，无需重新编译。

|参数项|说明|示例|
|---|---|---|
|halt_on_error|检测到第一个错误后是否停止程序 (默认 true)|halt_on_error=0 (报错不退出)|
|detect_leaks|是否启用内存泄漏检测 (默认 true)|detect_leaks=1|
|log_path|将错误信息重定向到文件而非终端|log_path=./asan.log|
|verbosity|打印级别，展示初始化过程|verbosity=1|
|check_initialization_order|检测全局变量初始化顺序导致的问题|check_initialization_order=true|
|detect_stack_use_after_return|检测函数返回后访问局部变量 (需额外开销)|detect_stack_use_after_return=1|

```
export ASAN_OPTIONS="halt_on_error=0:log_path=./out:detect_leaks=1"
./my_app
```

---

### 五、支持的环境与限制
#### 1. 支持的架构
* **x86 / x86_64** (支持最完善)
* **ARM / AArch64** (广泛用于移动端和嵌入式)
* **MIPS, PowerPC, s390** 等。

#### 2. 支持的操作系统
* **Linux**: 支持最全面，功能最稳。
* **macOS**: 集成在 Xcode/Clang 中。
* **Windows**: 较新版本的 MSVC (VS 2019 16.9+) 已开始支持。
* **Android**: 自 Android 6.0 起原生支持，常用于 App 稳定性测试。

#### 3. ASAN 无法检测的问题
* **未初始化的读取**: 需要使用 MemorySanitizer (MSAN)。
* **读取未插桩库的内存**: 如果你链接了一个没有用 ASAN 编译的第三方 `.so`，ASAN 无法检测该库内部的违规行为。

---

### 六、适用度评价

| 场景 | 推荐度 | 原因 |
| :--- | :--- | :--- |
| **单元测试 / 集成测试** | **极高** | 必须开启，能自动捕捉 90% 以上的内存隐患。 |
| **开发环境调试** | **高** | 替代 GDB 慢慢肉眼查错，直接定位行号。 |
| **性能测试** | **低** | ASAN 会带来 2 倍左右的 CPU 开销和 3 倍左右的内存占用。 |
| **线上生产环境** | **极低** | 除非是专门的 Canary 测试版本，否则不建议开启。 |

---

### 七、相关家族成员 (The Sanitizer Family)
除了 ASAN，你还可以根据需求组合使用：
* **LSAN (LeakSanitizer)**: 专门查内存泄漏（ASAN 已内置）。
* **TSAN (ThreadSanitizer)**: 专门查**多线程竞争 (Race Condition)**。
* **UBSAN (UndefinedBehaviorSanitizer)**: 查**未定义行为**（如整数溢出、空指针解引用）。
* **MSAN (MemorySanitizer)**: 查**未初始化内存读取**（仅限 Clang）。
