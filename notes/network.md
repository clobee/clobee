
## Scan the network

```bash
# Discover the machine in the network

$ nmap -sn 192.168.0.0/24 -oN nmap-init_discovery
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-02 08:38 BST
Nmap scan report for 192.168.0.1
Host is up (0.0040s latency).
Nmap scan report for 192.168.0.12
Host is up (0.0026s latency).
Nmap scan report for 192.168.0.16
Host is up (0.097s latency).
Nmap done: 256 IP addresses (3 hosts up) scanned in 3.03 seconds

# Scan the top 1000 ports of a machine (fast)

$ rustscan -a 192.168.0.12 --top
Open 192.168.0.12:22
Open 192.168.0.12:80
```

## Scan a machine

```bash
# Scan the top 1000 ports of a machine (fast)
# you can use `nmap -p- 192.168.0.12 -oN nmap-machine-12`

$ rustscan -a 192.168.0.12 --top
Open 192.168.0.12:22
Open 192.168.0.12:80

# Scan of specific ports 

$ nmap -sC -sV -A 192.168.0.12 -p 22,80  
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-01 01:17 BST  
Nmap scan report for 192.168.0.12 
Host is up (0.16s latency).  
  
PORT   STATE SERVICE VERSION  
22/tcp open  ssh     OpenSSH 5.3 (protocol 2.0)  
| ssh-hostkey:   
|   1024 18:6a:87:7c:84:a4:39:11:4c:86:b8:64:67:9e:3a:f0 (DSA)  
|_  2048 a1:55:b5:93:53:ea:38:3e:a7:23:94:fa:17:52:13:cd (RSA)  
80/tcp open  http    Apache httpd 2.2.15 ((CentOS))  
| http-robots.txt: 1 disallowed entry   
|\_/hidden  
|\_http-server-header: Apache/2.2.15 (CentOS)  
|\_http-title: smallville  
  
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 15.45 seconds
```

## Scan a machine for any known vulnerabilities

```bash
$ nmap -p- 10.10.10.40 --script vuln   
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-10 17:46 BST  
Pre-scan script results:  
| broadcast-avahi-dos:   
|   Discovered hosts:  
|     224.0.0.251  
|   After NULL UDP avahi packet DoS (CVE-2011-1002).  
|\_  Hosts are all up (not vulnerable).  
Nmap scan report for 10.10.10.40  
Host is up (0.016s latency).  
Not shown: 65526 closed ports  
PORT      STATE SERVICE  
135/tcp   open  msrpc  
139/tcp   open  netbios-ssn  
445/tcp   open  microsoft-ds  
49152/tcp open  unknown  
49153/tcp open  unknown  
49154/tcp open  unknown  
49155/tcp open  unknown  
49156/tcp open  unknown  
49157/tcp open  unknown  
  
Host script results:  
|\_smb-vuln-ms10-054: false  
|\_smb-vuln-ms10-061: NT\_STATUS\_OBJECT\_NAME\_NOT\_FOUND  
| smb-vuln-ms17-010:   
|   VULNERABLE:  
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)  
|     State: VULNERABLE  
|     IDs:  CVE:CVE-2017-0143  
|     Risk factor: HIGH  
|       A critical remote code execution vulnerability exists in Microsoft SMBv1  
|        servers (ms17-010).  
|             
|     Disclosure date: 2017-03-14  
|     References:  
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143  
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx  
|\_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for\-wannacrypt\-attacks/  
  
Nmap done: 1 IP address (1 host up) scanned in 161.14 seconds
```

## Routing

### List the routes

```bash
$ netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG        0 0          0 wlan0
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
192.168.0.0     0.0.0.0         255.255.255.0   U         0 0          0 wlan0

$ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG    600    0        0 wlan0
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
192.168.0.0     0.0.0.0         255.255.255.0   U     600    0        0 wlan0
```

Get the subnet with your listing

```bash
$ ip route
default via 192.168.0.1 dev wlan0 proto dhcp metric 600 
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown 
192.168.0.0/24 dev wlan0 proto kernel scope link src 192.168.0.12 metric 600
```
