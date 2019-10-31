# Cannot open shared object file: No such file or directory

在Linux系统上的Python开发经常会遇到这个问题，比如```ImportError: libdmdpi.so: cannot open shared object file: No such file or directory``` 



首先我们需要了解该系统中是否存在该文件，可以使用```locate``` 命令进行定位，```locate libdmdpi.so``` ,如果链接文件存在，可以进入其所在目录，也可不进>_<

解决方法：

- 执行 ```export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:. ``` 
  最后的 ```. ```表示把当前目录加入到动态链接库查找的目录中去，```.``` 可以替换为对应地址。

  重新运行

  该方法亲测通过

- 上述方法设置是临时的 另外一种方法：

  sudo vim /etc/ld.so.conf
  添加库路径 如 ./ （表示当前目录）
  添加保存后
  sudo ldconfig
  即可

