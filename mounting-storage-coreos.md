---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-08"

keywords: File Storage, file storage, NFS, mounting volume in Container Linux, CoreOS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:shortdesc: .shortdesc}


# Mounting {{site.data.keyword.filestorage_short}} on Container Linux
{: #mountingCoreOS}

Container Linux by CoreOS is an open source, lightweight operating system based on the Linux kernel. It is designed for providing infrastructure to clustered deployments. As an operating system, Container Linux provides the minimal functionality that is required for deploying applications inside software containers, together with built-in mechanisms for service discovery and configuration sharing. For more information, see the [Mounting storage](https://coreos.com/os/docs/latest/mounting-storage.html).
{:shortdesc}

All secondary mount files go in the `/etc/systemd/system` directory as the system level mounts are in a directory that is read-only. First, you must create a `MOUNTPOINT.mount` file. The **Where** section of the `.mount` file must match the file name. If the mount point is not directly off the `/`, you must name the file by using the syntax `path-to-mount.mount`. For example, if you want to mount the portable storage drive to `/mnt/www`, name the file `mnt-www.mount`.

You can use `fdisk` or `parted` to create the partition. Make sure that the file system that you create matches the one listed in the `.mount` file or the service fails to start. Because the mount is NFS, you can specify more options by using the `Options=` line in the mount file.

In the following example, NFS is set to mount at `/data/www`. The NFS mount point of the {{site.data.keyword.filestorage_short}} instance can be obtained from the {{site.data.keyword.filestorage_short}} listing page or through an API call `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

```
$ cat data-www.mount
[Unit]
Description = Mount for Container Storage

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=3,sec=sys,noauto

[Install]
WantedBy = multi-user.target
```
{:codeblock}

Next, enable the mount and check that it is mounted properly.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs3 (rw,relatime,vers=3.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}

For more information, see [`systemd mount` documentation](https://www.freedesktop.org/software/systemd/man/systemd.mount.html).
