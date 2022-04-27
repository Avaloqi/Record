## 环境工具venv

创建新环境：
    python -m venv new_venv  
开启环境： 
    . new_venv/vin/activate  
关闭环境： 
    deactivate

## ConfigParser模块

[python的ConfigParser模块](https://blog.csdn.net/miner_k/article/details/77857292)

## Pyinstaller
把.py脚本打包成.exe可执行文件的库

### 安装
    pip install pyinstaller

### 靠谱点儿的资料
[PyInstaller安装和使用教程](http://c.biancheng.net/view/2690.html)  
[PyInstaller用法](https://blog.csdn.net/jirryzhang/article/details/78881512)  
[pyinstaller系列之五](https://blog.csdn.net/u012219045/article/details/114841287)  

### Mac打包踩坑
使用mac打包时带 -w 命令时会有问题，应用无法正常使用， 无配置文件无弹窗有机会能用  
暂未找到在mac上可打包成不带cmd运行的方式  

应用运行需要额外配置文件时可使用 --add-data <SRC:DEST> 方式添加  
SRC - 当前文件位置  
mac上要使用 :  
DEST - 打包到的位置  
比如命令 
    pyinstaller -F splicing.py --add-data="cfg.ini:."  


## OpenCV 和 Numpy
可用来进行图像处理

### 安装
    pip install opencv-contrib-python  

这个库包含cv和numpy
    import cv2
    import numpy as np

### 资料
[Open_CV系列](https://skylarkprogramming.blog.csdn.net/article/details/123751380)


## Tkinter
python用来写界面的库

### 资料
[Tkinter](http://c.biancheng.net/tkinter/root-windows.html)
