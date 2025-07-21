# Broadcaster_with_SRV
为Broadcaster添加域名SRV记录支持，配合[NAT1 Traversal](https://github.com/Guation/nat1_traversal)的MCBE功能使用。

### 功能概述
此仓库基于原始Broadcaster项目，添加了SRV记录解析支持。当服务器的port设置为0时，程序会尝试查询_minecraft._udp.[你的域名]的SRV记录，并使用记录中的目标地址和端口进行重定向。当服务器的port不为0时，程序行为与原版无差别。

### 与原版Broadcaster的核心差异
|功能|原版Broadcaster|Broadcaster_with_SRV|
|:--|:--:|:--:|
|域名处理|仅解析A/AAAA记录|支持A/AAAA记录 + SRV记录|
|端口配置|必须手动指定端口号|支持port=0触发SRV查询|
|重定向逻辑|固定使用配置中的端口|动态使用SRV记录指定的端口|
|SRV记录支持|❌ 不支持|⚠️ 有限支持（不支持均衡负载）|
|配置示例|{"ip": "mc.example.com", "port": 19132}|{"ip": "mc.example.com", "port": 0}|
|实际连接方式|直连mc.example.com:19132|使用SRV记录的目标主机和端口|

### 常见问题
Q: 如果port=0但域名没有SRV记录会怎样？

A: 程序将回退到标准的A/AAAA记录解析，并使用默认的Minecraft端口(19132)


Q: 是否支持负载均衡？

A: 不支持基于权重的负载均衡，固定使用DNS服务器返回的第一条SRV记录


Q: 如何验证SRV记录配置？

A: 
```shell
$ dig +noall +answer mc.example.com A
mc.example.com.	305	IN	A	xx.xx.xx.xx

$ dig +noall +answer _minecraft._udp.mc.example.com SRV
_minecraft._udp.mc.example.com. 60 IN SRV	10 0 yyyy mc.example.com.
```

### 下载

前往[GitHub Actions](https://github.com/Guation/Broadcaster_with_SRV/actions/workflows/ci.yml)，在最新的runs中找到build-artifact。
