---

copyright:
  years: 2014, 2021
lastupdated: "2020-12-16"

keywords: File Storage, NFS, mounting File Storage, mounting storage on Linux,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}
{:term: .term}

# Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux
{: #mountingLinux}

Use these instructions to connect a Red Hat Enterprise Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a Network File System (NFS) share.
{:shortdesc}

First, make sure that the host that is to access the {{site.data.keyword.filestorage_full}} volume is authorized through the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/file){: external}.

1. In the console, go to **Classic Infrastructure**  > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Scroll to the File share you want to mount, and click the ellipsis (**...**) for Actions. Then, select **Authorize Host**.
3. Filter the available host list by selecting the device type, subnet, or IP address.

   When the list is filtered by subnet, the subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {:note}
4. Select one or more hosts from the list and click **Save**.

Alternatively, you can authorize the hosts through the SLCLI.
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
{: #mountRHEL}

The example is based on RHEL 7. The steps can be adjusted for other Linux&reg; distributions according to the operating system's (OS) vendor documentation.
{:tip}

1. Install the required tools.
   ```
   # yum install nfs-utils
   ```
   {:pre}

2. Mount the remote share.
   ```
   # mount -t nfs -o <options> <host:mount_point> /mnt
   ```

   Example
   ```
   # mount -t nfs -o nfsvers=3 nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

   The mount point information can be obtained from the {{site.data.keyword.filestorage_short}} Details page in the UI or through an API call - `SoftLayer_Network_Storage::getNetworkMountAddress()`.
   {:tip}


3. Verify that the mount was successful by using the disk file system command.
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

   The files that are created by root have ownership of `nobody:nobody`. To display ownership correctly, `idmapd.conf` needs to be updated with the correct domain settings. For more information, see the [How to implement no_root_squash for NFS](#norootsquash) section.
   {:tip}

5. Mount the remote share on start. To complete the setup, edit the file systems table (`/etc/fstab`) to add the remote share to the list of entries that are automatically mounted on startup:

   ```
   (hostname):/(username) /mnt nfs_version options 0 0
   ```

   Examples:

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs nfsvers=3 0 0
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs options 0 0
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 options 0 0
   ```

6. Verify that the configuration file has no errors.

   ```
   # mount -fav
   ```
   {:pre}

   If the command completes with no errors, your setup is complete.

   If you're using NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues.
   {:tip}

   If your host OS is CentOS, you can configure more options. For more information, see [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/FileStorage?topic=FileStorage-mountingCentOS).


## Implementing `no_root_squash` for NFS (optional)
{: #norootsquash}

By default, NFS downgrades any files that were created with the root permissions to the nobody user. This security feature prevents privileges from being shared unless they are requested.

Configuring `no_root_squash` allows root clients to retain root permissions on the remote NFS share.
- For NFSv3, clients don't need to anything; `no_root_squash` simply works.
- For NFSv4.1, you need to set the nfsv4 domain to: `slnfsv4.com` and start `rpcidmapd`, or a similar service that is used by your OS.

Example

1. From the host, set domain setting in `/etc/idmapd.conf`.

   ```
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The following should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

2. Run `nfsidmap -c`.
3. Start `rpcidmapd`.
   ```
   systemctl start rpcidmapd
   systemctl enable rpcidmapd
   ```

## Unmounting the file system
{: #umountRHEL}

To unmount any currently mounted file system on your host, run the `umount` command with disk name or mount point name.

```
umount /dev/sdb
```
{:pre}

```
umount /mnt
```
{:pre}
