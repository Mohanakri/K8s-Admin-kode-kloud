# Pre-requisite DNS

  - Take me to [Lecture](https://kodekloud.com/topic/prerequsite-dns/)
In this introductory lecture on DNS for Linux beginners, the instructor covers fundamental concepts and commands for DNS configuration on Linux hosts:

1. **Introduction to DNS**: The instructor introduces DNS (Domain Name System) and its importance in translating domain names to IP addresses.

2. **Basic Configuration**: Two computers, A and B, are part of the same network and assigned IP addresses. The instructor explains how to assign a name (`db`) to system B (`192.168.1.11`) in the `/etc/hosts` file on system A for easier access.

3. **Name Resolution**: The concept of name resolution is explained, where the system resolves hostnames to IP addresses using the `/etc/hosts` file or a DNS server.

4. **DNS Server Configuration**: The instructor explains how to configure a DNS server on Linux by specifying the DNS server's IP address in the `/etc/resolv.conf` file on each host.

5. **Hostname Resolution Order**: The lecture discusses the order in which the system resolves hostnames, first checking the `/etc/hosts` file and then querying the DNS server, which can be configured in the `/etc/nsswitch.conf` file.

6. **Domain Names**: The lecture delves into the structure of domain names, including top-level domains (TLDs) and subdomains, and explains how they are resolved using DNS servers.

7. **Search Domain Configuration**: The instructor demonstrates how to configure a search domain in the `/etc/resolv.conf` file to append domain names when resolving hostnames.

8. **Record Types**: Different types of DNS records (A, AAAA, CNAME) are explained, along with their purposes.

9. **Testing DNS Resolution**: Tools like `nslookup` and `dig` are introduced for testing DNS resolution.

10. **Upcoming Practice Exercises**: The lecture concludes by mentioning upcoming exercises where students will practice DNS configuration and troubleshooting in a lab environment.

Throughout the lecture, practical examples and real-world scenarios are used to illustrate concepts, making it accessible for beginners.

=================================================================================================


In this section, we will take a look at **DNS in the Linux**

## Name Resolution 

- With help of the `ping` command. Checking the reachability of the IP Addr on the Network.

```
$ ping 172.17.0.64
PING 172.17.0.64 (172.17.0.64) 56(84) bytes of data.
64 bytes from 172.17.0.64: icmp_seq=1 ttl=64 time=0.384 ms
64 bytes from 172.17.0.64: icmp_seq=2 ttl=64 time=0.415 ms

```
- Checking with their hostname

```
$ ping web
ping: unknown host web

```
- Adding entry in the `/etc/hosts` file to resolve by their hostname.

```
$ cat >> /etc/hosts
172.17.0.64  web


# Ctrl + c to exit
```
- It will look into the `/etc/hosts` file.

```
$ ping web
PING web (172.17.0.64) 56(84) bytes of data.
64 bytes from web (172.17.0.64): icmp_seq=1 ttl=64 time=0.491 ms
64 bytes from web (172.17.0.64): icmp_seq=2 ttl=64 time=0.636 ms

$ ssh web

$ curl http://web
```

## DNS

- Every host has a DNS resolution configuration file at `/etc/resolv.conf`.

```
$ cat /etc/resolv.conf
nameserver 127.0.0.53
options edns0
```

- To change the order of dns resolution, we need to do changes into the `/etc/nsswitch.conf` file.

```
$ cat /etc/nsswitch.conf

hosts:          files dns
networks:       files

```

- If it fails in some conditions.

```
$ ping wwww.github.com
ping: www.github.com: Temporary failure in name resolution

```

- Adding well known public nameserver in the `/etc/resolv.conf` file.

```
$ cat /etc/resolv.conf
nameserver   127.0.0.53
nameserver   8.8.8.8
options edns0
``` 
```
$ ping www.github.com
PING github.com (140.82.121.3) 56(84) bytes of data.
64 bytes from 140.82.121.3 (140.82.121.3): icmp_seq=1 ttl=57 time=7.07 ms
64 bytes from 140.82.121.3 (140.82.121.3): icmp_seq=2 ttl=57 time=5.42 ms

```

## Domain Names

![net-8](../../images/net8.PNG)

## Search Domain

![net-9](../../images/net9.PNG)

## Record Types

![net-10](../../images/net10.PNG)

## Networking Tools

- Useful networking tools to test dns name resolution.

#### nslookup 

```
$ nslookup www.google.com
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   www.google.com
Address: 172.217.18.4
Name:   www.google.com
```

#### dig

```
$ dig www.google.com

; <<>> DiG 9.11.3-1 ...
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8738
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;www.google.com.                        IN      A

;; ANSWER SECTION:
www.google.com.         63      IN      A       216.58.206.4

;; Query time: 6 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
```
