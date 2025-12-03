### vscode 远程开发+ssh免密 环境配置
1.windows本地配置  
powershell 打开，输入ssh-keygen 回车两次直到命令结束，生产RSA密钥对  
生成的文件 id_rsa为私钥，本地保留，id_rsa.pub为公钥，后续复制到远程服务器上  

2.远程服务器配置  
将上面的id_rsa.pub加入到远程服务器对应的用户 ~/.ssh/ authorized_keys下，如无该文件则新建一个  
注意权限配置  
chmod 700 ~/.ssh  
chmod 600 ~/.ssh/authorized_keys  

3.vscode配置  
a.安装 Remote-SSH插件  
b.进行VSCode远程连接信息配置    
c.配置好连接名、ip、端口（不填默认22）、登录用户名和本地电脑的私钥路径，然后保存  
