---
title: Let's Encrypt Https
date: 2018-06-01 10:15:59
tags:
---
## [Let's Encrypt](https://letsencrypt.org/getting-started/)

该网站提供免费的https证书，有效期是三个月，到期可更新。
[获取证书方法](https://certbot.eff.org/lets-encrypt/ubuntuxenial-other),  以Ubuntu 16.04为例:

* 安装certbot：
```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

* 获取证书：

   `sudo certbot certonly --standalone -d example.com -d www.example.com`
   
   域名要提前指向所在主机的公网IP地址.

* 更新证书(--dry-run只是测试下，正真更新的时候要去掉):
 
  　`sudo certbot renew --dry-run`
<!-- more -->
## [acme.sh](https://github.com/Neilpang/acme.sh)
这个项目的出现，又使得获取证书更简单了，感谢开源社区~~~

* 在线安装:

  `curl https://get.acme.sh | sh` 
  
  Or:

  `wget -O -  https://get.acme.sh | sh`
* [获取证书](https://github.com/Neilpang/acme.sh/tree/master/dnsapi):

    ### DNSPod:

    首先登录域名服务商的控制台，获取API Key和 ID
    ```
    export DP_Id="1234"
    export DP_Key="sADDsdasdgdsf"
    ```
    然后申请证书:

    `acme.sh --issue --dns dns_dp -d example.com -d www.example.com`

    最后DP_Id和DP_Key会被保存在`~/.acme.sh/account.conf`,下次更新证书的时候就不要export了。

    ### Aliyun
    
    同样获取API Key
    ```
    export Ali_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
    export Ali_Secret="jlsdflanljkljlfdsaklkjflsa"
    ```
    申请证书:

    `acme.sh --issue --dns dns_ali -d example.com -d www.example.com`

* 更新证书:
  
  官方文档说，没有必要手动更新，所有的证书会每隔60天自动更新。因为你在安装acme.sh的时候已经默认添加了一个系统的定时任务了。
  
  或者也可以强制手动更新：
  
  `acme.sh --renew -d example.com --force`


## [Caddy](https://caddyserver.com/):

这个项目就更方便了，你只要写一次配置，以后就不用管证书的事情了，每次自动更新。

### [下载地址](https://github.com/mholt/caddy/releases)
选择相应的os，下载完解压就能用。命令也很简单，直接看帮助文档就OK.

### 配置文件[Caddyfile](https://caddyserver.com/docs/caddyfile):
这个语法相比nginx真是太简单了，通俗易懂，功能强大。

```
http://colachg.com {
    log /logs/access.log
    redir https://colachg.com{uri}
}

https://colachg.com {
    log stdout
    root /home/colachg/index.html
}
```

上面这个就是Caddyfile的内容，很语义化吧？