---

copyright:
  years: 2014, 2024
lastupdated: "2024-05-29"

keywords: File Storage for Classic, NFS, mounting File Storage, mounting storage on Ubuntu,

subcollection: FileStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{{site.data.keyword.attribute-definition-list}}

# Mounting {{site.data.keyword.filestorage_short}} on Ubuntu
{: #mountingUbuntu}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

Use these instructions to connect an Ubuntu Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a Network File System (NFS) share. For more information about how to order {{site.data.keyword.filestorage_full}}, see the [Getting started](/docs/FileStorage?topic=FileStorage-getting-started) tutorial.
{: shortdesc}

Before you begin, make sure that the host that is to access the {{site.data.keyword.filestorage_short}} volume is authorized. For more information, see [Authorizing the host in the UI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/FileStorage?topic=FileStorage-managingstorage&interface=terraform#authhostterraform){: terraform}.
{: requirement}

## Mounting the {{site.data.keyword.filestorage_short}} share
{: #mountUbuntu}

1. Install the required tools.
   ```zsh
   # apt-get install nfs-common
   ```
   {: pre}

2. Mount the remote share.
   ```zsh
   # mount -t nfs -o <options> <host:/mount_point> /mnt
   ```

   Example for `storage_as_a_service` volumes.
   ```text
   #mount -t nfs -o nfsvers=3 fsf-wdc0403a-fz.service.softlayer.com:/IBM02SEV1414935_66/data01 /mnt
   ```

   Example for `enterprise` volumes.
   ```text
   # mount -t nfs -o nfsvers=3 nfshou0201d-fz.service.softlayer.com:/IBM01SEV1414935_2 /mnt
   ```

   The mount point information can be obtained from the {{site.data.keyword.filestorage_short}} Details page in the UI, with an API call - `SoftLayer_Network_Storage::getNetworkMountAddress()`, or by looking at the `ibm_storage_file` resource in Terraform.
   {: tip}


3. Verify that the mount was successful by using the disk file system command.
   ```text
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2  25G  1.4G  22G    6%   /
   /tmpfs     1.9G     0 1.9G    0%   /dev/shm
   /dev/xvda1 97M    51M  42M   55%
   ```

4. Go to the mount point, and read/write files.
   ```text
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x   2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root   root   4096 Sep 8 14:30 ..
   -rw-r--r--   1 nobody nobody    0 Sep 8 15:52 test
   ```

5. Make the configuration persistent by editing the file systems table (`/etc/fstab`). Add the remote share to the list of entries that are automatically mounted on startup:

   ```zsh
   sudo nano /etc/fstab
   ```
   Add a line with the following syntax to the end of the file.

   ```zsh
   (hostname):/(mount_point) /mnt nfs_version defaults 0 0
   ```

   Example

   ```text
   nfsdal1301a.service.softlayer.com:/IBM01SV278685_7 /mnt nfsvers=3 defaults 0 0
   ```

6. Verify that the configuration file has no errors.

   ```zsh
   # mount -fav
   ```
   {: pre}

   If the command completes with no errors, your setup is complete.

   If you're using NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues.
   {: tip}


## Unmounting the file system
{: #umountUbuntu}

To unmount any currently mounted file system on your host, run the `umount` command with disk name or mount point name.

```zsh
umount /dev/sdb
```
{: pre}

```zsh
umount /mnt
```
{: pre}
