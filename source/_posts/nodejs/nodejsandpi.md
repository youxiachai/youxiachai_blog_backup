title: 树莓派上的Node.js
date: 2014-08-31
tags: [node,pi]
---

## 前言

最近入手了,最新版本的树莓派 B+, 发现在其上面运行Node.js,实在好玩!

<!--more-->
## 安装Node.js

### 更新最新树莓派系统

```bash
sudo apt-get upgrade
sudo apt-get update
```

### 安装Node.js

如果是以前的教程,可能是叫你去下载源码编译..以树莓派的性能来编译Node.js的源码,应该至少一个小时吧?

幸运的而是官方已经提供了,编译的树莓派二进制包,所以就不要再去下源码编译了..

```bash
wget http://nodejs.org/dist/v0.10.26/node-v0.10.26-linux-arm-pi.tar.gz
tar -xvzf node-v0.10.26-linux-arm-pi.tar.gz
node-v0.10.26-linux-arm-pi/bin/node --version
```

接下来写进我们的环境变量


```bash
vi .bash_profile
```

在里面写入

```bash
PATH=$PATH:/home/pi/node-v0.10.26-linux-arm-pi/bin
```
在运行source 更新
```bash
source .bash_profile
```

现在我们有Node命令了

```bash
node --version
```

忽然,感慨当年先驱者为了在树莓派上运行上Node.js 各种折腾...现在真心方便.

#### 一些意外

如果运行
```bash
sudo npm install -g forever
```
发现,报找不到npm命令,那你需要吧node的运行目录写到sudo运行环境

```bash
sudo vi /etc/sudoers
```
找到
```
Defaults       secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
```
在原来的基础上加上你的node环境

```
Defaults       secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/pi/node-v0.10.26-linux-arm-pi/bin"
```

现在运行,就不会报错了.
```bash
sudo npm install -g forever
```


## 远程控制Led 灯

环境搞定了,接下来就是开始编写我们的控制程序了!

### gpio 库

有兴趣了解树莓派的,可以去下面这个link,了解一下树莓派的gpio口,为什么树莓派能够这么火,其魅力就是这gpio口了.

> https://github.com/rakeshpai/pi-gpio

这个gpio库,更新挺及时的,已经支持B+了.

默认情况下,要访问树莓派的gpio口是需要管理员权限的.所以,我建议在装多一个东西吧

```bash
git clone git://github.com/quick2wire/quick2wire-gpio-admin.git
cd quick2wire-gpio-admin
make
sudo make install
```

对于我们的远程控制一个led灯的软件环境准备已经完成,接下来就开始写Node代码吧.

最终效果可以移步下面这个link观看
> http://www.miaopai.com/show/xwnY1Sgn6IOOlIQLk9cSLw__.htm


### 代码编写

```js
var http = require('http')

var gpio = require('pi-gpio')

var fs = require('fs')

var errHandler = function (err){
    console.log(err);
}

gpio.open(8, "output", function (err) {
    if(err){
        console.log('已经打开');
    }
})

http.createServer(function (req, res){
    if(req.url == '/led/open') {
        res.writeHead(200, {'Content-Type': 'text/plain'});
        gpio.write(8, 1, errHandler);
        res.end('open')
        return;
    }

    if(req.url == '/led/close'){
        res.writeHead(200, {'Content-Type': 'text/plain'});
        gpio.write(8, 0, errHandler);
        res.end('close');
        return;
    }


    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end(fs.readFileSync('index.html'));
}).listen(1984);

console.log('server runn at 1984');

```

你刚刚看到的视频效果,背后的代码不到50行...如果,写好一些.估计可以压缩不到20行..

因为这代码没什么技术可言..所以,今天就到此为止了吧..






