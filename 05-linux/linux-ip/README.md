# Linux Networking





## Firewall

fedora:

```
sudo systemctl stop firewalld
sudo systemctl start firewalld
```

```
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --zone=nm-shared --list-all
sudo firewall-cmd --zone=nm-shared --add-port=8000/tcp --permanent
sudo firewall-cmd --reload
```



## NetworkManager CLI

```
nmcli connection show
sudo nmcli con add type ethernet ifname enp0s31f6 con-name "Direct-KVM"
sudo nmcli con mod "Direct-KVM" ipv4.addresses 192.168.1.100/24
sudo nmcli con mod "Direct-KVM" ipv4.method shared
sudo nmcli con down "Direct-KVM" && sudo nmcli con up "Direct-KVM"
```





***



## NAT







***



```
sudo sysctl -w net.ipv4.ip_forward=1
```

```
echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/99-hotspot.conf
sudo sysctl --system
```

```
sudo iptables -t nat -A POSTROUTING -o UPSTREAM_IFACE -j MASQUERADE
```

```
#!/usr/bin/env bash

echo "Setting IPv6 preference in system..."

sudo sysctl -w net.ipv6.conf.all.disable_ipv6=0
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=0

# Prefer IPv6 over IPv4 when both exist
echo "net.ipv6.conf.all.accept_ra=2" | sudo tee /etc/sysctl.d/40-ipv6.conf
echo "net.ipv6.conf.default.accept_ra=2" | sudo tee -a /etc/sysctl.d/40-ipv6.conf

sudo sysctl --system

echo "IPv6 preference enabled."
```

On PC A:

```
sudo ip addr add 192.168.10.1/24 dev eth0
sudo ip link set eth0 up
```

On PC B:

```
sudo ip addr add 192.168.10.2/24 dev eth0
sudo ip link set eth0 up
```

Test:

```
ping 192.168.10.2
```

## Mininet NAT

forward:

```
sudo sysctl -w net.ipv4.ip_forward=1
```

```
sudo ip route add 10.0.2.0/24 dev s1-eth1
```

```
ip route get 10.0.2.2
```

confim:

```
sudo ip addr add 10.0.2.254/24 dev s1-eth1
```

nat:

```
sudo iptables -t nat -A POSTROUTING -s 10.0.2.0/24 -o wlp4s0 -j MASQUERADE
```

```
sudo iptables -A FORWARD -i wlp4s0 -o s1-eth1 -j ACCEPT
sudo iptables -A FORWARD -o wlp4s0 -i s1-eth1 -j ACCEPT
```





## Linux Proxies&#x20;

<table><thead><tr><th>Name</th><th width="331.7890625">Link</th><th></th></tr></thead><tbody><tr><td>TinyProxy</td><td><a href="https://github.com/tinyproxy/tinyproxy">https://github.com/tinyproxy/tinyproxy</a></td><td></td></tr><tr><td>Squid Proxy</td><td><a href="https://github.com/squid-cache/squid">https://github.com/squid-cache/squid</a></td><td></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

<pre><code><strong>sudo vim /etc/apt/apt.conf.d01proxy
</strong></code></pre>

```
Acquire::http::Proxy
"http://192.168.0.101:8888";
```

## IP Command

```
ip a
ip neigh
ip route
```

## ARP

```
arp -a
```

## Ping&#x20;

```
ping -i 0.01 10.0.0.2
```





