# Ubuntu挂载iso文件和配置apt本地源

项目需要安装升级OpenSSH到新版本，但是项目不能联网，找了很多办法决定搭建一个具有足够基础包的本地源。记录步骤如下：

#### 通过本地iso文件

- 创建目录

```
mkdir /mnt/iso/
chmod -R 777 /mnt/iso
```


将镜像文件ubuntu-16.04.2-server-amd64.iso上传到目录/mnt下

- 挂载iso镜像到目录下

```
#挂载命令
sudo mount -t iso9660 -o loop /mnt/ubuntu-16.04.2-server-amd64.iso  /mnt/iso
```

操作之后，即可将iso文件挂载到/mnt/iso目录下，我们可以查看下/mnt/iso目录已经有ubuntu-16.04.2-server-amd64.iso 的文件了。

- 配置软件源
  将在线源修改本地的apt源地址，这里修改的是/etc/apt/下的sources.list文件，修改之前记得先备份一份（执行命令 ： sudo cp /etc/apt/sources.list /etc/apt/sources.list.back），将文件中的deb在线apt源注释掉，修改完成后保存文件。

- 添加本地软件源

```
sudo apt-cdrom -m -d=/mnt/iso add
```

- 测试本地软件是否已经设置成功

```
sudo apt-get upgrade
```

#### 通过包安装

该方法可能会有很多依赖问题需要解决

**从网站下载deb的包，传入服务器安装。**

下载路径: http://archive.ubuntu.com/ubuntu/pool/
从该pool中找到对应的deb文件下载下来。

```
sudo dpkg -i *.deb
```

