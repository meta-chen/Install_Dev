Anaconda换清华源

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/conda 

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/

config --set show_channel_urls yes
```

安装Pytorch

使用

[官网]: https://pytorch.org/get-started/locally/

命令

```
conda install pytorch torchvision cudatoolkit=10.0 -c pytorch
```

但是这里一定要注意，去掉

```
-c pytorch
```

，安装的时候才会默认从清华源下载相应的包，因此这里用命令行:

```
conda install pytorch torchvision cudatoolkit=10.0
```

