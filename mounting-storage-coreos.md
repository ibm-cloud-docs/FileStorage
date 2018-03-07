---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Mounting {{site.data.keyword.filestorage_short}} on CoreOS

CoreOS is a powerful Linux distribution built to make large, scalable deployments on varied infrastructure simple to manage. Based on a build of Chrome OS, CoreOS maintains a lightweight host system and uses Docker containers for all applications.

## Mounting Portable Storage

All secondary mount files go in the */etc/systemd/system* directory as the system level mounts are in a directory that is read-only in CoreOS. You will create a MOUNTPOINT.mount file. The Where section of the .mount file must match the filename. If the mount point is not directly off of / you have to name the file using the following syntax: path-to-mount.mount. As you can see in the following example we are wanting to mount the portable storage drive to `/mnt/www` so we name the file `mnt-www.mount`.

You have to use fdisk or parted to create the partition and make sure that the filesystem you create matches the one listed in the `.mount` file or else the service will fail to start.


```
[Unit]
Description = Mount for Portable Storage

[Mount]
What=/dev/xvdc1
Where=/mnt/www
Type=ext4

[Install]
WantedBy = multi-user.target
```

CoreOS uses systemd so in order to make the mount point survive a reboot you must enable the `*.mount` file. If you use the `--now` flag the partition will be mounted immediately and set to start on boot.

`$ systemctl enable --now mnt-www.mount`

## Mounting NFS/{{site.data.keyword.filestorage_short}}

The process for mounting our Endurance/Performance {{site.data.keyword.filestorage_short}} is pretty much the same, but since the mount is NFS we can specify some additional options using the Options= line in the mount file. In the following example we are setting the NFS to mount at `/data/www`. Note the NFS mount point of the {{site.data.keyword.filestorage_short}} instance can be obtained from the {{site.data.keyword.filestorage_short}} listing page or via call to API -SoftLayer_Network_Storage::getNetworkMountAddress().

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=4,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```

We can now enable the mount and check that it is mounted properly.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
 
## Mounting NAS/Cifs

Mounting a cifs share is not natively supported in CoreOS but there is a pretty easy workaround to allow the host system to mount NAS shares. You will use a container to build the mount.cfis module and then copy it to the CoreOS system
 
On the CoreOS system run the following to download and drop in to a Fedora container:  <br/>
`docker run -t -i -v /tmp:/host_tmp fedora /bin/bash`
 
Once you are in the container you will want to run the following to build the cifs utility
```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
 
Now that the mount.cifs file has been copied to the host machine you can exit the docker container by issuing the `exit` command or pressing **ctrl+d**. When you are back in the CoreOS system you can mount the CIFS share with the following command: <br/>
`/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount`
 
## Mounting ISCSI

This is currently not supported in CoreOS but is in line for a future release - https://github.com/coreos/bugs/issues/634
