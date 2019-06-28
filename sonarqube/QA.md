docker-compose 启动sonarqube, 提示 max virtual memory areas vm.max_map_count 65530 is too low 
是因为宿主机上的max_map_count太小，默认的是65535

“This file contains the maximum number of memory map areas a process may have. Memory map areas are used as a side-effect of calling malloc, directly by mmap and mprotect, and also when loading shared libraries.

While most applications need less than a thousand maps, certain programs, particularly malloc debuggers, may consume lots of them, e.g., up to one or two maps per allocation.

The default value is 65536.”

       max_map_count文件包含限制一个进程可以拥有的VMA(虚拟内存区域)的数量。虚拟内存区域是一个连续的虚拟地址空间区域。在进程的生命周期中，每当程序尝试在内存中映射文件，链接到共享内存段，或者分配堆空间的时候，这些区域将被创建。调优这个值将限制进程可拥有VMA的数量。限制一个进程拥有VMA的总数可能导致应用程序出错，因为当进程达到了VMA上线但又只能释放少量的内存给其他的内核进程使用时，操作系统会抛出内存不足的错误。如果你的操作系统在NORMAL区域仅占用少量的内存，那么调低这个值可以帮助释放内存给内核用。

 

调整示例，如下调整为默认的4倍

此操作需要root权限

[root@localhost ~]# sysctl -w vm.max_map_count=262144
查看修改结果

[root@localhost ~]# sysctl -a|grep vm.max_map_count
vm.max_map_count = 262144