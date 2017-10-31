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
Sample Testing
--------

### 1. Block all incoming traffic, unblock all outgoing traffic

1.1 Check all the help

```bash
sudo ./mf --help
 ```

1.2 Enter the configuration commands below to  Block all incoming traffic, unblock all outgoing traffic,

```bash
sudo ./mf --in --proto 3 --action BLOCK
sudo ./mf --out --proto 3 --action LOG
./mf --print
 ```
 
1.3 ping 127.0.0.1 –c 1

1.4 Check the output of the minifirewall kernel module by,

tail –f /var/log/messages or dmesg

### 2. Test IP address and Netmask

2.1 Replace 10.0.2.15 with your local IP address. Enter the commands below, and check if you can get similar result.

```bash
sudo ./mf --out --srcip 10.0.2.15 --proto UDP --action BLOCK 
./mf --print
----------------------------------------------
ping google.com
----------------------------------------------
sudo ./mf --delete 1
sudo ./mf --out --srcip 10.0.2.16 --proto UDP --action BLOCK
./mf --print
---------------------------------------------
ping google.com
---------------------------------------------
sudo ./mf --delete 1
sudo ./mf --out --srcip 10.0.2.16 --srcnetmask 255.252.0.0 --proto UDP --action BLOCK
./mf --print
---------------------------------------------
ping google.com

```
2.2 The idea is first time the ping is blocked because IP address matches the BLOCK rule. Second time the ping can go through because IP address doesn’t match. The third ping is blocked again because the first 14 bits (according to the mask 255.252.0.0) matches.

One can also examine the /var/log/messages or dmesg content for verification.

### 3. Block all incoming packets from youtube.com(216.58.221.46)

```bash
------------------------------
ping youtube.com
------------------------------

sudo ./mf --in --srcip 216.58.221.46 --proto 3 --action BLOCK

./mf --print
 ```
ping youtube.com will halt after block action 
