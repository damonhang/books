# nginx学习

### 安装
下载
[nginx: download](http://nginx.org/en/download.html)

> 如果出现 ::make: *** 没有规则可以创建“default”需要的目标“build”:: 需要安装依赖 
> `yum install pcre-devel zlib zlib-devel openssl openssl-devel` 
> 安装成功后配置环境变量
``` shell
export NGINX_HOME=/usr/local/nginx
export PATH=$PATH:$NGINX_HOME/sbin
```
常用命令
``` shell
# 查看nginx配置文件位置
nginx -t

```


