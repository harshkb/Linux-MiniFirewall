Overview
--------
The package is a Linux-minifirewall

It is a project for
* learn how to write a linux kernel module
* learn how to bridge user space and kernel space
* enhance network perspectives

The mini firewall can BLOCK or LOG(UNBLOCK) packets according to a set of rules.The rules
are set by a user space configuraiton utility program.
User can specify three conditions of ip to BLOCK and LOG:
* ip with specific port
* specific ip
* all of ip in same net id


Environment
--------
Linux kernel for version 3.19.0-15-generic.


Build and run
--------
```bash
cd miniFirewall
make
cd module
make
sudo insmod mf_km.ko
cd ..
sudo ./mf --out -t 31.13.87.36 -a BLOCK
```

close the mini firewall
```bash
sudo rmmod mf_km
```
