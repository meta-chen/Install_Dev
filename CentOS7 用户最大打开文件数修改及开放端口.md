# CentOS7 用户最大打开文件数修改及开放端口

### 最大打开文件数

> 在Centos7系统中，使用Systemd替代了之前的SysV。/etc/security/limits.conf文件的配置作用域缩小了。/etc/security/limits.conf的配置，只适用于通过PAM认证登录用户的资源限制，它对systemd的service的资源限制不生效。因此登录用户的限制，通过/etc/security/limits.conf与/etc/security/limits.d下的文件设置即可。
> 链接：https://www.jianshu.com/p/1d02c97f3573

需要修改两个文件：`/etc/security/limits.conf` 与 `/etc/security/limits.d`

```bash
用户 soft nofile 1048576
用户 hard nofile 1048576
```



### 开放端口

```bash
firewall-cmd --query-port=6666/tcp
/// 查询指定端口是否开放

firewall-cmd --zone=public --add-port=5672/tcp --permanent   # 开放5672端口

firewall-cmd --zone=public --remove-port=5672/tcp --permanent  #关闭5672端口

firewall-cmd --reload   # 配置立即生效
```

Note: 可能需要root 权限，在命令前加上`sudo` 即可



### 服务器需要重启的问题

一般很多操作需要重启，其实并不一定需要真正重启服务器，只需要重登当前用户即可，断开连接重连