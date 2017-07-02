## 安装opencv
1 解压，mkdir release , cd release
2 cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local/opencv/2.4.9 -D CUDA_GENERATION=Kepler ..  
3 make -j8  
4 sudo make install  
* 中间要修改graphcuts文件，并且拷贝ippcv文件到相应位置，因为下不下来。。  

## 同时装多个版本的opencv

将opencv的不同版本装在如下位置，如/usr/local/opencv/2.4.9和/usr/local/opencv/3.1.0  
编译opencv2.4.9之后，在其指定的目标文件下会有一个python文件夹，里面包含了一个cv.so文件，将这个文件放在主机python库的dist-package文件夹底下即可,opencv3也是同样的道理。
    但是为了使得多版本并存，这里我想在dist-package中分别添加cv249和cv310这两个软链接，分别链接到这两个cv2.so文件上，这样，我们在使用时可以通过import cv249来导入2.4.9版本的opencv。但是事实上不能这么做，因为python会去寻找init<name>这个函数，但是更改名字之后就找不到了。。。。所以还是通过版本控制pkg-config试试吧～  
* 不要使用apt-get install python-opencv，这会让Opencv的版本不可控         

### pkg-config

pkg-config --modversion opencv 查看当前opencv的版本号  

再将下面两行命令输入到~/.bashrc中，来控制当前版本:  
export PKG_CONFIG_PATH=/usr/local/opencv/2.4.8/lib/pkgconfig  
export LD_LIBRARY_PATH=/usr/local/opencv/2.4.8/lib/

