# 1. OpenStack HandsOnLab - Basic

## 1.1. Setting up resources for Instances:

<!-- TOC depthfrom:2 orderedlist:true -->

- [1.1. Setting up resources for Instances:](#11-setting-up-resources-for-instances)
- [1.2. Upload Images](#12-upload-images)
    - [1.2.1. How to login to an instance](#121-how-to-login-to-an-instance)
- [1.3. Create a new Flavor](#13-create-a-new-flavor)
- [1.4. Create Private Network](#14-create-private-network)
- [1.5. Create Public Network](#15-create-public-network)
- [1.6. Create Router](#16-create-router)
- [1.7. Declare public network](#17-declare-public-network)
- [1.8. Set Gateway in Router](#18-set-gateway-in-router)
- [1.9. Attach private network to router from network topology](#19-attach-private-network-to-router-from-network-topology)
- [1.10. Create security group](#110-create-security-group)
    - [1.10.1. Add security group rules for each group.](#1101-add-security-group-rules-for-each-group)
- [1.11. Create securty key and save it in .pem file.](#111-create-securty-key-and-save-it-in-pem-file)
- [1.12. Request floating ip in access and security](#112-request-floating-ip-in-access-and-security)
- [1.13. Launch instance.](#113-launch-instance)

<!-- /TOC -->

## 1.2. Upload Images
- Get Images from https://docs.openstack.org/image-guide/obtain-images.html.
- then, project -> compute -> images -> create image

![Create an Image](images/openstack-hol-create-image1.PNG)

### 1.2.1. How to login to an instance
In a CirrOS image, the login account is `cirros` and password is `gocubsgo`.

## 1.3. Create a new Flavor
- ADMIN -> Compute -> FLAVOURS -> create flavor
- Remember to c	hoose permission for required projects.

![Create flavors](images/openstack-hol-create-flavor.PNG)

```
openstack flavor create m1.large --ram 8192 --disk 10 --vcpus 4
``` 

## 1.4. Create Private Network
- Project ->network -> networks -> create network
  - give any network name
  - give any name for subnet
  - network address : it can be any ip because it is our own private network which is not exposed to internet or public. eg: 100.100.0.0/24
  - no need of gateway for private network.
  - leave the DHCP enabled for private network.

![Create Private Network](images/openstack-hol-create-privatenetwork.PNG)
 

## 1.5. Create Public Network 
- project ->network -> networks -> create network
- give any network name, next
- give any name for subnet
- network address : 172.25.9.0/24 (This will be your external network)
- gateway ip : 172.25.9.254
- Disable DHCP
- put the allocation pool like below.
  172.25.9.20,172.25.9.28

![Create Public Network](images/openstack-hol-create-publicnetwork.PNG)
 
## 1.6. Create Router
- project -> network ->routers -> create router
- give any routername and click on create


## 1.7. Declare public network
- Admin -> networks
- click on edit network of 'public network'
- tick the 'external network' checkbox

![Declare Public Network](images/openstack-hol-edit-network.PNG)

## 1.8. Set Gateway in Router
- project -> network-> routers
- click on set gateway in the router
- slect the public network and save 

*Note: if you dont see your public network here, then the reason is you havnt declared it in admins network tab earlier step.*

![Set Router Gateway](images/openstack-hol-set-gateway.PNG)

## 1.9. Attach private network to router from network topology
- project -> networks -> network topology
- keep cursor on router -> add interface
- select private network 

![Set Router Gateway](images/openstack-hol-attach-private-subnet-to-router.PNG)

## 1.10. Create security group
- Project - > Network -> Security groups -> Create Security group
- Give name for security group and click ‘create’

### 1.10.1. Add security group rules for each group.
- project - > access and security -> security groups -> select your  security group
- click on manage rules at right hand side
- Add rules
  - all ICMP : used for ping ( not advisable in production environment)
  - SSH : used for secure shell login
  - http & https : used for web server.

## 1.11. Create securty key and save it in .pem file.
- project -> Compute  -> Key pairs -> create key pair
- give any name for keypair and create
- immediately it will ask you to save the pem key file , so please save it to your local machine 

*Note: This key should be kept for future , in future, it should be used for login purpose to the virtual machine(instance).*


## 1.12. Request floating ip in access and security
Note: You need to get seperate floating ip's for each instance.
Floating ip is used to access the instance from outside the private network. Whenever you need to login to the instance , you need to request for a floating ip and attach it to the private network ip of the instance. Once attached you can ssh to the floating ip to login to instance. If you don’t have any activity and the instance is used by only internal network users/applications then, you can keep the instance detached from the floating IP.

- project -> Networks ->floating ips
- allocate ip to project
- select the public ip.

 
## 1.13. Launch instance.
- project -> instaces -> launch instance
- access and security -> select the keypair you made
- go to networking ->
- click on the + symbol of private network
- click on ‘launch instance’.

Once instance is launched and ‘RUNNING’ then, associate  floating IP which you can use to login/ping from outside
- project -> access and security -> select the private ip of the instance you created -> click on associate.

Now ping the IP – public IP of the instance i.e 172.25.9.21 -  from your local machine to check if the instance is up and reachable.
eg:

[kiosk@foundation9 Downloads]$ ping 172.25.9.21
PING 172.25.9.21 (172.25.9.21) 56(84) bytes of data.
64 bytes from 172.25.9.21: icmp_seq=1 ttl=63 time=5.02 ms
64 bytes from 172.25.9.21: icmp_seq=2 ttl=63 time=0.678 ms
64 bytes from 172.25.9.21: icmp_seq=3 ttl=63 time=0.633 ms
64 bytes from 172.25.9.21: icmp_seq=4 ttl=63 time=0.496 ms
64 bytes from 172.25.9.21: icmp_seq=5 ttl=63 time=0.562 ms
64 bytes from 172.25.9.21: icmp_seq=6 ttl=63 time=0.563 ms
64 bytes from 172.25.9.21: icmp_seq=7 ttl=63 time=0.575 ms
^C
--- 172.25.9.21 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6001ms
rtt min/avg/max/mdev = 0.496/1.219/5.026/1.555 ms
[kiosk@foundation9 Downloads]$ 


Note: Now , give 600 permision to your keypair.pem file
Without 600 permision it wont work.

[kiosk@foundation9 Downloads]$ ls -l
total 399276
-rw-rw-r--. 1 kiosk kiosk      1675 Jul 23 13:11 keypair1.pem
-rw-rw-r--. 1 kiosk kiosk 408852480 Jul 23 12:15 web.img
[kiosk@foundation9 Downloads]$ chmod 600 keypair1.pem
[kiosk@foundation9 Downloads]$ 

[kiosk@foundation9 Downloads]$ pwd
/home/kiosk/Downloads
[kiosk@foundation9 Downloads]$ ssh -i keypair1.pem root@172.25.9.21
Please login as the user "cloud-user" rather than the user "root".

Connection to 172.25.9.21 closed.
[kiosk@foundation9 Downloads]$ 


[kiosk@foundation9 Downloads]$ ssh -i keypair1.pem cloud-user@172.25.9.21
[cloud-user@web1 ~]$ 
[cloud-user@web1 ~]$ 
[cloud-user@web1 ~]$ 
[cloud-user@web1 ~]$ 
[cloud-user@web1 ~]$ hostname
web1.novalocal
[cloud-user@web1 ~]$ id
uid=1001(cloud-user) gid=1001(cloud-user) groups=1001(cloud-user),4(adm),10(wheel),190(systemd-journal) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

Note: use sudo su – to switch to root id.

[cloud-user@web1 ~]$ sudo su -
Last login: Sat Jul 23 03:56:32 EDT 2016 from 172.25.9.250 on pts/0
[root@web1 ~]# 
[root@web1 ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[root@web1 ~]# hostname
web1.novalocal
[root@web1 ~]# 



How to Disassociate the public ip from the created instance.

Project -> access and security -> Floating IPs ->  click on your mapped private IP ->
Disassociate

 


Note: Now , you can use the same floating ip for creating a new instance or to login to any other instance for patching activity or something.

disassociating a floating ip from the instance will not affect production instance unless the instance accessed from public network using that IP, because users will be accessing using their private ip only. floating ip is just for us - admins- to login.






Create/launch another instance now with the existing openstack resources you just configured.

Note: Now, Since you have already made templates/resources ( images, ip’s, router, volumes,flavours etc. ) for the instances, you can use those to create any number of instancesprovided you have enough resources in the pool.

Project -> instances -> launch instance

 

 


Now after launching the instance , associate the public ip from

Project -> access and security -> floating ips

select the newly made second web server this time.


 



Now login to each web instances and edit /var/www/html/index.html
to customize the web page.

Eg: This is a web page from Web instance1
/
This is a web page from Web instance2.

☺

And restart the web services in each instance.

Note: for this you need to login to the instances using the public ip you associated to it.

[root@web1 ~]# systemctl enable httpd;systemctl start httpd
[root@web1 ~]# 





creating load balancer between instances Web1  and Web2


Project -> network -> loadbalancers -> add pool

select private network subnet.


 

 

loadbalancers -> members -> add member


 

Note: now , like above screenshot, press Cntrl key and select both the web instances ( OS+Web server) to loadbalance.


Now for the load balancing part , create a dummy VIP to associate with the public floating ip.





 



select the private subnet.

you can mention any ip of your choice because it is just a static private ip.

 



Now go to project -> access and security -> click on associate


 

select the newly made vip and associate a public ip to it.

Now , try to access public ip 172.25.9.21 from the browser for seeing web page.

http://172.25.9.21

 

 



Note: now, observe that when you hit the same page multiple times - it goes to web1 , web2 in a load balanced manner each time.

Here our case of the above 2 screen shots, first one is coming from instance1 and the second screenshot is for the web page fetched from instance2.







How to delete the instance & all other resources.



Admin tab-> Select instance -> terminate/delete

Admin -> Go to router -> Delete router

Project -> networks -> delete Private network -> delete public network


Project -> Compute -> Access and security->
	Delete Security Group
	Delete Keypair




















Annexure:

location of tutorials and images in Vijay's lab:

openstack Questions:

http://classroom/content/cl210_exam/

image location:

http://classroom/materials/

name of image file : web.img


How to Delete all openstack related things ( openstack installed binary + resources + instances) from serverA

To delete/reset all things( openstack and related things) from the scratch to rebuild servera

run below command from your local machine where the Virtual machine server is created.

rht-vmctl fullreset servera serverb


