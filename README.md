# kvm_memo
KVM Memo 

# commands

check status of gest OS

```
$ virsh list --all

 Id    Name                           State
----------------------------------------------------
 1     client1                        running
 2     web1                           running
 3     l3dsrweb                       running
 9     opstack1                       running
 14    web3                           running
 17    web2                           running
 -     client2                        shut off
 -     client3                        shut off
 -     CVP2017101                     shut off
 -     rdo01                          shut off
 -     Spine-1                        shut off
 -     test                           shut off
 -     test3                          shut off

```

Install guest (CentOS 7)

```
$ qemu-img create -f qcow2 /var/lib/libvirt/images/centos7.qcow2 8G

$ virt-install \
--name centos7_CICD \
--virt-type=kvm \
--ram 1024 \
--vcpus 1 \
--os-type=linux \
--os-variant rhel7 \
--boot hd \
--network bridge=br0 \
--disk path=/var/lib/libvirt/images/centos7.qcow2,size=8,format=qcow2 \
--location='/var/lib/libvirt/iso/CentOS-7-x86_64-DVD-1804.iso' \
--graphics none \
--serial pty \
--console pty \
--extra-args "console=tty0 console=ttyS0,115200n8"
```


Run guest OS

```
$ virsh start <guest name>
```

Stop geust

```
$ virsh shutdown <guest name>
```

remove guest

```
$ virsh destroy <guest name>

$ virsh undefine <guest name>
```

Access and Log in guest 

```
$ virsh console <guest name>
```

## Network Setting

Check a NIC Networm Manager 

```
$ nmcli dev

DEVICE     TYPE              STATE
eth0       802-3-ethernet    unmanaged
eth1       802-3-ethernet    unmanaged
eth2       802-3-ethernet    disconnected
eth3       802-3-ethernet    unmanaged
eth4       802-3-ethernet    unmanaged
eth5       802-3-ethernet    connected
```

```
$ nmcli con

NAME                      UUID                                   TYPE              SCOPE    TIMESTAMP-REAL
System eth5               c1a137dd-45d5-466a-ac18-76f5299c2eeb   802-3-ethernet    system   Wed 22 Aug 2018 02:39:50 AM JST
System eth2               c1a137dd-45d5-466a-ac18-76f5299c2eeb   802-3-ethernet    system   never
NAME                      UUID                                   TYPE              SCOPE    TIMESTAMP-REAL
```

```
# virsh net-list --all

Name                 State      Autostart     Persistent
--------------------------------------------------
default              active     yes           yes
```

Check virtual bridge

```
$ brctl show

bridge name	bridge id		STP enabled	interfaces
br-client		8000.f4ce46a6da64	no		eth0
							vnet0
br-l3dsrweb		8000.f4ce46a6da67	no		eth3
							vnet4
br-ras		8000.000000000000	no
br-ras2		8000.000000000000	no
br-web		8000.f4ce46a6da65	no		eth1
							vnet2
							vnet7
br-web2		8000.f4ce46a6da66	no		eth2
							vnet9
br0		8000.441ea1d31fdc	no		eth4
							vnet1
							vnet10
							vnet3
							vnet5
							vnet6
							vnet8
br1		8000.000000000000	no
virbr0		8000.525400d01630	yes		virbr0-nic
```

Create Virtual Bridge Network

```
$ brctl addbr br-container

$ brctl show

bridge name	bridge id		STP enabled	interfaces
br-container		8000.000000000000	no
```

Connect Phisycal NIC to Virtual Bridge

```
$ brctl addif <br name> <eth name>

$ brctl show

```

purge and delete Virtual Bridge 

```
$ brctl delif <br name> <eth name>

$ brctl delbr <br name>

$ brctl show
```