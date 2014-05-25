XenTools
========

A collection of simple tools to aid in the management of XenServer.

XE Find UUID
-------

###What?

Reads the output of Xen binaries like **xe vm-list** and **xe template-list** and allows you to grep the **name-label**, returning the uuid for that instance.

###Why?
Although I love the simplicity of managing XenServer, I don't find the pretty output (example below) of **xe** perticularly helpful when trying to perform bulk operations on multiple instances. 

```
$> xe vm-list
uuid ( RO)           : 6ae1ca8c-61fc-544b-828d-a802f016ddde
    name-label ( RW) : XOA
    power-state ( RO): running

uuid ( RO)           : 4aaf9c85-3110-eeff-e044-00d3976e70f6
    name-label ( RW) : Windows 7 (64-bit)
    power-state ( RO): running

uuid ( RO)           : 4aaf9c85-5481-eeff-e044-1a4c115725f3
    name-label ( RW) : Windows 8 (64-bit)
    power-state ( RO): running


uuid ( RO)           : 8fda69c5-c538-e683-1943-462408ebdb18
    name-label ( RW) : Damn Small Linux
    power-state ( RO): running
...

```

###How?
```
$> xe vm-list | xe-find-uuid Windows
4aaf9c85-3110-eeff-e044-00d3976e70f6
4aaf9c85-5481-eeff-e044-1a4c115725f3
...
```

###Now what?
Well, you now have a nice ordered list of UUIDs which you can parse using something like...
```
#!/bin/sh
for uuid in `xe template-list | xe-find-uuid CentOS`
do
    # This will set all new CentOS VMs to use the legacy BIOS boot method; very handy
    # since this is the only way to boot from a CentOS ISO :)!
    xe template-param-set HVM-boot-policy="BIOS order" uuid=$uuid
done
```
