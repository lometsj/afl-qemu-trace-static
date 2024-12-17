# afl-qemu-trace-staic
提供静态链接的afl-qemu-trace，用以在受限环境下开启afl fuzz对异构二进制的测试，而不用在受限场景自主编译。（配代理劳心费力）
HOST均为amd64，target支持amd64 i386 arm aarch64
同时会记录一些fuzz测试的注意事项

## 用法
直接将对应架构的二进制改名为`afl-qemu-trace`放在afl++根目录即可，之后afl-fuzz使用`-Q`开启。
如需qasan，同理改名为`libqasan.so`放在afl++根目录即可

## attention
在fuzz之前可以先用afl-qemu-trace尝试跑一下二进制，用法跟qemu-user-xx一致
在开启使用afl-fuzz时，qemu-user-xx的参数不好传递，可以用环境变量传递
```
Argument             Env-variable      Description
-h                                     print this help
-help
-g port              QEMU_GDB          wait gdb connection to 'port'
-L path              QEMU_LD_PREFIX    set the elf interpreter prefix to 'path'
-s size              QEMU_STACK_SIZE   set the stack size to 'size' bytes
-cpu model           QEMU_CPU          select CPU (-cpu help for list)
-E var=value         QEMU_SET_ENV      sets targets environment variable (see below)
-U var               QEMU_UNSET_ENV    unsets targets environment variable (see below)
-0 argv0             QEMU_ARGV0        forces target process argv[0] to be 'argv0'
-r uname             QEMU_UNAME        set qemu uname release string to 'uname'
-B address           QEMU_GUEST_BASE   set guest_base address to 'address'
-R size              QEMU_RESERVED_VA  reserve 'size' bytes for guest virtual address space
-d item[,...]        QEMU_LOG          enable logging of specified items (use '-d help' for a list of items)
-dfilter range[,...] QEMU_DFILTER      filter logging based on address range
-D logfile           QEMU_LOG_FILENAME write logs to 'logfile' (default stderr)
-p pagesize          QEMU_PAGESIZE     set the host page size to 'pagesize'
-singlestep          QEMU_SINGLESTEP   run in singlestep mode
-strace              QEMU_STRACE       log system calls
-seed                QEMU_RAND_SEED    Seed for pseudo-random number generator
-trace               QEMU_TRACE        [[enable=]<pattern>][,events=<file>][,file=<file>]
-version             QEMU_VERSION      display version information and exit
```
另外要fuzz so需要
```
AFL_INST_LIBS=1
```
开启qasan需要
```
AFL_USE_QASAN=1
```