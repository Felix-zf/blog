---
title: Hysteria 2 协议搭建
date: 2024-09-13
tags:
  - 教程
  - 科学上网
---
# Hysteria 2 协议搭建
## Hysteria 2协议介绍
Hysteria2 是一个用于构建高效、安全网络通信的协议。它主要用于加速数据传输和增强抗封锁能力，适合用于代理、加速器等网络场景。以下是 Hysteria2 的一些关键特性和概述：
### 基于 QUIC 协议
Hysteria2 是基于 QUIC 协议开发的，而 QUIC 是一个现代化的传输层协议，具有快速连接建立、低延迟、高带宽利用率等优点。QUIC 协议也广泛应用于 HTTP/3。
###  抗干扰和抗封锁
Hysteria2 设计之初就考虑了抗封锁特性，通过其基于 UDP 和 QUIC 的特性，它能够在复杂的网络环境中更加有效地应对封锁、干扰和网络质量不稳定等问题。
###  高性能
Hysteria2 利用 QUIC 协议的优势，提供低延迟、高吞吐量的网络性能。它可以动态调整流量，确保在不同的网络状况下保持稳定、高效的传输体验。
###  隐匿性与安全性
Hysteria2 通过传输层加密（如 TLS）来保障数据的隐私和安全性。同时，它还提供了一定程度的流量混淆，使其不容易被识别和封锁。
### UDP 传输
相较于传统 TCP 代理，Hysteria2 使用了基于 UDP 的传输方式，这使得它在传输效率上具有明显优势，尤其在高丢包或高延迟的网络环境中表现更好。
### 多场景应用
Hysteria2 可以用于多种场景，例如跨国数据加速、游戏加速、视频加速等。它能够提供低延迟和高传输速率的网络连接，非常适合对实时性要求较高的应用场景。 
## 搭建步骤
### 服务端部署
- 更新 VPS 系统安装所需组件  
1. Debian命令  
```
apt update -y
apt install curl sudo -y
```

- Hysteria 官方的一键安装脚本
1. 安装或升级到最新版本 Hysteria 2  
```
bash <(curl -fsSL https://get.hy2.sh/)
```
2. 移除 Hysteria 2  
```
bash <(curl -fsSL https://get.hy2.sh/) --remove
```
- SSL证书申请  
1. 一键脚本
```
wget -N --no-check-certificate https://raw.githubusercontent.com/Felix-zf/ACME-Scripts/main/acme.sh && bash acme.sh
```
2. 手动安装
```
openssl req -x509 -nodes -newkey ec:<(openssl ecparam -name prime256v1) -keyout /etc/hysteria/server.key -out /etc/hysteria/server.crt -subj "/CN=bing.com" -days 36500 && sudo chown hysteria /etc/hysteria/server.key && sudo chown hysteria /etc/hysteria/server.crt
```
## 服务端配置文件
服务端的配置要和客户端状态一致，也就是说，服务端配置的是无域名的配置，客户端也必须选择无域名的配置参数，必须一致，反之一样，服务端配置的是有域名的，那稍后客户端也必须选择有域名的参数
```
listen: :443
 
# 以下 acme 和 tls 字段，二选一
# 有域名部署的选择 acme ，无域名的选择 tls
# 选择 acme，必须注释掉 tls，反之一样
 
acme:
  domains:
    - cn2.bozai.us        # 域名
  email: yourself@email.com   # 邮箱，格式正确即可
 
#tls:
#  cert: /etc/hysteria/server.crt
#  key: /etc/hysteria/server.key
 
auth:
  type: password
  password: 88888888   # 请及时更改密码
 
masquerade:
  type: proxy
  proxy:
    url: https://addons.mozilla.org # 伪装网站
    rewriteHost: true
```
## 客户端配置
### V2rayN
```
server: ip:443
auth: ****
 
#bandwidth:
#  up: 30 mbps
#  down: 90 mbps
  
tls:
  sni: cn2.bozai.us  # 若无域名，请改为 bing.com
  insecure: false    # 若无域名，需要改参数为 true
 
socks5:
  listen: 127.0.0.1:1080
http:
  listen: 127.0.0.1:8080
```
### Android / IOS / MacOS 配置
```
{
  "dns": {
    "servers": [
      {
        "tag": "cf",
        "address": "https://1.1.1.1/dns-query"
      },
      {
        "tag": "local",
        "address": "223.5.5.5",
        "detour": "direct"
      },
      {
        "tag": "block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "geosite": "category-ads-all",
        "server": "block",
        "disable_cache": true
      },
      {
        "outbound": "any",
        "server": "local"
      },
      {
        "geosite": "cn",
        "server": "local"
      }
    ],
    "strategy": "ipv4_only"
  },
  "inbounds": [
    {
      "type": "tun",
      "inet4_address": "198.18.0.1/16",
      "auto_route": true,
      "strict_route": false,
      "sniff": true
    }
  ],
  "outbounds": [
    {
      "type": "hysteria2",
      "tag": "proxy",
      "server": "ip",             //服务器 IP地址
      "server_port": 443,
      "up_mbps": 30,              //上传速率，实际填写，过大会导致流量浪费
      "down_mbps": 90,           //下载速率，实际填写，过大会导致流量浪费
      "password": "**********",   //hysteria2 服务密码
      "tls": {
        "enabled": true,
        "server_name": "cn2.bozai.us",    //若域名搭建，请填写域名，若IP搭建，请填写 bing.com
        "insecure": false                 //若域名搭建，请填写 false，若IP搭建，请填写 true
      }
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "block",
      "tag": "block"
    },
    {
      "type": "dns",
      "tag": "dns-out"
    }
  ],
  "route": {
    "rules": [
      {
        "protocol": "dns",
        "outbound": "dns-out"
      },
      {
        "geosite": "cn",
        "geoip": [
          "private",
          "cn"
        ],
        "outbound": "direct"
      },
      {
        "geosite": "category-ads-all",
        "outbound": "block"
      }
    ],
    "auto_detect_interface": true
  }
}
```

## Hysteria 2 相关命令
```
systemctl start hysteria-server.service  //启动Hysteria2
systemctl stop hysteria-server.service  //停止Hysteria2
systemctl enable hysteria-server.service  //设置开机自启
systemctl restart hysteria-server.service  //重启Hysteria2
systemctl status hysteria-server.service  //查看状态
journalctl -u hysteria-server.service  //查看日志
```