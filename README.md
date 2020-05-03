# macOSX-isatap
configure ipv6 on maxOSX even behind a NAT

``` bash
#!/usr/bin/env bash
LOCAL_IP4=$(sudo ifconfig en0 | grep inet | grep -v inet6 | awk '{print $2}')
PUBLIC_IP4="$(curl http://ident.me)"
REMOTE_IP4="166.111.21.1"

LINK_PREFIX="fe80::200:5efe"
GLOBAL_PREFIX="2402:f000:1:1501:200:5efe"

echo NAT: ${LOCAL_IP4}, Public: ${PUBLIC_IP4}

sudo route delete -inet6 default
sudo ifconfig gif0 destroy

sudo ifconfig gif0 create
sudo ifconfig gif0 tunnel ${LOCAL_IP4} ${REMOTE_IP4}

sudo ipconfig set gif0 MANUAL-V6 ${GLOBAL_PREFIX}:${LOCAL_IP4} 64
sudo route add -inet6 ::/0 -interface gif0
```


``` bash
ping6 ipv6.tsinghua.edu.cn
```
