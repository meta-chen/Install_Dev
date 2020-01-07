## Elasticsearch·head插件的安装与配置

### 1.安装node.js

#### 1.1、通过官网下载二进制安装包

https://nodejs.org/en/download/

![img](E:\PycharmProjects\SomeTips\pictures\wlpvp63xq2.png)
  

选择对应的版本，右键复制下载链接，进入linux目录，切换到要安装目录的磁盘。这里我们软件安装在/usr/local目录下，执行如下命令下载安装包

```javascript
cd /usr/local/
wget https://nodejs.org/dist/v10.16.3/node-v10.16.3-linux-x64.tar.xz
```

#### 1.2、解压安装包

```javascript
tar -xJf node-v10.16.3-linux-x64.tar.xz 
```

#### 1.3、配置环境变量

```javascript
vi /etc/profile
```

在文件最后面追加node.js环境变量

```javascript
export NODE_HOME=/usr/local/node-v10.16.3-linux-x64
export PATH=$NODE_HOME/bin:$PATH
```

#### 1.4、重新加载配置文件并验证是否安装成功  

```javascript
source /etc/profile
```

```javascript
node -v
npm -v
```

### 2.head插件的安装与配置

安装head插件需要下载安装包，但是通过git下载，所以我们首先需要在系统安装git插件，然后才能进行下载安装【不一定，只要能将安装包下载下来均可】

#### 2.1-2.2 安装git 略过

#### 2.3、下载并安装head插件

```javascript
git clone git://github.com/mobz/elasticsearch-head.git
cd elasticsearch-head/
npm install
```

可能会报错：

```bash
PhantomJS not found on PATH
Downloading https://github.com/Medium/phantomjs/releases/download/v2.1.1//phantomjs-2.1.1-linux-x86_64.tar.bz2
Saving to /tmp/phantomjs/phantomjs-2.1.1-linux-x86_64.tar.bz2
...
Error making request.
Error: connect ETIMEDOUT 13.229.188.59:443
    at TCPConnectWrap.afterConnect [as oncomplete] (net.js:1129:14)
```

链接超时，总之就是下载失败，所以需要手动下载指定包到指定文件夹下

重新运行`npm install`

#### 2.4、配置elasticsearch，允许head插件远程访问

```javascript
cd elasticsearch-6.5.1/config/
vi elasticearch.yml
```

在配置文件末尾添加如下内容，重新启动elasticsearch服务

```javascript
http.cors.enabled: true
http.cors.allow-origin: "*" // 不建议，推荐使用 /http?:\/\/192.168.10.139(:[0-9]+)?/
```

#### 2.5、启动elasticsearch-head服务

```javascript
cd elasticsearch-head/
npm run start
```

