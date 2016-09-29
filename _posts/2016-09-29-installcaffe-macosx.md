---
layout: post
category: "programming"
title:  "第二篇"
tags: [Caffe,Pycaffe,Deep Learning,Machine Learning, Mac OSX]
---


最近在Mac下安装了Caffe，顺便学习下整套Caffe的框架。文中记录了安装过程及遇到的问题和解决方案。电脑配置情况如下:  
* 操作系统：Mac OS X EI Capitan  Version:10.11.6  
* 无独立显卡，无GPU。  
Caffe官方OSX安装教程：(_http://caffe.berkeleyvision.org/install\_osx.html_)  
---

**步骤一：安装前系统相关软件准备**  

1. Homebrew 安装  
    使用mac开发的人，一般都安装了安装包管理器Homebrew吧。如果没有，那么请先安装：(_http://brew.sh/_)  
2. 安装Xcode
    同上，默认你已经在用Xcode了，如果没有，请直接通过Appstore安装，或：
_https://developer.apple.com/download/_  
3. Anaconda Python
    _https://www.continuum.io/downloads_  选择需要的版本。Caffe官方网站：_http://caffe.berkeleyvision.org/install_osx.html_ 中推荐安装anaconda python. 当然，也可以使用自己系统已安装好的python,但是需要手动修改相关的python依赖库。安装完成后，设置anaconda python的环境变量：找到已安装的anaconda bin文件夹的位置，命令如下:
    ![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-207a65725ae74d8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   
    ```
     export PATH="(你自己的安装目录)/anaconda/bin:$PATH"
    ```  
    记住修改Caffe安装包中的Makefile.config中anaconda python的相关配置
4. 安装 BLAS - Intel MKL
    MKL 为数学计算库，可以加速CPU的计算。在校大学生可通过intel 官网免费申请：_https://software.intel.com/en-us/qualify-for-free-software/student_
    安装之后进行环境变量设置，默认安装路径:_/opt/intel/_记住修改Caffe安装包中的Makefile.config中MKL相关配置  
5. CUDA（无需安装）
    在无GPU的情况下，不需要安装CUDA相关软件。

**步骤二：安装Caffe相关依赖 命令如下: **
本文使用的是Caffe提供的python接口，即需安装pycaffe相关依赖.

```shell
brew install --fresh -vd snappy leveldb gflags glog szip lmdb homebrew/science/opencv
brew install --build-from-source --with-python --fresh -vd protobuf
brew install --build-from-source --fresh -vd boost boost-python
```

**步骤三：从命令端下载Caffe安装包**  

```shell
git clone https://github.com/BVLC/caffe.git
cd caffe
```

**步骤四：修改配置文件Makefile.config**

```shell
cp Makefile.config.example Makefile.config
```

打开配置文件 vim Makefile.config  
1) 修改为CPU模式  
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-5a18a0c0e49aae54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
2）修改BLAS 及MKL相关依赖文件路径  
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-2ded720f65ea8f64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
3）修改OPENCV相关路径  
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-fdc9e210dd586114.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**步骤五：安装Caffe**  

```shell
mkdir build
cd build
cmake -DCPU_ONLY=ON ..
make all -j8
make test
make runtest
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-99e92cc8b7cffc32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

在“make runtest“时，出现出错如下：  
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-46d1254665c07079.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  错误显示无法加载libhdf5库，解决方案：手动安装hdf5

```shell
brew install hdf5
```

重新make runtest, 通过。  

**步骤五：pycaffe安装**  

```shell
make pycaffe
make pytest
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-e1a67372e5c375ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
可能会出现以上错误：protobuf安装出问题。解决方案：重新安装protobuf

```
pip install protobuf
```

重新make pytest, 通过：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-7d64ffe54f118109.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**步骤六：设置pycaffe 环境变量**  
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-4ffefb96bfdd8ee5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
其中标红色为安装路径  备注：mac下环境变量设置永久生效，需在文件/.bash_profile 中设置（更多细节请google） 若只是在终端export只是临时生效。  

**步骤七：在python中测试caffe接口，成功**  
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/3032150-849176b49288d70e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
**参考:**  
_http://caffe.berkeleyvision.org/_    
_http://hoondy.com/2015/04/03/how-to-install-caffe-on-mac-os-x-10-10-for-dummies-like-me/_   