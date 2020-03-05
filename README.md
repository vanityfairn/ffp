[![Build Status](https://travis-ci.com/YUX-IO/ffp.svg?branch=master)](https://travis-ci.com/YUX-IO/ffp)
[![Coverage Status](https://coveralls.io/repos/github/YUX-IO/ffp/badge.svg?branch=master)](https://coveralls.io/github/YUX-IO/ffp?branch=master)
[![codebeat badge](https://codebeat.co/badges/52718a21-307b-4f31-a3be-93fa49df77ec)](https://codebeat.co/projects/github-com-yux-io-ffp-master)
[![](https://img.shields.io/docker/pulls/yuxio/ffp.svg?colorB=4AC41C)](https://hub.docker.com/r/yuxio/ffp)
[![](https://shields.beevelop.com/docker/image/image-size/yuxio/ffp/latest.svg) ](https://hub.docker.com/r/yuxio/ffp)


# ffp 
**yet another Flask File Proxy**


### :rocket:QUICK START
**If you can see the little badge here --> [![](https://ffp.yux.io/https://img.shields.io/badge/ffp.yux.io-%E2%9C%94-green.svg)] The proxy is ON.**

Let's say you want to use the Docker Install Script:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```
but you have a "special network envoirment" that you may cannot connet to `https://get.docker.com` propelly. Then you may wanna try this:
```bash
curl -fsSL https://ffp.yux.io/https://get.docker.com -o get-docker.sh
sh get-docker.sh
```
so the script comes to you via the proxy now. But there is another problem you must have noticed already that there are a lot of other resouces from that script, such as `https://github.com/docker......`, `http://ftp.debian.org/......` and they happen to be all unreachble to you. So let's rewrite all urls to the proxy address.
```bash
curl -fsSL https://ffp.yux.io/r/https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

### :neckbeard:RECOMMENDED

**make sure you have**

- docker, `docker --version`
- Domain name
- ssl
- Nginx (or anything you can point your domain to local port 502)

### :pager:INSTALLATION

1. install **ffp** via docker 

```bash
docker run -d --name=ffp \
  -p 127.0.0.1:502:80 \
  --restart=always \
  yuxio/ffp:latest
```

2. Point your domain or subdomain to local `502` port. (reverse proxy)

3. Get the HTTPS work

### :trollface:USAGE

1. **File Proxy**
   - Rewrite `https://get.docker.com` to `https://your.domain.here/https://get.docker.com`
   - Now you get the file passed through the proxy.

2. **Rewritten Script Proxy**          *please note `/r/` in the url*
   - Rewrite `https://get.docker.com` to `https://your.domain.here/r/https://get.docker.com`
   - This will replace all the URL in the script to `https://your.domain.here/URL`, which means you'd get all the resources passing through your proxy.

### :squirrel:OTHER

1. If you don't have a domain, use `docker run -d --name=ffp -p 80:80 yuxio/ffp:latest`, then rewrite `https://get.docker.com` to `http://$IP/https://get.docker.com`, it should work just fine. But you can't use **Rewritten Script Proxy**.
2. If you don't want to use docker, clone this repository then run `pip install -r requirements.txt && python main.py --host=0.0.0.0 --port=$PORT`, choose the port you want. Rewrite `https://get.docker.com` to `http://$IP:$PORT/https://get.docker.com`.



# ffp 
**使用Flask制作的文件代理**


### :rocket:原地开始
**你要是能看见这有个小图标 --> [![](https://ffp.yux.io/https://img.shields.io/badge/ffp.yux.io-%E2%9C%94-green.svg)] 代理网站在线。**

比如你想用 Docker Install Script:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```
但是由于可能存在的特殊网络环境，访问 `https://get.docker.com`并不通畅，试一下这个:
```bash
curl -fsSL https://ffp.yux.io/https://get.docker.com -o get-docker.sh
sh get-docker.sh
```
这样脚本就通过**ffp**访问。但是这就带来了另一个问题，脚本中有些外部资源，像 `https://github.com/docker......`, `http://ftp.debian.org/......` 依然无法访问，这时就需要重写脚本中的外部链接，使所有外部资源都通过**ffp**。
```bash
curl -fsSL https://ffp.yux.io/r/https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

### :neckbeard:推荐食用方法

**确保你有**

- docker环境, 通过`docker --version`查看。
- 域名
- ssl证书
- Nginx，或者其他帮助你反代绑定域名的工具

### :pager:安装

1. 通过docker部署**ffp**

```bash
docker run -d --name=ffp \
  -p 127.0.0.1:502:80 \
  --restart=always \
  yuxio/ffp:latest
```

没有报错的话，此时**ffp**就部署成功了，监听本地的502端口。

2. 将你的域名，或者子域名绑定到本地的502端口。

3. 部署你的HTTPS。

### :trollface:用法

1. **文件代理**
   - 把你想要获取的文件地址前加上你的域名，例如，将 `https://get.docker.com` 改成 `https://your.domain.here/https://get.docker.com`
   - 这样这个`README.md`文件就会通过**ffp**代理，即时文件的原地址在你目前所处的网络环境下无法访问，也可以通过**ffp**正常查看。
  
### :squirrel:其他

1. 如果你没有域名，可以这样部署 `docker run -d --name=ffp -p 80:80 yuxio/ffp:latest`, 然后把你想要访问的链接，例如 `https://get.docker.com` 改成 `http://$IP/https://get.docker.com`, 这样就可以了，但是无法重写脚本中的外部链接。
2. 如果不想使用docker，也可以直接运行，**ffp**目前仅在python-3.7.6版本中测试通过。 `pip install -r requirements.txt && python main.py --host=0.0.0.0 --port=$PORT`, 选择端口，一般使用80端口。 将 `https://get.docker.com` 重写为 `http://$IP:$PORT/https://get.docker.com`.
