- 因需要搭建了多个版本的Tensorflow环境，考虑到速度,选择GPU版。在网上找了很多资料，受益颇多。看到网上资料关于使用RTX2060显卡，在windows环境下安装tensorflow-gpu的较少，且有不少是使用cuda9.2，且使用的是大神自已编译修改过后的tensorflow版本（whl文件），结果走了不少弯路。由此想记下搭建过程，作为日后的参考。
# 一、 机器配置
- win10 64bit
- RTX2060

# 二 文件准备
- VS2017，官网社区版（2012，2013，2015也支持）
- NVIDA驱动（最好版本是385.54版本，不知为何所使用的机器自动安装了最新版驱动）。
- CUDA9.0，官网下载地址（注意目前官网显示是支持到CUDA9.0）
- cuDNN V7.3.0.29，官网下载，进去后可看历史版本,下载需注册（需要注意与cuda的对应关系）
- Anaconda3-5.0.1，清华镜像下载（注意使用目前的tensorflow版本还不支持python3.7及3.8，不要从官网下载Anaconda的最新版本）

# 三 安装过程
## 1. VS2017、Anaconda
VS2017中只需安装C++组件，见下图（引用自参考1），一路默认就行。 
## 2. CUDA9.0安装
- (1) 选择对应的版本进行下载，可选择在线安装版和离线安装版，请随意。 
最好使用管理员权限进行安装，一路默认就行
注意这里如果在系统检查环节出现“This graphics driver could not find compatible graphics hardware....”
选择继续，这里是因为电脑所安装的驱动版本高于CUDA自带驱动版本号。
直接点击继续->同意并继续->自定义（高级）->只选择CUDA进行安装

- (2) 验证CUDA安装是否成功：

打开命令提示符，输入：nvcc -V 
出现如下类似信息： nvcc：NVIDIA(R) Cuda compiler driver 以及相应版本信息即表示安装成功

## 3、修改环境变量
将下面四个路径加入到环境变量中，注意要换成自己的安装路径：

C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0

C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\bin

C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\lib\x64

C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\libnvvp


## 4.解压cudnn-9.0-windows10-x64-v7，将文件夹里内容拷贝至安装CUDA对应的文件夹下
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0


## 5.在Anaconda中新建环境，并使用“pip install tensorflow-gpu==1.x.0” 来选择自己所要安装的版本，亲测可安装1.5.0、1.9.0、1.11.0版本

## 6.测试安装是否成功
a.查看是否使用GPU
``` 
import tensorflow as tf
tf.test.gpu_device_name()
```

若使用gpu版本会显示
'/device:GPU:0'

b.查看在使用哪个GPU
```
from tensorflow.python.client import device_lib
device_lib.list_local_device()
```

- 安装好只是开始，后面还有很多坑要踩，加油！

参考

[Win10 安装Tensorflow-GPU版教程](https://blog.csdn.net/ygjustgo/article/details/78883981)

[Win10 Anaconda下Tensorflow-GPU环境搭建详细教程](https://www.cnblogs.com/guoyaohua/p/9265268.html)

[Nvidia官网](https://developer.nvidia.com/rdp/cudnn-archive)

