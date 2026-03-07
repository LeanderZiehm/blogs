# Network Devices



in current network, discover all hosts:

```
arp-scan -l
```

or

```
sudo nmap -p- -sS -sV -O -A -T4
```

get all open ports:

```
sudo nmap -sn 100.100.100.0/24
```

