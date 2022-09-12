

```shell
# create a vm

$ virt-install \
    --name fed34 \
    --ram 512 \
    --vcpus 1 \
    --disk path=/var/lib/libvirt/images/fed34.img,size=20 \
    --os-variant fedora34 \
    --os-type linux \
    --network bridge=br0 \
    --graphics none \
    --console pty,target_type=serial \
    --location 'https://mirror.arizona.edu/fedora/linux/releases/34/Server/x86_64/os/' \
    --extra-args 'console=ttyS0,115200n8 serial'
```