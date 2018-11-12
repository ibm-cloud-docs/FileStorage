---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Mounting {{site.data.keyword.filestorage_short}} on CoreOS

CoreOS is a powerful Linux distribution that is built to make large, scalable deployments on varied infrastructure simple to manage. Based on a build of Chrome OS, CoreOS maintains a lightweight host system and uses Docker containers for all applications.

## Mounting Portable Storage

All secondary mount files go in the `/etc/systemd/system` directory as the system level mounts are in a directory that is read-only in CoreOS. First, you must create a `MOUNTPOINT.mount` file. The **Where** section of the `.mount` file must match the file name. If the mount point is not directly off the `/`, you must name the file by using the syntax `path-to-mount.mount`. For example, if you want to mount the portable storage drive to `/mnt/www`, name the file `mnt-www.mount`.

You can use `fdisk` or `parted` to create the partition. Make sure that the file system you create matches the one listed in the `.mount` file or the service fails to start.


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
{:codeblock}


CoreOS uses `systemd`, so to make the mount point survive a restart, you must enable the `*.mount` file. If you use the `--now` flag, the partition is mounted immediately and set to start on boot.

```
$ systemctl enable --now mnt-www.mount
```
{:pre}

## Mounting NFS/{{site.data.keyword.filestorage_short}}

The process for mounting the {{site.data.keyword.filestorage_short}} is the same. Because the mount is NFS, you can specify more options by using the `Options=` line in the mount file.

In the example, NFS is set to mount at `/data/www`. The NFS mount point of the {{site.data.keyword.filestorage_short}} instance can be obtained from the {{site.data.keyword.filestorage_short}} listing page or through an API call `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

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
{:codeblock}

Next, enable the mount and check that it is mounted properly.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}

## Mounting NAS/CIFS

Mounting a CIFS share isn't natively supported in CoreOS but there's an easy workaround to allow the host system to mount NAS shares. You can use a container to build the `mount.cfis` module, and then copy it to the CoreOS system

On the CoreOS system, run the following to download and drop in to a Fedora container.

```
docker run -t -i -v /tmp:/host_tmp fedora /bin/bash
```
{:pre}

When you're in the container, run the following to build the CIFS utility.

```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
{:codeblock}

Now that the `mount.cifs` file is copied to the host you can exit the docker container by entering the `exit` command or pressing **Ctrl+d**. When you're back in the CoreOS system, you can mount the CIFS share with the following command:
```
/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount
```
{:pre}

## Mounting ISCSI

iSCSI isn't supported currently in CoreOS but is in line for a future release. - https://github.com/coreos/bugs/issues/634
