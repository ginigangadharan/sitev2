

# mount iso


[root@vcenter ~]# mount -t iso9660 -o loop ~/VMware-VCSA-all-6.7.0-8217866.iso /tmp/mnt/
mount: /dev/loop0 is write-protected, mounting read-only
[root@vcenter ~]# cd /tmp/mnt/
[root@vcenter mnt]# ls -l
total 71
dr-xr-xr-x. 2 root root 8192 Apr 10  2018 dbschema
dr-xr-xr-x. 2 root root 4096 Apr 10  2018 migration-assistant
-r-xr-xr-x. 1 root root 6670 Apr 10  2018 readme-de.txt
-r-xr-xr-x. 1 root root 6171 Apr 10  2018 readme-es.txt
-r-xr-xr-x. 1 root root 7297 Apr 10  2018 readme-fr.txt
-r-xr-xr-x. 1 root root 7057 Apr 10  2018 readme-ja.txt
-r-xr-xr-x. 1 root root 6543 Apr 10  2018 readme-ko.txt
-r-xr-xr-x. 1 root root 5561 Apr 10  2018 readme.txt
-r-xr-xr-x. 1 root root 5563 Apr 10  2018 readme-zh-CN.txt
-r-xr-xr-x. 1 root root 5457 Apr 10  2018 readme-zh-TW.txt
dr-xr-xr-x. 2 root root 2048 Apr 10  2018 umds
dr-xr-xr-x. 3 root root 2048 Apr 10  2018 vcsa
dr-xr-xr-x. 7 root root 2048 Apr 10  2018 vcsa-cli-installer
dr-xr-xr-x. 5 root root 2048 Apr 10  2018 vcsa-ui-installer


devops@devops-VirtualBox:/media/devops/VMware VCSA/vcsa-cli-installer/lin64$ pwd
/media/devops/VMware VCSA/vcsa-cli-installer/lin64

