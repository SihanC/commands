# Java Commands

## Table of Contents
- [Heap](#heap)
    - [Class Histogram](#class-histogram)
    - [Heap Dump](#heap-dump)

## Heap <a name="heap"></a>
### Class Histogram <a name="class-histogram"></a>
[Reference](https://docs.oracle.com/en/java/javase/17/docs/specs/man/jcmd.html)

会先 trigger 个 full GC.
```console
$ jcmd {pid} GC.class_histogram
```
不会 trigger 个 full GC, 包含了 unreachable 的 objects.
```console
$ jcmd {pid} GC.class_histogram -all
```
### Heap Dump <a name="heap-dump"></a>
[Reference](https://docs.oracle.com/en/java/javase/17/docs/specs/man/jcmd.html)

会先 trigger 个 full GC.
```console
$ jcmd {pid} GC.heap_dump {file_path_to_save_to}
```
不会 trigger 个 full GC, 包含了 unreachable 的 objects.
```console
$ jcmd {pid} GC.class_histogram -all {file_path_to_save_to} 
```