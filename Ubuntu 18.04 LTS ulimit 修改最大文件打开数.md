# Ubuntu 18.04 LTS ulimit 修改最大文件打开数

如果你想增加 ulimit -n 显示的极限值，你可以：

修改 /etc/systemd/user.conf 及 /etc/systemd/system.conf 中如下面这行的配置项（这将处理图形登录）：
        DefaultLimitNOFILE=65535

修改 /etc/security/limits.conf 中如下面这几行（这将处理非图形登录）：

        * soft nofile 65535
        * hard nofile 65535
