Here is a list of common commands for reference.
https://docs.openstack.org/cli-reference/
Identity (keystone)

List all users
$ keystone user-list

List Identity service catalog
$ keystone catalog
Images (glance)

List images you can access
$ glance image-list

Delete specified image
$ glance image-delete IMAGE

Describe a specific image
$ glance image-show IMAGE

Update image
$ glance image-update IMAGE

Upload kernel image
$ glance image-create --name "cirros-threepart-kernel" \
  --disk-format aki --container-format aki --is-public False \
  --file ~/images/cirros-0.3.1~pre4-x86_64-vmlinuz

Upload RAM image
$ glance image-create --name "cirros-threepart-ramdisk" \
  --disk-format ari --container-format ari --is-public False \
  --file ~/images/cirros-0.3.1~pre4-x86_64-initrd

Upload three-part image
$ glance image-create --name "cirros-threepart" --disk-format ami \
  --container-format ami --is-public False \
  --property kernel_id=$KID-property ramdisk_id=$RID \
  --file ~/images/cirros-0.3.1~pre4-x86_64-blank.img

Register raw image
$ glance image-create --name "cirros-raw" --disk-format raw \
  --container-format bare --is-public False \
  --file ~/images/cirros-0.3.1~pre4-x86_64-disk.img

Compute (nova)

List instances, check status of instance
$ nova list

List images
$ nova image-list

List flavors
$ nova flavor-list

Boot an instance using flavor and image names (if names are unique)
$ nova boot --image IMAGE --flavor FLAVOR INSTANCE_NAME
$ nova boot --image cirros-0.3.1-x86_64-uec --flavor m1.tiny \
  MyFirstInstance

Login to instance
# ip netns
# ip netns exec NETNS_NAME ssh USER@SERVER
# ip netns exec qdhcp-6021a3b4-8587-4f9c-8064-0103885dfba2 \
  ssh cirros@10.0.0.2

Note
In CirrOS the password for user cirros is “cubswin:)” without the quotes.

Show details of instance
$ nova show NAME
$ nova show MyFirstInstance

View console log of instance
$ nova console-log MyFirstInstance

Set metadata on an instance
$ nova meta volumeTwoImage set newmeta='my meta data'

Create an instance snapshot
$ nova image-create volumeTwoImage snapshotOfVolumeImage
$ nova image-show snapshotOfVolumeImage

Pause, suspend, stop, rescue, resize, rebuild, reboot an instance
Pause
$ nova pause NAME
$ nova pause volumeTwoImage

Unpause
$ nova unpause NAME

Suspend
$ nova suspend NAME

Unsuspend
$ nova resume NAME

Stop
$ nova stop NAME

Start
$ nova start NAME

Rescue
$ nova rescue NAME
$ nova rescue NAME --rescue_image_ref RESCUE_IMAGE
Resize
$ nova resize NAME FLAVOR
$ nova resize my-pem-server m1.small
$ nova resize-confirm my-pem-server1

Rebuild
$ nova rebuild NAME IMAGE
$ nova rebuild newtinny cirros-qcow2

Reboot
$ nova reboot NAME
$ nova reboot newtinny

Inject user data and files into an instance
$ nova boot --user-data FILE INSTANCE
$ nova boot --user-data userdata.txt --image cirros-qcow2 \
  --flavor m1.tiny MyUserdataInstance2

To validate that the file was injected, use ssh to connect to the instance, and look in /var/lib/cloud for the file.

Inject a keypair into an instance and access the instance with that keypair
Create keypair
$ nova keypair-add test > test.pem
$ chmod 600 test.pem
Start an instance (boot)
$ nova boot --image cirros-0.3.0-x86_64 --flavor m1.small \
  --key_name test MyFirstServer

Use ssh to connect to the instance
# ip netns exec qdhcp-98f09f1e-64c4-4301-a897-5067ee6d544f \
  ssh -i test.pem cirros@10.0.0.4

Manage security groups
Add rules to default security group allowing ping and SSH between instances in the default security group
$ nova secgroup-add-group-rule default default icmp -1 -1
$ nova secgroup-add-group-rule default default tcp 22 22

Networking (neutron)

Create network
$ neutron net-create NAME

Create a subnet
$ neutron subnet-create NETWORK_NAME CIDR
$ neutron subnet-create my-network 10.0.0.0/29

Block Storage (cinder)
Used to manage volumes and volume snapshots that attach to instances.

Create a new volume
$ cinder create SIZE_IN_GB --display-name NAME
$ cinder create 1 --display-name MyFirstVolume

Boot an instance and attach to volume
$ nova boot --image cirros-qcow2 --flavor m1.tiny MyVolumeInstance

List volumes, notice status of volume
$ cinder list

Attach volume to instance after instance is active, and volume is available
$ nova volume-attach INSTANCE_ID VOLUME_ID auto
$ nova volume-attach MyVolumeInstance /dev/vdb auto

Manage volumes after login into the instance
List storage devices
# fdisk -l

Make filesystem on volume
# mkfs.ext3 /dev/vdb

Create a mountpoint
# mkdir /myspace

Mount the volume at the mountpoint
# mount /dev/vdb /myspace

