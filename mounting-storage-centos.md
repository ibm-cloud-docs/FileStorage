---

copyright:
  years: 2014, 2023
lastupdated: "2013-04-19"

keywords: File Storage, mounting File Storage, Linux, CentOS, NFS

subcollection: FileStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{{site.data.keyword.attribute-definition-list}}

# Mounting {{site.data.keyword.filestorage_short}} in CentOS
{: #mountingCentOS}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

To mount {{site.data.keyword.filestorage_full}} in CentOS 7, you must authorize the host first. Then, install the NFS utilities as described in [Mounting {{site.data.keyword.filestorage_short}} on Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux).
{: shortdesc}

## Authorizing the host in the UI
{: #authCentOShostUI}
{: ui}

You can authorize a host to access the {{site.data.keyword.filestorage_short}} volume through the [{{site.data.keyword.cloud}} console](/cloud-storage/file){: external}.

1. In the console, go to **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Scroll to the File share that you want to mount, and click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions"). Then, select **Authorize Host**.
3. Filter the available host list by selecting the device type, subnet, or IP address.

   When the list is filtered by subnet, the subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {: note}

4. Select one or more hosts from the list and click **Save**.

## Authorizing the host from the CLI
{: #authCentOShostCLI}
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
{: #authhCentOShostterraform}
{: terraform}

To authorize a Compute host to access the share, use the `ibm_storage_file` resource and specify the `allowed_virtual_guest_ids` for virtual servers, or `allowed_hardware_ids` for bare metal servers. Specify `allowed_ip_addresses` to define which IP addresses have access to the storage. 

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

After your storage resource is created, you can access the `hostname` and `volumename` attributes, which you can use to determine the mount target later. For example, a File Storage resource with the `hostname` argument set to `nfsdal0901a.service.softlayer.com` and the `volumename` argument set to `IBM01SV278685_7` has the mount point `nfsdal0901a.service.softlayer.com:-IBM01SV278685_7`.

For more information about the arguments and attributes, see [ibm_storage_file](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_file){: external}.

## Mounting the storage
{: #mountingStorageCentOS}

Most of the steps that are required for mounting your {{site.data.keyword.filestorage_short}} to a CentOS host are the same as the ones that are described in [Mounting {{site.data.keyword.filestorage_short}} on Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux). However, for CentOS, you can specify some additional options by using the `Options=` line in the mount file. In the following example, the NFS is set to mount at `/data/www`.

The mount point information can be obtained from the {{site.data.keyword.filestorage_short}} Details page in the UI, with an API call - `SoftLayer_Network_Storage::getNetworkMountAddress()`, or by looking at the `ibm_storage_file` resource in Terraform.
{: tip}

```text
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
{: codeblock}

Next, enable the mount and check that it is mounted properly.

```zsh
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{: codeblock}
