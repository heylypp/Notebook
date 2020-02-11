# 安装

1. 安装Ubuntu
   使用U盘进行Ubuntu操作系统的安装：
   参考：

https://jingyan.baidu.com/article/a3761b2b66fe141577f9aa51.html
一开始安装选择"Install Ubuntu"回车后过一会儿屏幕如果显示“输入不支持”，这和Ubuntu对显卡的支持有关，在安装主界面的F6，选择nomodeset，就可以进入下一步安装了。
安装过程略，安装镜像下载地址：
ubuntu.com/download/des
下载：ubuntu-18.04.2-desktop-amd64.iso

2. 安装ssh
   备注：这一步需要到服务器桌面上的命令窗口输入，这一步完成后，就可以用ssh工具远程连接服务器了，本文使用的是XShell。

```
sudo apt-get install openssh-server
```

3. 安装NVIDIA TITAN Xp显卡驱动
   默认安装的显卡驱动不是英伟达的驱动，所以先把旧得驱动删除掉。

```
sudo apt-get purge nvidia*
```

添加Graphic Drivers PPA

```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
```

查看合适的驱动版本：

```
ubuntu-drivers devices
```

图中可以看出推荐的是最新的430版本的驱动，安装该驱动：

```
sudo apt-get install nvidia-driver-430
```

安装完毕后重启机器：

```
sudo reboot
```

重启完毕运行

```
nvidia-smi
```

看看生效的显卡驱动：

4. 安装依赖库

```
sudo apt-get install freeglut3-dev build-essential libx11-dev libxmu-dev libxi-devlibgl1-mesa-glx libglu1-mesa libglu1-mesa-dev
```



5. GCC降低版本
   CUDA9.0要求GCC版本是5.x或者6.x，其他版本不可以，需要自己进行配置，通过以下命令才对GCC版本进行修改。

- 版本安装

```
# 版本安装：
sudo apt-get install gcc-5
sudo apt-get install g++-5
```

- 通过命令替换掉之前的版本

```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 50
```



6. 安装Anaconda和tensorflow、keras和pytorch

**重点：让conda自动安装cuda和cudnn！！！**由于Anaconda可以提供完整的科学计算库，所以直接使用Anaconda来进行相关的安装。
1）安装Anaconda
下载地址：https://www.anaconda.com/download/
这里我们下载Python 3.7 64bit 的linux版本。
安装：

```
bash Anaconda3-2019.03-Linux-x86_64.sh
```

2）更改pip和conda为国内的源
由于国内访问pip和conda比较慢，建议更改为国内的源：
a.更改pip的源为阿里云：

```
mkdir ~/.pip
```



```
cat > ~/.pip/pip.conf << EOF
[global]
trusted-host=mirrors.aliyun.com
index-url=https://mirrors.aliyun.com/pypi/simple/
EOF
```

**备注:conda 国内源都封了，不需要更换源了**

3）在Anaconda中安装Python3.7的虚拟环境
创建一个Python的虚拟环境

```
conda create --name tf python=3.7 #创建tf环境
```

虚拟环境主要命令：

```
source activate tf #激活tf环境
source deactivate tf #退出tf环境
conda remove --name tf --all #删除tf环境（全部删除）
```

4）在Anaconda中安装TensorFlow GPU 1.9

```
conda install tensorflow-gpu==1.9
```

将会自动安装：
cuda，cudnn以及相关的其他组件

5）使用下列代码测试安装正确性
命令行输入：

```
source activate tf
python
```

Python命令下输入以下代码：

```
import tensorflowas tf
hello= tf.constant('Hello, TensorFlow!')
sess= tf.Session()
print(sess.run(hello))
```

没有报错就是配置好了。

6）安装Keras
直接在这个虚拟环境中安装：

```
pip install keras
```

7）安装Pytorch
直接在这个虚拟环境中安装：

```
conda install pytorch torchvision -c pytorch
```

系统会自动安装cuda和cudnn
测试Pytorch是否安装成功：
命令行输入：

```
source activate tf
python
```

python命令下输入以下代码：

```
import torch
print(torch.cuda.is_available())
```

返回True说明安装成功了。

参考

http://blog.csdn.net/weixin_41863685/article/details/80303963