Create a file on the volume
# touch /myspace/helloworld.txt
# ls /myspace

Unmount the volume
# umount /myspace

Object Storage (swift)
Display information for the account, container, or object
$ swift stat
$ swift stat ACCOUNT
$ swift stat CONTAINER
$ swift stat OBJECT

List containers
$ swift list


Check Openstack version
# nova service-list
# nova endpoints
# openstack --version

# openstack user list
# openstack catalog list


Public Network Creation
# neutron net-list
# neutron net-create public
# neutron net-list
# neutron subnet-create --name public_subnet --enable_dhcp=False --allocation-pool=start=192.168.122.21,end=192.168.122.41 --gateway=192.168.122.1 public 192.168.122.1/24
Public Network Deletion
# neutron net-list
# neutron net-show efc36f59-532d-46e2-974e-3e1a208cfb6b (public ID)
# # neutron net-delete efc36f59-532d-46e2-974e-3e1a208cfb6b (public ID)
# neutron net-list
Private Network Creation
# neutron net-list
# neutron net-create private
# neutron subnet-create --name private_subnet private 80.80.80.80/24 --dns-nameserver 80.80.80.80
# neutron net-list
Private Network Deletion
# neutron net-list
# neutron net-show 0a5b1752-3497-466d-a2bf-ba3eb7e77e7d (private ID)
# neutron net-delete 0a5b1752-3497-466d-a2bf-ba3eb7e77e7d (private ID)
# neutron net-list
Create Router
# neutron router-list
# neutron subnet-list
# neutron net-update public --router:external
# neutron router-create router
# neutron router-list
# neutron subnet-list
# neutron router-interface-add router private_subnet
# ip netns
# neutron router-port-list router
# neutron router-gateway-set router public
# neutron router-port-list router

Delete Router
# neutron router-port-list router
#  neutron router-list
# neutron router-show 1b5f1486-fb0a-477a-b4d7-011a562dfad3 (Router ID)
# neutron router-gateway-clear 1b5f1486-fb0a-477a-b4d7-011a562dfad3 (Router ID)
# neutron router-port-list 1b5f1486-fb0a-477a-b4d7-011a562dfad3 (Router ID)
# neutron router-interface-delete 1b5f1486-fb0a-477a-b4d7-011a562dfad3 ad35baf2-ff62-4499-97cc-e8e76eb4782b (Router ID & private ID)
# neutron router-delete 1b5f1486-fb0a-477a-b4d7-011a562dfad3 (Router ID)
# neutron router-list

Generate a keypair
# nova keypair-list
# ssh-keygen -q -N ""
# nova keypair-add --pub-key ~/.ssh/id_rsa.pub centos-key
# nova keypair-list

Delete Keypair
# nova keypair-list
# nova keypair-delete centos-key
# nova keypair-list

Add security group rules
# nova secgroup-list
# nova secgroup-create centos_secgroup "centos for check"
# nova secgroup-list-rules centos_secgroup
Permit ICMP (ping)
# nova secgroup-add-rule centos_secgroup icmp -1 -1 0.0.0.0/0
Permit secure shell (SSH) access
# nova secgroup-add-rule centos_secgroup tcp 22 22 0.0.0.0/0
Permit secure shell (HTTP) access
# nova secgroup-add-rule centos_secgroup tcp 80 80 0.0.0.0/0
Permit secure shell (HTTPS) access
# nova secgroup-add-rule centos_secgroup tcp 443 443 0.0.0.0/0
# nova secgroup-list

Delete security group rules
# nova secgroup-list
# nova secgroup-list-rules centos_secgroup
# nova secgroup-delete-rule centos_secgroup tcp 22 22 0.0.0.0/0
# nova secgroup-delete-rule centos_secgroup tcp 80 80 0.0.0.0/0
# nova secgroup-delete-rule centos_secgroup icmp -1 -1 0.0.0.0/0
# nova secgroup-list-rules centos_secgroup
# nova secgroup-list
# nova secgroup-delete centos_secgroup
# nova secgroup-list

Image Creation
# glance image-list
# wget http://download.cirros-cloud.net/0.3.4/cirros-0.3.4-x86_64-disk.img
# glance image-create --name "cirros" --file cirros-0.3.4-x86_64-disk.img --disk-format qcow2 --container-format bare --visibility public --progress
# glance image-list

Image Deletion
(image-list ID: 00c622a3-45c8-4a67-8a64-4086ff2b4e4c)
# glance image-list
# glance image-delete 00c622a3-45c8-4a67-8a64-4086ff2b4e4c
# glance image-list

Lanch instance
# nova flavor-list
# nova image-list
# neutron net-list
# nova secgroup-list
# nova secgroup-list-rules centos_secgroup
# glance image-list
# nova keypair-list
# nova boot --flavor m1.tiny --image cirros --nic net-id=5f87faba-5e8a-4325-b8b9-eafea33a0b88 --security-group default --key-name centos-key public-instance
# nova list
# nova show public-instance

Delete Instance 
(public-instance ID: 5fab20da-4197-45d7-a2e3-ab3cf664307e)
# nova list
# nova delete 5fab20da-4197-45d7-a2e3-ab3cf664307e
# nova list
