# kvm_memo
KVM Memo 

# commands

check status of gest OS

```
$ virsh list --all

 Id    Name                           State
----------------------------------------------------

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
```

Access and Log in guest 

```
$ virsh console <guest name>
```
