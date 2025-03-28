---

copyright:
  years: 2014, 2025
lastupdated: "2025-03-14"

keywords: File Storage for Classic, NFS, mounting File Storage, mounting storage on Linux,

subcollection: FileStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{{site.data.keyword.attribute-definition-list}}

# Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux
{: #mountingLinux}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

Use these instructions to connect a Red Hat Enterprise Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a Network File System (NFS) share. For more information about how to order {{site.data.keyword.filestorage_full}}, see the [Getting started](/docs/FileStorage?topic=FileStorage-getting-started) tutorial.
{: shortdesc}

Before you begin, make sure that the host that is to access the {{site.data.keyword.filestorage_short}} volume is authorized. For more information, see [Authorizing the host in the console](/docs/FileStorage?topic=FileStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/FileStorage?topic=FileStorage-managingstorage&interface=terraform#authhostterraform){: terraform}.
{: requirement}

## Mounting the {{site.data.keyword.filestorage_short}} share
{: #mountRHEL}

1. Install the required tools.
   ```sh
   # yum install nfs-utils
   ```
   {: pre}

1. Create a directory in your instance.

   ```sh
   mkdir /mnt/test
   ```
   {: pre}

1. Obtain the mount point information from the {{site.data.keyword.filestorage_short}} Details page in the console, with an API call (`SoftLayer_Network_Storage::getNetworkMountAddress()`), or by looking at the `ibm_storage_file` resource in Terraform.

1. Mount the remote share.
   ```sh
   # mount -t nfs -o <options> <host:mount_point> /mnt
   ```
   {: pre}

   Example for `storage_as_a_service` volumes.
   ```text
   #mount -t nfs -o nfsvers=3 fsf-wdc0403a-fz.service.softlayer.com:/IBM02SEV1414935_66/data01 /mnt
   ```
   {: screen}

   Example for `enterprise` volumes.
   ```text
   # mount -t nfs -o nfsvers=3 nfshou0201d-fz.service.softlayer.com:/IBM01SEV1414935_2 /mnt
   ```
   {: screen}

   If you're using NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues. If you're using NFSv3 or NFS 4.1, add `_netdev` to wait for the storage to be mounted after all network components are started. For more information about the `mount` command and its options, see the [mount(8) manual page](https://man7.org/linux/man-pages/man8/mount.8.html){: external}.
   {: tip}

1. Verify that the mount was successful by using the disk file system command.
   ```text
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2  25G  1.4G  22G    6%   /
   /tmpfs     1.9G     0 1.9G    0%   /dev/shm
   /dev/xvda1 97M    51M  42M   55%
   ```
   {: screen}

1. Go to the mount point, and read/write files.
   ```text
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x   2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root   root   4096 Sep 8 14:30 ..
   -rw-r--r--   1 nobody nobody    0 Sep 8 15:52 test
   ```
   {: screen}

   The files that are created by root have ownership of `nobody:nobody`. To display ownership correctly, the `idmapd.conf` needs to be updated with the correct domain settings. For more information, see the [Implementing `no_root_squash` for NFS (optional)](#norootsquash) section.
   {: tip}

5. Mount the remote share on start. To complete the setup, edit the file systems table (`/etc/fstab`) to add the remote share to the list of entries that are automatically mounted on startup:

   ```text
   (hostname):/(username) /mnt nfs_version options 0 0
   ```

   See the following examples.

   ```text
   fsf-wdc0403a-fz.service.softlayer.com:/IBM02SEV1414935_66/data01 /mnt nfs nfsvers=3 0 0
   fsf-wdc0403a-fz.service.softlayer.com:/IBM02SEV1414935_66/data01 /mnt nfs options 0 0
   fsf-wdc0403a-fz.service.softlayer.com:/IBM02SEV1414935_66/data01 /mnt nfs4 options 0 0
   fsf-wdc0403a-fz.service.softlayer.com:/IBM02SEV1414935_66/data01 /mnt nfs _netdev,nfsvers=3 0 0
   ```
   {: screen}

   For more information, see [An introduction to the Linux `/etc/fstab` file](https://www.redhat.com/en/blog/etc-fstab){: external}.

6. Verify that the configuration file has no errors.

   ```sh
   # mount -fav
   ```
   {: pre}

   If the command completes with no errors, your setup is complete.

   If your host OS is CentOS, you can configure more options. For more information, see [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS).

### Managing user permissions to the content of the mounted file share
{: #rhel-user-group-permissions}

As a system administrator, you can manage the access to data on the mounted file storage volume. After the file share is mounted, you can refine access control by using the `chown` and `chmod` commands to assign read, write, and execute permissions to individual users and groups. For more information, see [Red Hat's tutorial: How to manage Linux permissions for users, groups, and others](https://www.redhat.com/en/blog/manage-permissions){: external}.

## Implementing `no_root_squash` for NFS (optional)
{: #norootsquash}

By default, NFS downgrades any files that were created with the root permissions to the nobody user. This security feature prevents privileges from being shared unless they are requested.

Configuring `no_root_squash` allows root clients to retain root permissions on the remote NFS share.
- For NFSv3, clients don't need to do anything; the `no_root_squash` simply works.
- For NFSv4.1, you need to set the nfsv4 domain to: `slnfsv4.com` and start `rpcidmapd`, or a similar service that is used by your OS.

Example

1. From the host, set the domain setting in `/etc/idmapd.conf`.

   ```text
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The domain value should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

2. Run `nfsidmap -c`.
3. Start `rpcidmapd`.
   ```sh
   systemctl start rpcidmapd
   systemctl enable rpcidmapd
   ```

## Unmounting the file system
{: #umountRHEL}

To unmount any currently mounted file system on your host, run the `umount` command with disk name or mount point name.

```sh
umount /dev/sdb
```
{: pre}

```sh
umount /mnt
```
{: pre}
