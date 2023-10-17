---

copyright:
  years: 2014, 2023
lastupdated: "2023-10-17"

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

Before you begin, make sure that the host that is to access the {{site.data.keyword.filestorage_short}} volume is authorized. For more information see [Authorizing the host in the UI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/FileStorage?topic=FileStorage-managingstorage&interface=cli#authhostCLI){: cli.}[Authorizing the host with Terraform](/docs/FileStorage?topic=FileStorage-managingstorage&interface=cli#authhostTerraform){: terraform}.

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
