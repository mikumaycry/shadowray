# shadowray

基于centos 8镜像制作，目前版本为v0.5，基于shadowsocks-libev v3.3.5 + v2ray-plugin v1.3.1。

下面是配置示例，涉及的域名、path、IP、密码、加密方式等，请替换为自用的参数。

## 服务器镜像

地址：[https://hub.docker.com/repository/docker/wbuntu/shadowray](https://hub.docker.com/repository/docker/wbuntu/shadowray)

使用方法：

```shell
docker run -d --restart always --network host --name ss-server -v /etc/shadowsocks-libev/config.json:/etc/shadowsocks-libev/config.json docker.io/wbuntu/shadowray:v0.5
```

程序位于caddy或nginx之后，caddy自带TLS证书管理，配置示例如下

```
test.example.com {
    root /usr/share/caddy
    proxy /23336666 localhost:123456 {
        websocket
        header_upstream -Origin
    }
}
```

**/etc/shadowsocks-libev/config.json** 文件示例

```json
{                                             
    "server":"0.0.0.0",                       
    "server_port":123456,                      
    "password":"1njgUnQeKyovK3mTI3um",           
    "timeout":600,                            
    "method":"chacha20-ietf-poly1305",       
    "fast_open":true,                         
    "reuse_port":true,                        
    "no_delay":true,                          
    "mode":"tcp_only",                        
    "plugin": "v2ray-plugin",                 
    "plugin_opts": "server;fast-open;host=test.example.com;path=/23336666"
}                                             
```

## 客户端镜像

地址：[https://hub.docker.com/repository/docker/wbuntu/shadowray-client](https://hub.docker.com/repository/docker/wbuntu/shadowray-client)

使用方法：

```shell
docker run -d --restart always --network host --name ss-local -v /etc/shadowsocks-libev/config.json:/etc/shadowsocks-libev/config.json docker.io/wbuntu/shadowray-client:v0.5
```

**/etc/shadowsocks-libev/config.json** 文件示例

```json
{
    "server":"233.3.66.66",
    "server_port":443,
    "local_port":1080,
    "local_address":"0.0.0.0",
    "password":"1njgUnQeKyovK3mTI3um",
    "timeout":600,
    "method":"chacha20-ietf-poly1305",
    "fast_open":true,
    "reuse_port":true,
    "no_delay":true,
    "plugin": "v2ray-plugin",
    "plugin_opts": "tls;fast-open;host=test.example.com;path=/23336666"
}
```
