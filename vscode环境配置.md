# C/C++ 开发环境
## vscode 远程开发+ssh免密 环境配置
1.windows本地配置  
powershell 打开，输入ssh-keygen 回车两次直到命令结束，生产RSA密钥对  
生成的文件 id_rsa为私钥，本地保留，id_rsa.pub为公钥，后续复制到远程服务器上  

2.远程服务器配置  
将上面的id_rsa.pub加入到远程服务器对应的用户 ~/.ssh/authorized_keys下，如无该文件则新建一个  
注意权限配置  
chmod 700 ~/.ssh  
chmod 600 ~/.ssh/authorized_keys  

3.vscode配置  
a.安装 Remote-SSH插件  
b.进行VSCode远程连接信息配置    
c.配置好连接名、ip、端口（不填默认22）、登录用户名和本地电脑的私钥路径，然后保存

## 常用组件

名称: Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code
ID: MS-CEINTL.vscode-language-pack-zh-hans
说明: Language pack extension for Chinese (Simplified)
版本: 1.110.2026030409
发布者: Microsoft
VS Marketplace 链接: https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans

名称: C/C++
ID: ms-vscode.cpptools
说明: C/C++ IntelliSense, debugging, and code browsing.
版本: 1.31.0
发布者: Microsoft
VS Marketplace 链接: https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools


名称: Remote - SSH
ID: ms-vscode-remote.remote-ssh
说明: Open any folder on a remote machine using SSH and take advantage of VS Code's full feature set.
版本: 0.122.0
发布者: Microsoft
VS Marketplace 链接: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh


名称: GitLens — Git supercharged
ID: eamodio.gitlens
说明: Supercharge Git within VS Code — Visualize code authorship at a glance via Git blame annotations and CodeLens, seamlessly navigate and explore Git repositories, gain valuable insights via rich visualizations and powerful comparison commands, and so much more
版本: 17.11.0
发布者: GitKraken
VS Marketplace 链接: https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens


名称: Adam's Toolbar
ID: AdamAnand.adamstool
说明: Creates toolbar to help Visual Studio Code users.
版本: 0.1.8
发布者: Adam
VS Marketplace 链接: https://marketplace.visualstudio.com/items?itemName=AdamAnand.adamstool


Name: clangd
Id: llvm-vs-code-extensions.vscode-clangd
Description: C/C++ completion, navigation, and insights
Version: 0.4.0
Publisher: llvm-vs-code-extensions
VS Marketplace Link: https://marketplace.cursorapi.com/items/?itemName=llvm-vs-code-extensions.vscode-clangd


Name: CMake Tools
Id: ms-vscode.cmake-tools
Description: Extended CMake support in Visual Studio Code
Version: 1.22.28
Publisher: ms-vscode


## C/C++ 与 clangd 对比使用


