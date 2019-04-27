---
layout:     post   				    # 使用的布局（不需要改）
title:      Ubuntu离线部署环境 				# 标题 
subtitle:   Hello World, Hello Blog #副标题
date:       2019-04-27 				# 时间
author:     Tao					# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 部署
---

在离线环境下安装软件一般需要源码自己编译安装，有时各种依赖关系会比较麻烦。比如说安装**Apache2**,**PHP**等软件时需要安装一堆依赖包，很容易出错。
以在ubuntu16.04LTS系统为例，说明如何部署一些常用的软件以及python依赖环境等。

---
# 离线apt安装
apt本质上是包管理工具，实际安装还是下载的是各种deb包安装的，只是可以自动获取各种依赖关系，下载对应的deb包，然后调用dpkg安装deb包。
apt似乎默认安装完成后会删除deb包（可能有配置文件可以设置不删除，不知道在哪儿），不过有参数提供了仅下载deb包而不安装的功能。
```
sudo apt install --download-only apache2
```
下载的文件在 **/var/cache/apt/archives** 目录下，都是一些\.deb包，相关的依赖也在文件夹下。
**为了有效安装全部的依赖，避免之前安装过一些包，建议开一个虚拟机从头开始，把所有需要的软件用上面的命令执行一遍，下载到所需的所有依赖。**
下载完成后将 **/var/cache/apt/archives**文件夹拷贝到一个文件夹并打包且生成依赖关系，这里以 **/apps**为例。
```
sudo cp -r /var/cache/apt/archives /apps
sudo chmod 777 -R /apps
sudo dpkg-scanpackages /apps/ /dev/null | gzip >/apps/Packages.gz # 生成依赖关系
sudo mv /apps/Packages.gz /apps/archives/
```

之后可以将这个文件夹**apps**拷贝到离线的机器上，设置这个文件夹里的内容为apt的下载源，这样就可以既是离线的，又可以享受到自动依赖安装。
```
# 把apps还是拷贝到/apps位置
sudo mv /etc/apt/source.list /etc/apt/source.list.bak # 备份源地址文件
sudo echo "deb file:/// /apps/archives/" > /etc/apt/source.list  #或者vim手动修改
sudo apt-get update --allow-insecure-repositories # 更新源到我们复制的包
然后就可以正常使用了
sudo apt-get install XXXXX
最后记得恢复改掉的源
mv /etc/apt/source.list.bak /etc/apt/source.list
```

# 离线pip安装
**pip**也是类似的方法。**pip**下载的是一些wheel文件，可以通过提前下好，建立requirements.txt文件,然后拷贝到离线机器安装。
比较简单,直接上代码备用.
 ```
#以安装Flask为例
pip install Flask
pip freeze > requirements.txt #pip list中的内容 包括包名和版本
pip install --download /data/py-packages -r requirements.txt #下载wheel文件到目录下

拷贝txt和py-packages到离线机器上
pip install --no-index -f=/data/py-packages -r requirements.txt
```
**Success!**

# docker部署
docker功能强大,适合部署,[官方教程](https://docs.docker.com/)很多。具体的在[其他教程]()中总结。
总之就是打包安装好环境的容器为镜像给别人使用，别人只需要docker安装包以及打包好的镜像文件即可一键部署。
