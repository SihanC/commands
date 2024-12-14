# Network Commands

## Table of Contents
- [lsof](#lsof)
    - [List all open files](#list-open-files)
    - [List files on a process](#list-files-specific-process)
    - [List all network connections](#list-open-network-connections)
    - [List all network connections on a specific port](#list-open-network-connections-specific-port)
    - [List all network connections in a port range](#list-open-network-connections-port-range)
- [netstat](#netstat)
    - [Default Outpout Fields](#default-output-fields)


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
tcp4       0      0  192.168.68.54.57484    lb-140-82-116-5-.https ESTABLISHED
tcp4       0      0  192.168.68.54.57483    cdn-185-199-108-.https ESTABLISHED
tcp4       0      0  192.168.68.54.57482    lb-140-82-116-3-.https ESTABLISHED