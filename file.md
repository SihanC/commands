# File Commands

## Table of Contents
- [df](#df)
    - [Show inode usage](#show-inode-usage)
- [du](#du)
    - [Sort File By Size](#sort-file-by-size)

## df <a name="df"></a>
df — “disk free” displays statistics about the amount of free disk space on the specified mounted file system or on the file system of which file is a part.  By default block counts are displayed with an assumed block size of 512 bytes (如果用-h (human-readable) df就会自动换算, 不用管了). If <b>neither</b> a file or a file system operand is specified, statistics for <b>all mounted file systems</b> are displayed (subject to the -t option below).
```console
$ df [options] [filesystems]
```
### Show inode usage <a name="show-inode-usage"></a>
Every file (including directories, symlinks, sockets, etc.) consumes one inode. Your filesystem has a <b>fixed number of inodes</b> 
created at formatting time.

Even if you still have free disk space, you cannot create new files if:
- You run out of inodes
- (e.g., you created millions of small files)


```console
$ df -ih
```
Output:
- Inodes: total available
- IUsed: number of inodes used (i.e., number of files)
- IFree: how many inodes are still available
- IUse%: inode usage percentage

If IUse% is 100%, you’re out of inodes — even if df -h says there’s space left.

## du <a name="du"></a>
<b>du</b> short for Disk Usage. It will show you details about the disk usage of files and directories on a Linux computer or server. With the du command, you need to specify which folder or file you want to check.
```console
$ du <options> <location of directory or file>
```

-h 代表 hunman-readable.
```console
$ du -h file.txt
➜  c-sample git:(main) ✗ du -h
 28K	./synchronization
...
4.0K	./.vscode
 16K	./signal
324K	.
```   
### Short file by size <a name="sort-file-by-size"></a>
```console
$ du -h /home/user/Desktop | sort -rn
COMMAND     PID   USER   FD      TYPE             DEVICE  SIZE/OFF                NODE NAME
loginwind   164 sihanc  cwd       DIR                1,4       640                   2 /
loginwind   164 sihanc  txt       REG                1,4   2722208 1152921500312137157 /System/Library/CoreServices/loginwindow.app/Contents/MacOS/loginwindow
loginwind   164 sihanc  txt       REG                1,4       110 1152921500312134972 /System/Library/CoreServices/SystemVersion.bundle/English.lproj/SystemVersion.strings
loginwind   164 sihanc  txt       REG                1,4    137016 1152921500312134679 /System/Library/CoreServices/SystemAppearance.bundle/Contents/Resources/SystemAppearance.car
loginwind   164 sihanc  txt       REG                1,4    209664 1152921500312234978 /System/Library/LoginPlugins/FSDisconnect.loginPlugin/Contents/MacOS/FSDisconnect
```
**FD**: File descriptor