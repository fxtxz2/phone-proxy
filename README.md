# phone-proxy
使用wget获得js文件到本地，然后用nginx代理，并映射本地js文件。

## wget使用
```Shell
wget -x http://user.qyshare.cn/content/lib/vendor/Ponto.js
```
使用-x，可以让wget根据请求创建对于的文件夹目录保存文件

## nginx使用
mac系统brew安装nginx，找到/usr/local/etc/nginx/nginx.conf文件,配置代理和本地映射。在默认基础上面添加如下配置：
```Shell
# 拦截所有请求，进行代理
location / {
  # 配置dns解析
  resolver	223.6.6.6;
  # 保持原始请求头
  proxy_set_header Host	$host;
  # 保持原始请求协议，请求uri，请求参数
  proxy_pass  $scheme://$host$uri$is_args$args;
}s
# 拦截特定请求，并配置本地映射
location ~ (Ponto|contact-list)\.js$ {
  # 注意这里的映射配置的是root目录，即请求的root目录
  root /Users/zhangyalin/Downloads/test;
}
```

## wireshark分析
完成nginx代理和本地文件映射，就可以开始用Wireshark来分析手机的请求了，只要用Wireshark监控nginx代理端口所在的网卡，然后用Wireshark灵活的筛选命令过滤请求就好了。
```Shell
#显示请求的uri包含"user.qyshare.cn"的http封包
http.request.uri contains "user.qyshare.cn"
```
## 参考
[正则表达式](https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)  
[Wireshark DisplayFilters](https://wiki.wireshark.org/DisplayFilters)  
