# OSCP笔记

## 第一件事情：改root密码
root@kali:/# passwd


## 常用命令：
pwd, man, ls, cd, mkdir, rmdir, cp, mv, locate, adduser, su, sudo, echo, cat, vim, chmod, ifconfig, ping


## 常用服务： apache2, ssh, postgresql
root@kali:/# service apache2 start
root@kali:/# systemctl enable apache2


## 信息收集
Google (inurl:cnn.com, filetype:pdf, intext, .etc)
Exploit-DB/Google Hacking DB
shodan
WHOIS
Netcraft
theharvester


## 扫描
TCP vs UDP

### Nmap
> 选项：-vv详细，-Pn不论是否存活一律扫描，-A综合扫描，-sS SYN扫描，-sU UDP扫描，-T4扫描速度（1慢，5快），-p指定端口，-p-所有端口，-oN输出格式，--script使用脚本扫描

root@kali:/# nmap -vv -Pn -A -sS -T4 -p- -oN /root/tcpscan.txt 192.168.1.101        # TCP SYN扫描    
root@kali:/# nmap -vv -Pn -A -sU -T4 -top-ports 200 -oN /root/udpscan.txt 192.168.1.101         # UDP扫描
root@kali:/# nmap -vv -p 137 --script=all 192.168.1.101                # 对137端口跑脚本

### Nessus
root@kali:/Downloads# dpkg -i <Nessus安装包>.deb     # 安装
root@kali:/Downloads# /etc/init.d/nessusd start     # 启动

### Metasploit
root@kali:/# msfconsole     # 启动msf
msf> search portscan        # 查找模块
msf> use auxilary/...       # 使用
msf> show options           # 查看模块信息
msf> set ports 1-65535      # 设置参数
msf> set rhosts 192.168.15.2
msf> set threads 10
msf> exploit                # 开始



## Kioptrix Level 1
### SSH信息枚举（SSH版本）
查看版本，然后去`searchsploit`或exploit-db找漏洞

### HTTP信息枚举（Web Server版本，目录，文件等）
1. 查看网页源码
2. 查看HTTP通信流量
3. 目录扫描（dirbuster, nikto，御剑）

字典：github: dirbuster wordlist

### SMB信息枚举（OS版本，SMB版本，$IPC等）
root@kali:/# enum4linux
msf5> use auxiliary/scanner/smb/smb_version
root@kali:/# nbtscan
root@kali:/# smbclient