# Linux Virtualization

```
sudo cp debian-13-nocloud-amd64.qcow2 /var/lib/libvirt/images/
```

```
getent passwd libvirt-qemu
```

```
ls -ld /var/lib/libvirt/images
```



maybe:

```
sudo chown libvirt-qemu:libvirt-qemu /var/lib/libvirt/images/debian-13-nocloud-amd64.qcow2
```





```
sudo qemu-img create -f qcow2 -F qcow2 -b /var/lib/libvirt/images/debian-13-nocloud-amd64.qcow2 /var/lib/libvirt/images/k8s-master.qcow2
```



```
qemu-img create -f qcow2 -F qcow2 -b debian-13-nocloud-amd64.qcow2 k8s-master.qcow2
qemu-img create -f qcow2 -F qcow2 -b debian-13-nocloud-amd64.qcow2 k8s-worker1.qcow2
qemu-img create -f qcow2 -F qcow2 -b debian-13-nocloud-amd64.qcow2 k8s-worker2.qcow2
```



## Qemu

#### Convert ova file to qemu

unpack like zip/tar file:

```sql
tar -xvf "filename.ova"
```

convert to qcow2 format:

```sql
qemu-img convert -f vmdk -O qcow2 "filenameunpacked.vmdk" new.qcow2
```

run:

```sql
qemu-system-x86_64 -enable-kvm -m 2G -hda new.qcow2
```
