```bash
ls /lib/systemd/system/
```

> 找到 rc-local.service 文件。

```bash
vim /lib/systemd/system/rc-local.service
```
加入 [install] 部分：
```bash
[Install]
WantedBy=multi-user.target
Alias=rc-local.service
```

```bash
sudo chmod 777 /lib/systemd/system/rc-local.service
```

```bash
vim /etc/rc.local
```
加入：
```bash
#!/bin/sh
/sbin/iptables -t nat -I PREROUTING -s 192.168.132.0/24 ! -d 192.168.132.0/24 -j MASQUERADE
/sbin/iptables -I FORWARD -s 192.168.132.0/24 -j ACCEPT
/sbin/iptables -I FORWARD -d 192.168.132.0/24 -j ACCEPT
echo 1 > /proc/sys/net/ipv4/ip_forward
```

```bash
sudo chmod +x /etc/rc.local
```

```bash
ln -s /libsystemd/system/rc.local.service /etc/systemd/system/
```