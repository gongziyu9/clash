# clash
port: 7890
socks-port: 7891
redir-port: 7892
mixed-port: 7893
allow-lan: true
mode: rule
log-level: info
ipv6: false
external-controller: 127.0.0.1:9090

# Clash for Android 兼容设置
clash-for-android:
  append-system-dns: false

profile:
  tracing: true

experimental:
  sniff-tls-sni: true

dns:
  enable: true
  ipv6: false
  listen: 0.0.0.0:1053
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - 223.5.5.5
    - 119.29.29.29
    - 8.8.8.8
    - 1.1.1.1

proxies: [] # 订阅获取节点

proxy-groups:
  - name: "🚀 选择代理"
    type: select
    proxies:
      - "♻️ 自动选择"
      - "🌍 负载均衡"
      - "🎯 直连"
      - "🔰 手动选择"

  - name: "♻️ 自动选择"
    type: url-test
    proxies: []
    url: "http://www.gstatic.com/generate_204"
    interval: 300

  - name: "🌍 负载均衡"
    type: load-balance
    proxies: []
    url: "http://www.gstatic.com/generate_204"
    interval: 300
    strategy: consistent-hashing

  - name: "🎯 直连"
    type: select
    proxies:
      - "DIRECT"

  - name: "🔰 手动选择"
    type: select
    proxies: []

rule-providers:
  gfw:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/gfw.txt"
    path: ./ruleset/gfw.yaml
    interval: 86400

  cn:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/cn.txt"
    path: ./ruleset/cn.yaml
    interval: 86400

  streaming:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/streaming.txt"
    path: ./ruleset/streaming.yaml
    interval: 86400

  streaming-ip:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/streaming-ip.txt"
    path: ./ruleset/streaming-ip.yaml
    interval: 86400

rules:
  - RULE-SET,gfw,🚀 选择代理
  - RULE-SET,streaming,🚀 选择代理
  - RULE-SET,streaming-ip,🚀 选择代理
  - RULE-SET,cn,🎯 直连
  - GEOIP,CN,🎯 直连
  - MATCH,🚀 选择代理
