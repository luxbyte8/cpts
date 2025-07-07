# Network Enumeration with Nmap

## Index
- [home](../index.md)
- [cheatsheet](./cheatsheet.md)

---

## Nmap Reference
- [Nmap Reference](https://nmap.org/book/man.html)
- [Nmap Online Book](https://nmap.org/book/toc.html)

## Host Discovery
- [Host Discovery Techniques](https://nmap.org/book/host-discovery-techniques.html)
```console
#> nmap -vv -PE -PP -PS443 -PA80 10.10.10.0/24 # default nmap host discovery flags
#> nmap -vv -PE -PP PS21,22,25,80,443,3389 -PA21,80,113,443,10042 -PU53,161,<RND HIGH PORT> --source-port 53 10.10.10.0/24 -oA extended_ping # comprehensive host discovery
```

## Host and Port Scanning
| State             | Description                                           |
| :---              | :---                                                  |
| open              | connection to the port has been established           |
| closed            | packet with RST was received                          |
| filtered          | nmap cannot identify if open or closed                |  
| unfiltered        | only ACK scan; cannot identify if open or closed      |
| open\|filtered    | got no response                                       |
| closed\|filtered  | only Idle scan; cannot identify if closed or filtered |

### Basic Port Scanning
```console
#> nmap -vv -n -Pn --disable-arp-ping -A 10.10.10.10 -oA initial
#> nmap -vv -n -Pn --disable-arp-ping -sSU -sV -sC -pT:80,443,U:53,161 10.10.10.10 # tcp and udp port scan
```

### UDP Scanning
```console
#> nmap -vv -n -Pn -T4 -sUV -F --version-intensity 0 10.10.10.10 # improve udp scan performance
#> nmap -vv --min-hostgroup=100 # try increasing host parallelism
#> nmap -vv --host-timeout=15m  # try skipping slow hosts
```

## Saving Results
```console
#> xsltproc scan.xml -o scan.html # convert nmap xml scan to hmtl page
```

## Nmap Scripting Engine (NSE)

### Categories
- auth
- broadcast
- brute
- default
- discovery
- dos
- exploit
- external
- fuzzer
- intrusive
- malware
- safe
- version
- vuln
```console
#> nmap --script-help "smb-* and safe and discovery"
#> nmap --script-help "safe and (default or discovery)"
#> nmap --script-help "discovery and not intrusive"
```

## Performance
- [Timing and Performance](https://nmap.org/book/man-performance.html)
- [Scan Time Reduction Techniques](https://nmap.org/book/reduce-scantime.html)
- [Timing Templates](https://nmap.org/book/performance-timing-templates.html)
- [Low Level Timing Controls](https://nmap.org/book/performance-low-level.html)
- [Scan Code and Algorithms](https://nmap.org/book/port-scanning-algorithms.html)

## Firewall/IDS Evasion
```console
#> nmap -D RND:5    # use five random decoy IP
#> nmap -D RND,RND,RND,ME,RND # insert yourself
#> nmap -f # fragment the packet
#> nmap --mtu 8 --data-length 20 # fragment into 8 bytes with random 20 byte data 
```

---

[home](../index.md)