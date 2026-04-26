# Network Commands

## Table of Contents
- [Public IP](#public-ip)
    - [Check public IP](#check-public-ip)
- [lsof](#lsof)
    - [List all open files](#list-open-files)
    - [List files on a process](#list-files-specific-process)
    - [List all network connections](#list-open-network-connections)
    - [List all network connections on a specific port](#list-open-network-connections-specific-port)
    - [List all network connections in a port range](#list-open-network-connections-port-range)
- [netstat](#netstat)
    - [Default Outpout Fields](#default-output-fields)
- [ss](#ss)
    - [List all connections](#list-all-connections)
    - [List all TCP connections](#list-all-tcp-connections)
    - [List connections to a specific IP](#list-all-connections-specific-ip)
    - [List IPv4|IPv6 connections](#list-ipv46-connections)
    - [Show process ID](#show-process-id)
- [ip](#ip)
    - [List all interfaces](#ip-list-all-interfaces)
    - [Show a specific interface](#ip-show-specific-interface)
    - [Show IP addresses only](#ip-show-ip-addresses)
    - [Bring an interface up/down](#ip-set-interface-updown)
    - [Add/Delete an IP address](#ip-add-delete-address)
    - [Show routing table](#ip-show-routing-table)
    - [Show route to a destination](#ip-get-route)
    - [Show neighbor table (ARP/NDP)](#ip-show-neighbor-table)
    - [List network namespaces](#ip-netns-list)
    - [Create/Delete a network namespace](#ip-netns-add-delete)
    - [Run a command inside a namespace](#ip-netns-exec)
    - [Show which namespace a process belongs to](#ip-netns-identify)
    - [List processes in a namespace](#ip-netns-pids)
- [tcpdump](#tcpdump)
    - [List all interfaces](#list-all-interfaces)
    - [Capture specific interface](#capture-specific-interface)
    - [Capture any interface](#capture-any-interface)
    - [Skip hostname conversion](#skip-host-name-conv)
    - [Read/Write captures to a file](#r/w-to-file)

## Public IP <a name="public-ip"></a>
### Check public IP <a name="check-public-ip"></a>
如果想知道外部网站看到的你的公网出口 IP, 可以直接:
```console
$ curl ifconfig.me
```
这个返回的是 public IP, 不是本机的内网 IP.

比如在家里, 这个通常是你家路由器的公网 IP. 如果你在 VPN 或 proxy 后面, 这里显示的是 VPN/proxy 的出口 IP.

## lsof <a name="lsof"></a>
又叫做 list open files. It lists all the open files (在 Linux 里, everything is a file, 比如 network socket 也是) belonging to all active processes.

### List all open files <a name="list-open-files"></a>
```console
$ lsof
COMMAND     PID   USER   FD      TYPE             DEVICE  SIZE/OFF                NODE NAME
loginwind   164 sihanc  cwd       DIR                1,4       640                   2 /
loginwind   164 sihanc  txt       REG                1,4   2722208 1152921500312137157 /System/Library/CoreServices/loginwindow.app/Contents/MacOS/loginwindow
loginwind   164 sihanc  txt       REG                1,4       110 1152921500312134972 /System/Library/CoreServices/SystemVersion.bundle/English.lproj/SystemVersion.strings
loginwind   164 sihanc  txt       REG                1,4    137016 1152921500312134679 /System/Library/CoreServices/SystemAppearance.bundle/Contents/Resources/SystemAppearance.car
loginwind   164 sihanc  txt       REG                1,4    209664 1152921500312234978 /System/Library/LoginPlugins/FSDisconnect.loginPlugin/Contents/MacOS/FSDisconnect
```
**FD**: File descriptor

### List files on a process <a name="list-files-specific-process"></a>
```console
$ lsof -p 422
COMMAND   PID   USER   FD      TYPE             DEVICE  SIZE/OFF                NODE NAME
ControlCe 422 sihanc  cwd       DIR                1,4       640                   2 /
ControlCe 422 sihanc  txt       REG                1,4  14677680 1152921500312115341 /System/Library/CoreServices/ControlCenter.app/Contents/MacOS/ControlCenter
ControlCe 422 sihanc  txt       REG                1,4      7144 1152921500312115379 /System/Library/CoreServices/ControlCenter.app/Contents/Resources/InfoPlist.loctable
ControlCe 422 sihanc  txt       REG                1,4       110 1152921500312134972 /System/Library/CoreServices/SystemVersion.bundle/English.lproj/SystemVersion.strings
ControlCe 422 sihanc  txt       REG                1,4    238064            71415945 /private/var/db/timezone/tz/2023d.1.0/icutz/icutz44l.dat
ControlCe 422 sihanc  txt       REG                1,4    137016 1152921500312134679 /System/Library/CoreServices/SystemAppearance.bundle/Contents/Resources/SystemAppearance.car
ControlCe 422 sihanc  txt       REG                1,4     73240 1152921500312134664 /System/Library/CoreServices/SystemAppearance.bundle/Contents/Resources/FauxVibrantDark.car
ControlCe 422 sihanc  txt       REG                1,4      1366 1152921500312115363 /System/Library/CoreServices/ControlCenter.app/Contents/Resources/Base.lproj/Main.storyboardc/MainMenu.nib
ControlCe 422 sihanc  txt       REG                1,4      5735 1152921500312168076 /System/Library/Frameworks/ApplicationServices.framework/Versions/A/Frameworks/SpeechSynthesis.framework/Versions/A/Resources/TimeAnnouncements.loctable
ControlCe 422 sihanc  txt       REG                1,4     18138 1152921500312315129 /System/Library/PrivateFrameworks/DoNotDisturbKit.framework/Versions/A/Resources/Localizable.loctable
ControlCe 422 sihanc  txt       REG                1,4    281152 1152921500312512777 /usr/lib/libobjc-trampolines.dylib
```

### List all network connections <a name="list-open-network-connections"></a>
```console
$ lsof -i
COMMAND     PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
loginwind   164 sihanc    5u  IPv4 0xaaf98b5dacdde4a3      0t0  UDP *:*
ControlCe   422 sihanc    8u  IPv4 0xaaf98b70dcac6b7b      0t0  TCP *:afs3-fileserver (LISTEN)
ControlCe   422 sihanc    9u  IPv6 0xaaf98b6c107093e3      0t0  TCP *:afs3-fileserver (LISTEN)
ControlCe   422 sihanc   10u  IPv4 0xaaf98b70dcac7703      0t0  TCP *:commplex-main (LISTEN)
ControlCe   422 sihanc   11u  IPv6 0xaaf98b6c10706be3      0t0  TCP *:commplex-main (LISTEN)
ControlCe   422 sihanc   16u  IPv4 0xaaf98b5dacde88a3      0t0  UDP *:*
Google      429 sihanc   66u  IPv4 0xaaf98b5dacdde0a3      0t0  UDP *:*
rapportd    442 sihanc    9u  IPv4 0xaaf98b70da45f0ab      0t0  TCP *:60780 (LISTEN)
rapportd    442 sihanc   10u  IPv6 0xaaf98b6c107133e3      0t0  TCP *:60780 (LISTEN)
```
### List all network connections on a specific port <a name="list-open-network-connections-specific-port"></a>
```console
$ lsof -i :80
COMMAND     PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
WeChat    16970 sihanc   86u  IPv6 0xaaf98b6c0fed43e3      0t0  TCP 10.0.0.121:61707->49.51.190.94:http (CLOSE_WAIT)
WeChat    16970 sihanc   96u  IPv6 0xaaf98b6c0fec2be3      0t0  TCP 10.0.0.121:61708->49.51.67.253:http (CLOSE_WAIT)
WeChatApp 17012 sihanc   24u  IPv4 0xaaf98b70db207e13      0t0  TCP 10.0.0.121:62966->101.33.20.249:http (CLOSE_WAIT)
```
上面的 command 会 check both TCP and UDP. 如果只想 check TCP 可以用following
```console
$ lsof -i tcp:80
COMMAND     PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
WeChat    16970 sihanc   86u  IPv6 0xaaf98b6c0fed43e3      0t0  TCP 10.0.0.121:61707->49.51.190.94:http (CLOSE_WAIT)
WeChat    16970 sihanc   96u  IPv6 0xaaf98b6c0fec2be3      0t0  TCP 10.0.0.121:61708->49.51.67.253:http (CLOSE_WAIT)
WeChatApp 17012 sihanc   24u  IPv4 0xaaf98b70db207e13      0t0  TCP 10.0.0.121:62966->101.33.20.249:http (CLOSE_WAIT)
```
如果要 check udp 就把上面的 tcp 改成 udp.
找到这个 process 以后可以用 kill -9 pid 来 kill 这个 process.

### List all network connections in a port range <a name="list-open-network-connections-port-range"></a>
```console
$ lsof -i tcp:1-80
COMMAND     PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
WeChat    16970 sihanc   86u  IPv6 0xaaf98b6c0fed43e3      0t0  TCP 10.0.0.121:61707->49.51.190.94:http (CLOSE_WAIT)
WeChat    16970 sihanc   96u  IPv6 0xaaf98b6c0fec2be3      0t0  TCP 10.0.0.121:61708->49.51.67.253:http (CLOSE_WAIT)
WeChatApp 17012 sihanc   26u  IPv4 0xaaf98b70db18046b      0t0  TCP 10.0.0.121:65454->101.33.21.91:http (ESTABLISHED)
```

## netstat <a name="netstat"></a>
netstat does nth special, It can print network statistics, but ifconfig can do that too. It can print routing tables, but route can do that too. It can print open connections, but lsof does that, and more. 用它的主要原因是因为它结合了常用的 network analysis actions into one command 和因为它是 multi-platform 的. [[Reference](https://www.pluralsight.com/resources/blog/cloud/netstat-network-analysis-and-troubleshooting-explained)]

Note, 这个command已经是deprecated了, 要用可以用 ss.
### Default Output Fields <a name="default-output-fields"></a>
**Proto**: The protocol (tcp, udp, raw) used by the socket.\
**Recv-Q|Send=Q**: How much data is in the queue for that socket, waiting to be read (Recv-Q) or sent (Send-Q). In short: if this is 0, everything's ok. If there are non-zero values anywhere, there may be trouble. [[Reference](https://www.pluralsight.com/resources/blog/cloud/netstat-network-analysis-and-troubleshooting-explained#:~:text=The%20%22Recv%2DQ%22%20and,anywhere%2C%20there%20may%20be%20trouble.)]
```console
➜ netstat
Active Internet connections
Proto Recv-Q Send-Q  Local Address          Foreign Address        (state)
tcp4       0      0  192.168.68.54.57487    server-3-163-158.https ESTABLISHED
tcp6       0      0  sihans-macbook-p.1024  fe80::ce8a:e332:.1024  SYN_SENT
tcp4       0      0  192.168.68.54.57486    lb-140-82-114-26.https ESTABLISHED
tcp4       0      0  192.168.68.54.57485    lb-140-82-114-22.https ESTABLISHED
```

## ss <a name="ss"></a>
[Reference](https://phoenixnap.com/kb/ss-command)

Output 和上面的 netstat 的基本一样.\
**Netid**: Type of socket. Common types are TCP, UDP, u_str (Unix stream), and u_seq (Unix sequence).

By default, output omit listening socket, 只显示 non-listening (for TCP this means established connections) socket.
```console
$ ss
Netid  State      Recv-Q Send-Q               Local Address:Port                        Peer Address:Port
u_str  ESTAB      0      0                    * 25265                                   * 25267
u_str  ESTAB      0      0                    /run/systemd/journal/stdout 19514         * 18849
u_str  ESTAB      0      0                    * 23108                                   * 23107
```
### List all connections <a name="list-all-connections"></a>
-a 或者 --all
```console
$ ss -a
Netid State      Recv-Q Send-Q                Local Address:Port                        Peer Address:Port
nl    UNCONN     0      0                     rtnl:kernel                               *
nl    UNCONN     768    0                     tcpdiag:kernel                            *
u_str LISTEN     0      100                   private/virtual 23118                     * 0
```
### List all TCP connections <a name="list-all-tcp-connections"></a>
-t list TCP connections, -at list all TCP connections including LISTEN sockets.
```console
$ ss -ta
State      Recv-Q Send-Q                   Local Address:Port                           Peer Address:Port
LISTEN     0      128                                  *:sunrpc                                    *:*
LISTEN     0      100                          127.0.0.1:smtp                                      *:*
ESTAB      0      0                           10.10.0.69:57086                       169.254.169.254:http
ESTAB      0      0                           10.10.0.69:57084                       169.254.169.254:http
ESTAB      0      0                           10.10.0.69:57090                       169.254.169.254:http
FIN-WAIT-1 0      1                           10.10.0.69:ssh                            218.92.0.161:52663
ESTAB      0      0                           10.10.0.69:57094                       169.254.169.254:http
ESTAB      0      0                           10.10.0.69:57092                       169.254.169.254:http
ESTAB      0      0                           10.10.0.69:59002                        147.154.18.124:https
LAST-ACK   0      1281                        10.10.0.69:ssh                            218.92.0.161:22719
LISTEN     0      128                               [::]:sunrpc                                 [::]:*
```
### List connections to a specific IP <a name="list-all-connections-specific-ip"></a>
用 src | dst [ip]
```console
$ ss dst 169.254.169.254
Netid  State      Recv-Q Send-Q            Local Address:Port                           Peer Address:Port
tcp    ESTAB      0      0                    10.10.0.69:57170                       169.254.169.254:http
tcp    ESTAB      0      0                    10.10.0.69:57168                       169.254.169.254:http
tcp    ESTAB      0      0                    10.10.0.69:57180                       169.254.169.254:http
```
### List IPv4|IPv6 connections <a name="list-ipv46-connections"></a>
用 -4 | -6
```console
$ ss -4
Netid  State      Recv-Q Send-Q            Local Address:Port                           Peer Address:Port
tcp    ESTAB      0      36                   10.10.0.69:ssh                           50.47.239.199:60711
tcp    ESTAB      0      0                    10.10.0.69:57344                       169.254.169.254:http
tcp    ESTAB      0      0                    10.10.0.69:57342                       169.254.169.254:http
tcp    FIN-WAIT-1 0      1                    10.10.0.69:ssh                            218.92.0.161:59177
tcp    ESTAB      0      0                    10.10.0.69:57350                       169.254.169.254:http
tcp    ESTAB      0      0                    10.10.0.69:57346                       169.254.169.254:http
```
### Show Process IDs <a name="show-process-id"></a>
用 -p. Note, have to be in root to get process id. Otherwise, it will be empty.
```console
$ sudo ss -tp
State      Recv-Q Send-Q                Local Address:Port                        Peer Address:Port
ESTAB      0      0                        10.10.0.69:57466                    169.254.169.254:http                  users:(("runcommand",pid=17986,fd=17))
ESTAB      0      36                       10.10.0.69:ssh                        50.47.239.199:60711                 users:(("sshd",pid=15315,fd=3),("sshd",pid=15312,fd=3))
FIN-WAIT-1 0      1                        10.10.0.69:ssh                         218.92.0.161:32864
ESTAB      0      0                        10.10.0.69:57456                    169.254.169.254:http                  users:(("runcommand",pid=17986,fd=16))
ESTAB      0      0                        10.10.0.69:57460                    169.254.169.254:http                  users:(("runcommand",pid=17986,fd=18))
ESTAB      0      0                        10.10.0.69:57464                    169.254.169.254:http                  users:(("runcommand",pid=17986,fd=15))
ESTAB      0      0                        10.10.0.69:57458                    169.254.169.254:http                  users:(("runcommand",pid=17986,fd=7))
ESTAB      0      0                        10.10.0.69:55846                     147.154.28.185:https                 users:(("gomon",pid=17962,fd=14))
ESTAB      0      0                        10.10.0.69:57468                    169.254.169.254:http                  users:(("runcommand",pid=17986,fd=20))
```

## ip <a name="ip"></a>
`ip` 是 Linux 上比较新的 network command, 用来 replace `ifconfig`, `route`, `arp` 等老 command. 常见 object 有 `addr`, `link`, `route`, `neigh`.

格式基本是:
```console
$ ip [ OPTIONS ] OBJECT COMMAND
```

### List all interfaces <a name="ip-list-all-interfaces"></a>
看所有 network interface 的 link 层信息, 比如 interface name, MAC address, MTU, state.
```console
$ ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:12:34:56 brd ff:ff:ff:ff:ff:ff
```
`state UP` 表示 interface 是启动状态.

### Show a specific interface <a name="ip-show-specific-interface"></a>
只看某一个 interface.
```console
$ ip link show dev eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:12:34:56 brd ff:ff:ff:ff:ff:ff
```
`dev` 是 `device` 的缩写, 用来指定对哪个 network interface 操作. 这里的 `eth0` 就是 interface name.

### Show IP addresses only <a name="ip-show-ip-addresses"></a>
看 interface 上的 IP address. `addr` 可以简写成 `a`.
```console
$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default
    inet 192.168.1.10/24 brd 192.168.1.255 scope global eth0
    inet6 fe80::5054:ff:fe12:3456/64 scope link
```
如果只想要 brief output:
```console
$ ip -br addr
lo               UNKNOWN        127.0.0.1/8 ::1/128
eth0             UP             192.168.1.10/24 fe80::5054:ff:fe12:3456/64
```

### Bring an interface up/down <a name="ip-set-interface-updown"></a>
把 interface 启动或关掉.
```console
$ sudo ip link set dev eth0 up
$ sudo ip link set dev eth0 down
```
这个 command 只改 interface state, 不一定会自动配置 IP.

### Add/Delete an IP address <a name="ip-add-delete-address"></a>
给 interface 加一个 IP, 或者删掉一个 IP.
```console
$ sudo ip addr add 192.168.1.20/24 dev eth0
$ sudo ip addr del 192.168.1.20/24 dev eth0
```
这个通常是 temporary change, reboot 或 network service reload 以后可能会消失.

### Show routing table <a name="ip-show-routing-table"></a>
看 kernel routing table. `route` 可以简写成 `r`.
```console
$ ip route show
default via 192.168.1.1 dev eth0 proto dhcp src 192.168.1.10 metric 100
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10
```
`default via 192.168.1.1` 就是 default gateway.

### Show route to a destination <a name="ip-get-route"></a>
看如果要去某个 destination, kernel 会选哪条 route.
```console
$ ip route get 8.8.8.8
8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.10 uid 1000
    cache
```
这个在 troubleshooting 很有用, 因为它会告诉你具体走哪个 interface, 用哪个 source IP.

### Show neighbor table (ARP/NDP) <a name="ip-show-neighbor-table"></a>
看 neighbor table. IPv4 基本可以理解成 ARP cache, IPv6 则是 NDP.
```console
$ ip neigh show
192.168.1.1 dev eth0 lladdr aa:bb:cc:dd:ee:ff REACHABLE
192.168.1.20 dev eth0 lladdr 11:22:33:44:55:66 STALE
```
常见 state 有 `REACHABLE`, `STALE`, `FAILED`.

### List network namespaces <a name="ip-netns-list"></a>
`netns` 是 network namespace. 它可以把 network stack 隔离开, 每个 namespace 可以有自己独立的 interface, route table, ARP table, iptables 等.
```console
$ ip netns list
ns1
ns2
```
如果没有 output, 通常表示当前没有 named namespace.

### Create/Delete a network namespace <a name="ip-netns-add-delete"></a>
创建一个新的 network namespace, 或者删掉它.
```console
$ sudo ip netns add ns1
$ sudo ip netns delete ns1
```
`add` 以后这个 namespace 就存在了, 但一开始里面通常只有 loopback interface, 而且可能还是 down 的.

### Run a command inside a namespace <a name="ip-netns-exec"></a>
在指定的 network namespace 里执行 command.
```console
$ sudo ip netns exec ns1 ip addr
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```
这个很常用, 比如:
```console
$ sudo ip netns exec ns1 ip link set lo up
$ sudo ip netns exec ns1 ping 8.8.8.8
```
也就是把后面的 command 放到 `ns1` 这个 namespace 里跑.

### Show which namespace a process belongs to <a name="ip-netns-identify"></a>
看一个 process 属于哪个 named namespace.
```console
$ ip netns identify 12345
ns1
```
这里的 `12345` 是 PID.

### List processes in a namespace <a name="ip-netns-pids"></a>
看某个 network namespace 里有哪些 process.
```console
$ ip netns pids ns1
12345
12378
```
这个在 troubleshooting 很有用, 可以看哪些 process 还在占用这个 namespace.

## tcpdump <a name="tcpdump"></a>
### List all interfaces <a name="list-all-interfaces"></a>
```console
➜  ~ tcpdump -D
1.en0 [Up, Running, Wireless, Associated]
2.awdl0 [Up, Running, Wireless, Associated]
3.llw0 [Up, Running, Wireless, Associated]
4.utun0 [Up, Running]
...
24.stf0 [none]
```
### Capture specific interface <a name="capture-specific-interface"></a>
```console
➜  ~ tcpdump -i en0
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on en0, link-type EN10MB (Ethernet), snapshot length 524288 bytes
15:25:49.563869 ARP, Request who-has 192.168.68.69 (Broadcast) tell 192.168.68.1, length 46
15:25:49.650051 IP 192.168.68.73.49552 > resolver-a.as20055.net.domain: 16184+ PTR? 69.68.168.192.in-addr.arpa. (44)
```
### Capture any interface <a name="capture-any-interface"></a>
```console
➜  ~ sudo tcpdump -i any
Password:
tcpdump: data link type PKTAP
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on any, link-type PKTAP (Apple DLT_PKTAP), snapshot length 524288 bytes
15:27:10.941172 IP 192.168.68.73.55059 > sea09s35-in-f10.1e100.net.https: Flags [.], ack 931785855, win 2048, length 0
15:27:11.001743 IP 192.168.68.73.59013 > resolver-a.as20055.net.domain: 22780+ PTR? 234.215.251.142.in-addr.arpa. (46)
```
### Skip hostname conversion <a name="skip-host-name-conv"></a>
By default, tcpdump resolves IP addresses and ports into names. When troubleshooting network issues, it is often easier to use the IP addresses and port numbers; 

Don't convert host addresses to names. This can be used to avoid DNS lookups, using -n.
```console
➜  ~ tcpdump -i en0 -n
```
Don't convert protocol and port numbers etc. to names either by using the option -nn.
```console
➜  ~ tcpdump -i en0 -nn
```
### Read/Write captures to a file <a name="r/w-to-file"></a>
Read from a file
```console
➜  ~ tcpdump -i en0 -r {file_name}
```
Write to a file
```console
➜  ~ tcpdump -i en0 -w {file_name}
```
