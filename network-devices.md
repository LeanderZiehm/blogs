# Network Devices



options to discover all hosts in the current network:

```
arp-scan -l
```

```
sudo nmap -sn 100.100.100.0/24
```

{% hint style="info" icon="thought-bubble" %}
nmap requires sudo and you specifing the network ip, favoring the use of arp-scan for host discovery
{% endhint %}



get all open ports of device with it's ip:

```
sudo nmap -p- -sS -sV -O -A -T4 100.100.100.50/24
```

