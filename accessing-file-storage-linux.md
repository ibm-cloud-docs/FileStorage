---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-22"

keywords: File Storage, NSF, mounting File Storage, mounting storage on Linux, auxiliary storage

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Mounting {{site.data.keyword.filestorage_short}} on Linux
{: #mountingLinux}

First, make sure that the host that is to access the {{site.data.keyword.filestorage_full}} volume is authorized through the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.

1. From the {{site.data.keyword.filestorage_short}} listing page, click the **Actions** link that is associated with the new share and click **Authorize Host**.
2. Select the host or hosts from the list and click **Submit**. This action authorizes the host to access the share.

Alternatively, you can authorize the hosts through the SLCLI.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to
                            authorize
  --help                    Show this message and exit.
```

## Mounting the {{site.data.keyword.filestorage_short}} share

Use these instructions to connect a Linux-based {{site.data.keyword.BluSoftlayer_full}} Compute instance to a Network File System (NFS) share. The example is based on Red Hat Enterprise Linux 6. The steps can be adjusted for other Linux distributions according to the operating system (OS) vendor documentation.

The mount point of the file storage instance can be obtained from the {{site.data.keyword.filestorage_short}} listing page or through an API call - `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

1. Install the required tools.
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}

2. Mount the remote share.
   ```
   # mount -t "nfs version" -o "options" <mount_point> /mnt
   ```

   Example
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

3. Verify that the mount was successful.
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```

4. Go to the mount point, and read/write files.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   The files that are created by root have ownership of `nobody:nobody`. To display ownership correctly, `idmapd.conf` needs to be updated with the correct domain settings. See the [How to implement no_root_squash for NFS](#norootsquash) section.
   {:tip}

5. Mount the remote share on start. To complete the setup, edit the file systems table (`/etc/fstab`) to add the remote share to the list of entries that are automatically mounted on startup:

   ```
   (hostname):/(username) /mnt "nfs version" "options" 0 0
   ```

   Example

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```

6. Verify that the configuration file has no errors.

   ```
   # mount -fav
   ```
   {:pre}

   If the command completes with no errors, your setup is complete.

   If you're using NFS 4.1, add `sec=sys` to the mount command to prevent file ownership issues.
   {:tip}

   If your host OS is CentOS, you can configure more options. For more information, see [Mounting {{site.data.keyword.filestorage_short}} in CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS).


## Implementing `no_root_squash` for NFS (optional)
{: #norootsquash}

Configuring `no_root_squash` allows root clients to retain root permissions on the NFS share.
- For NFSv3, there is nothing that clients need to do; `no_root_squash` works.
- For NFSv4, you need to set the nfsv4 domain to: `slnfsv4.com` and start `rpcidmapd`, or a similar service that is used by your OS.

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
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
