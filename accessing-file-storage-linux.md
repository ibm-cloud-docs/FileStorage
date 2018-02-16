---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Accessing {{site.data.keyword.filestorage_full_notm}} on Linux

Before starting, make sure the host accessing the {{site.data.keyword.filestorage_full}} volume has been authorized through the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. From the {{site.data.keyword.filestorage_short}} listing page, click **Actions** associated with the newly provisioned share and click **Authorize Host**.
2. Select the desired host(s) from the list and click **Submit**; this authorizes the host(s) to access the share.

## Mounting the {{site.data.keyword.filestorage_short}} share

Following are the steps required to connect a Linux-based {{site.data.keyword.BluSoftlayer_full}} Compute instance to a Network File System (NFS) share. The example is based on Red Hat Enterprise Linux 6. The steps can be adjusted for other Linux distributions according to the operating system (OS) vendor documentation.

**Note:** The mount point of the file storage instance can be obtained from the {{site.data.keyword.filestorage_short}} listing page or via call to API  - SoftLayer_Network_Storage::getNetworkMountAddress().

1. Install required packages/tools.

    `[root@TEST-RHEL6]# yum -y install nfs-utils nfs-utils-lib
    `
2. Mount the remote share
    `[root@TEST-RHEL6]# mount -t "nfs version" -o "options" <mount_point> /mnt`
    
    Here is an example of mounting the remote share to a storage instance.
    
    `[root@TEST-RHEL6 mnt]# mount -t nfs4 -o hard,intr`
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt`
 
3. Verify the mount was successful.

    `[root@TEST-RHEL6]# df -h`
    
    `Filesystem Size Used Avail Use% Mounted on`
    
    `/dev/xvda2 25G 1.4G 22G 6% /`
    
    `tmpfs 1.9G 0 1.9G 0% /dev/shm`
    
    `/dev/xvda1 97M 51M 42M 55%`
    
4. Navigate to the mount point and read/write files.

    `[root@TEST-RHEL6]# touch /mnt/test`
    
    `[root@TEST-RHEL6]# ls -la /mnt`
    
    `total 12`
    
    `drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .`
    
    `dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..`
    
    `-rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test`

    **Note:** The files created by root will have ownership of nobody:nobody. In order to display ownership correctly idmapd.conf needs to be updated with the correct domain settings. See “How to implement no_root_squash for NFS” below.
    
5. Mount the remote share on boot. In order to complete the setup, edit the file systems table /etc/fstab to add the remote share to the list of entries that will be automatically mounted on startup:

    `(hostname):/(username) /mnt "nfs version" "options" 0 0`
    
    Using the instance from the mounting the remote share example, the entry would be:
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0`
    
6.  Verify there are no errors with the configuration file.

    `[root@TEST-RHEL6 /]# mount -fav`
    
    If the command completes with no errors, your setup is complete.

**Note:** If using NFS 4.1, add 'sec=sys' to the mount command to prevent file ownership issues.

 
## How to implement no_root_squash for NFS (optional)

Configuring no_root_squash allows root clients to retain root permissions on the NFS share. For NFSv3, there is nothing that clients will need to do; no_root_squash should just work.
For NFSv4, you will need to set the nfsv4 domain to: slnfsv4.com and start rpcidmapd, or similar service depending on OS version.

Here’s an example:

1. From the host, set domain setting in /etc/idmapd.conf.

    `#vi /etc/idmapd.conf`
    
    `[General]`
    
    `#Verbosity = 0`
    
    `#The following should be set to the local NFSv4 domain name`
    
    `#The default is the host's DNS domain name.`
    
    `Domain = slnfsv4.com`
    
    `[Mapping]`
    
    `Nobody-User = nobody`
    
    `Nobody-Group = nobody`
    
2. Execute nfsidmap -c.
3. Start rpcidmapd.

   ` # /etc/init.d/rpcidmapd start`
   
   ` Starting RPC idmapd: [ OK ]`
