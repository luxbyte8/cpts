# Nmap cheatsheet

## Index
- [home](../index.md)
- [back](./main.md)

---

```console
#> nmap -vv -n -sn --disable-arp-ping -PE -PP -PS21,22,25,80,443,3389 -PA80,443 -PU53,161 -g 53 10.10.10.0/24 -oA host_discover
#> nmap -vv -n -Pn --disable-arp-ping -sS -sV -sC -O 10.10.10.10 -oA initial_tcp
#> nmap -vv -n -Pn --disable-arp-ping --max-retries=2 --osscan-limit -A -p- 10.10.10.10 -oA full_tcp
#> nmap -vv -n -Pn --disable-arp-ping --mtu 32 --data-length 20 -A 10.10.10.10
#> nmap -vv -n -Pn --disable-arp-ping -D RND:5 -A 10.10.10.10
#> nmap -vv -n -Pn -sUV -F --version-intensity 0 10.10.10.10 -oA fast_upd
#> nmap -vv -n -Pn -sUV --host-timeout=15m --version-intensity <0-9> 10.10.10.10 -oA full_udp
#> nmap -vv --dns-servers <DNS,DNS> # if you want reverse dns resolution but not from your DNS server
#> 
```

---

[back](./main.md)