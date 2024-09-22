---
title: Reality 协议搭建
date: 2024-09-13
tags:
  - 教程
  - 科学上网
---
# Reality 协议搭建
## Reality 协议介绍
“Reality协议”在翻墙领域通常指的是一种用于突破互联网审查的代理协议。这类协议的主要目的是通过加密通信、伪装流量等手段，帮助用户绕过防火长城（Great Firewall）或其他网络审查工具，从而访问被屏蔽的网站或服务。  
Reality协议通常与 **V2Ray** 项目中的 **Xray-core** 密切相关，它是一种基于 **VLESS** 协议扩展的工具。Xray-core 是 V2Ray 的增强版本，包含了一些新功能和优化，Reality 就是其中的一种新型加密代理协议。Reality协议主要通过伪装与加密相结合的方式工作。服务器和客户端之间通过 VLESS 协议建立连接，然后借助 Reality 的加密与伪装技术，将流量包装成常见的 HTTPS 数据包。由于 HTTPS 流量在网络中非常常见，审查系统难以区分合法的 HTTPS 请求与 Reality 协议的数据包，因此能够有效避开防火墙。
### Reality协议的主要特点：

1. **高隐匿性**：Reality协议通过加密流量，并将其伪装成合法的 HTTPS 流量，规避网络审查工具的检测。这样，即使审查机构通过深度包检测（DPI）来分析网络流量，也难以发现其真实用途。
2. **免配置TLS证书**：不同于传统的 VLESS 或者 VMess 协议，Reality 不需要用户手动配置 TLS 证书。它通过自动化生成和验证来简化操作，同时提升了安全性。
3. **性能优越**：由于流量被伪装成普通的 HTTPS 流量，Reality协议不仅稳定，还能够在高强度的网络审查环境下保持高效的速度和低延迟。
4. **伪装网站支持**：Reality 协议还提供了对伪装网站的支持，使得其流量看起来像正常的互联网流量。这种流量伪装进一步提高了其隐匿性，防止流量被审查者阻断。
## 搭建步骤
### 服务端搭建
- 一键脚本搭建
```
wget -N --no-check-certificate https://raw.githubusercontent.com/Felix-zf/Singbox-Scripts/main/reality.sh && bash reality.sh
```
- 输入1选项，安装Sing-box Reality,.等待安装依赖之后、设置端口号、UUID和回落域名 4.管理命令为：bash reality.sh，可使用6选项修改Reality的配置文件
### 回落域名说明
1.选择回落域名的最低标准为：国外的网站，支持 TLS v1.3、H2 协议，并使用 x25519 证书
```
# 域名推荐
gateway.icloud.com
itunes.apple.com
download-installer.cdn.mozilla.net
airbnb【这个不同的区有不同的域名建议自己搜索】
addons.mozilla.org
www.microsoft.com
www.lovelive-anime.jp

# CDN
Apple:
swdist.apple.com
swcdn.apple.com
updates.cdn-apple.com
mensura.cdn-apple.com
osxapps.itunes.apple.com
aod.itunes.apple.com

Microsoft:
cdn-dynmedia-1.microsoft.com
update.microsoft
software.download.prss.microsoft.com

Amazon:
s0.awsstatic.com
d1.awsstatic.com
images-na.ssl-images-amazon.com
m.media-amazon.com
player.live-video.net

Google:
dl.google.com
```
2.检测方法
- 打开Chrome，进入待测网页。按下F12键，转到“Secure”选项卡。在“Connection”下出现“TLS 1.3，X25519”字样即代表网页支持 TLSv1.3 协议、并且使用的是 x25519 证书
- 转到“Console”选项卡，输入这个命令 window.chrome.loadTimes()，查看 npnNegotiatedProtocol 的值是否为 h2，如果是的话就代表使用的是 H2 协议