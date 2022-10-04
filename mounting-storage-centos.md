---

copyright:
  years: 2014, 2019
lastupdated: "2019-11-14"

keywords: File Storage, mounting file storage, Linux, CentOS, NFS

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
{:codeblock: .codeblock}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

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

You can authorize a host to access the {{site.data.keyword.filestorage_short}} volume through the [{{site.data.keyword.cloud}} console](/classic/storage/file){: external}.

1. In the console, go to **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Scroll to the File share you want to mount, and click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions"). Then, select **Authorize Host**.
3. Filter the available host list by selecting the device type, subnet, or IP address.

   When the list is filtered by subnet, the subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {: note}

4. Select one or more hosts from the list and click **Save**.

## Authorizing the host from the SLCLI
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

## Mounting the storage
{: #mountingStorageCentOS}

Most of the steps that you need to perform to mount your {{site.data.keyword.filestorage_short}} to a CentOS host are the same as the ones that are described on [Mounting {{site.data.keyword.filestorage_short}} on Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux). However, for CentOS, you can specify some additional options by using the `Options=` line in the mount file. In the following example, the NFS is set to mount at `/data/www`.

 The NFS mount point information can be obtained from the {{site.data.keyword.filestorage_short}} Details page in the UI or through an API call -`SoftLayer_Network_Storage::getNetworkMountAddress()`.
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
