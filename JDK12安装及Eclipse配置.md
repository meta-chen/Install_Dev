# JDK12安装及Eclipse配置

- ## JDK安装

JDK下载略过

...

JDK12并未自带jre，需要手动生成，使用命令;

进入jdk文件夹下，如果安装在C盘，需要以管理员启动cmd:

```
bin\jlink.exe --module-path jmods --add-modules java.desktop --output jre
```

配置环境变量，略



- ## Eclipse 配置 jdk

进入eclipse界面，依次点击 Window -- Preferences -- Java -- Installed JREs-add

安装结束后，如果需要能够查看源代码的话，再次点击Edit:

选择 Source Attachment

在lib文件夹下选择src.zip压缩包