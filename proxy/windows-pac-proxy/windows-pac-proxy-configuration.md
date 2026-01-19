# 准备Pac脚本

create file with name `proxy.pac`
```
function FindProxyForURL(url, host) {
    // 你的代理，格式需要是 主机名:端口，主机名之前不能是 http://服务器:端口，不能有http://
    var PROXY = "PROXY your-proxy:8080";

    // 域名白名单：写主域名即可（会覆盖其所有子域名）
    var allowDomains = [
        "baidu.com"
    ];

    // 命中白名单：走代理
    for (var i = 0; i < allowDomains.length; i++) {
        var d = allowDomains[i];

        // 精确命中 host == d，或命中子域名 *.d
        if (host === d || dnsDomainIs(host, "." + d)) {
            return PROXY;
        }
    }

    // 其他全部直连
    return "DIRECT";
}
```

# 准备一个HTTP服务器 
windows的pac代理不支持本地文件，需要http服务
快速验证：python -m http.server 8080 --bind

# 配置windows的代理设置
略

