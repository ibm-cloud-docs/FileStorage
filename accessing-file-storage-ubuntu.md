---

copyright:
  years: 2014, 2023
lastupdated: "2023-04-19"

keywords: File Storage, NFS, mounting File Storage, mounting storage on Ubuntu,

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

First, make sure that the host that is to access the {{site.data.keyword.filestorage_short}} volume is authorized.

## Authorizing the host in the UI
{: #authUbuntuhostUI}
{: ui}

You can authorize a host to access the {{site.data.keyword.filestorage_short}} volume in the [{{site.data.keyword.cloud}} console](/classic/storage/file){: external}.

1. In the console, go to **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Scroll to the File share that you want to mount, and click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions"). Then, select **Authorize Host**.
3. Filter the available host list by selecting the device type, subnet, or IP address.

   When the list is filtered by subnet, the subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {: note}

4. Select one or more hosts from the list and click **Save**.

After the host is authorized, go to the {{site.data.keyword.filestorage_short}} Details page, and take note of the mountpoint information.

## Authorizing the host from the SLCLI
{: #authUbuntuhostCLI}
{: cli}

You can authorize a host to access the {{site.data.keyword.filestorage_short}} volume by using the `file access-authorize` command.

```python
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

## Authorizing the host with Terraform
{: #authhLinuxhostterraform}
{: terraform}

To authorize a compute host to access the share, use the `ibm_storage_file` resource and specify the `allowed_virtual_guest_ids` for virtual servers, or `allowed_hardware_ids` for bare metal servers. Specify `allowed_ip_addresses` to define which IP addresses have access to the storage. 

The following example defines that the Virtual Server with the ID `28961689` can access the volume from the `10.146.139.64/26` subnet, and `10.146.139.84` address.

```terraform
resource "ibm_storage_file" "fs_endurance" {
  type       = "Endurance"
  datacenter = "dal09"
  capacity   = 20
  iops       = 0.25

  allowed_virtual_guest_ids = ["28961689"]
  allowed_subnets           = ["10.146.139.64/26"]
  allowed_ip_addresses      = ["10.146.139.84"]
  snapshot_capacity         = 10
  hourly_billing            = true
}
```
{: codeblock}

After your storage resource is created, you can access the `hostname` and `volumename` attributes, which you can use to determine the mount target later. For example, a file storage resource with the `hostname` argument set to `nfsdal0901a.service.softlayer.com` and the `volumename` argument set to `IBM01SV278685_7` has the mount point `nfsdal0901a.service.softlayer.com:-IBM01SV278685_7`.

For more information about the arguments and attributes, see [ibm_storage_file](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_file){: external}.

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
   Add a line with the following syntax to the end of file.

   ```zsh
   (hostname):/(mount_point) /mnt nfs_version defaults 0 0
   ```

   Example

   ```text
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfsvers=3 defaults 0 0
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
