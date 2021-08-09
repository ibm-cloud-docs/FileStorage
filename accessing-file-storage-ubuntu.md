---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-18"

keywords: File Storage, NFS, mounting File Storage, mounting storage on Ubuntu,

subcollection: FileStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}
{:term: .term}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Mounting {{site.data.keyword.filestorage_short}} on Ubuntu
{: #mountingUbuntu}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

Use these instructions to connect an Ubuntu Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a Network file system (NFS) share. For more information about how to order {{site.data.keyword.filestorage_full}}, see the [Getting started](/docs/FileStorage?topic=FileStorage-getting-started) tutorial.
{: shortdesc}

First, make sure that the host that is to access the {{site.data.keyword.filestorage_short}} volume is authorized.

## Authorizing the host in the UI
{: #authUbuntuhostUI}
{: ui}

You can authorize a host to access the {{site.data.keyword.filestorage_short}} volume through the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/file){: external}.

1. In the console, go to **Classic Infrastructure**  > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Scroll to the File share you want to mount, and click the ellipsis (**...**) for Actions. Then, select **Authorize Host**.
3. Filter the available host list by selecting the device type, subnet, or IP address.

   When the list is filtered by subnet, the subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {: note}
4. Select one or more hosts from the list and click **Save**.

## Authorizing the host from the SLCLI
{: #authUbuntuhostCLI}
{: cli}

You can authorize a host to access the {{site.data.keyword.filestorage_short}} volume by using the `file access-authorize` command.

```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  -s, --subnet-id TEXT      An ID of one subnet to authorize.
  --help                    Show this message and exit.
```

## Mounting the {{site.data.keyword.filestorage_short}} share
{: #mountUbuntu}

1. Install the required tools.
   ```
   # apt-get install nfs-common
   ```
   {: pre}

2. Mount the remote share.
   ```
   # mount -t nfs -o <options> <host:/mount_point> /mnt
   ```

   Example
   ```
   # mount -t nfs -o nfsvers=3 nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

   The mount point information can be obtained from the {{site.data.keyword.filestorage_short}} Details page in the UI or through an API call - `SoftLayer_Network_Storage::getNetworkMountAddress()`.
   {: tip}


3. Verify that the mount was successful by using the disk filesystem command.
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2  25G  1.4G  22G    6%   /
   /tmpfs     1.9G     0 1.9G    0%   /dev/shm
   /dev/xvda1 97M    51M  42M   55%
   ```

4. Go to the mount point, and read/write files.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x   2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root   root   4096 Sep 8 14:30 ..
   -rw-r--r--   1 nobody nobody    0 Sep 8 15:52 test
   ```

5. Make the configuration persistent by editing the file systems table (`/etc/fstab`). Add the remote share to the list of entries that are automatically mounted on startup:

   ```
   sudo nano /etc/fstab
   ```
   Add a line with the following syntax to the end of file.

   ```
   (hostname):/(mount_point) /mnt nfs_version defaults 0 0
   ```

   Example

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfsvers=3 defaults 0 0
   ```

6. Verify that the configuration file has no errors.

   ```
   # mount -fav
   ```
   {: pre}

   If the command completes with no errors, your setup is complete.

   If you're using NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues.
   {: tip}


## Unmounting the file system
{: #umountUbuntu}

To unmount any currently mounted file system on your host, run the `umount` command with disk name or mount point name.

```
umount /dev/sdb
```
{: pre}

```
umount /mnt
```
{: pre}
