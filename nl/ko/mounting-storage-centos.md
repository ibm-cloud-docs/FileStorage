---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-22"

keywords: File Storage, mounting file storage, Linux, CentOS, NFS

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# CentOS에서 {{site.data.keyword.filestorage_short}} 마운트
{: #mountingCentOS}

CentOS 7에서 {{site.data.keyword.filestorage_full}}를 마운트하려면 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window} 또는 SLCLI를 통해 호스트에 먼저 권한을 부여해야 합니다. 그런 다음 [Linux에 {{site.data.keyword.filestorage_short}} 마운트](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)에 설명된 대로 NFS 유틸리티를 설치하십시오.

CentOS의 경우 마운트 파일에 `Options=` 행을 사용하여 추가 옵션을 지정할 수 있습니다. 다음 예에서는 NFS가 `/data/www`에 마운트되도록 설정되어 있습니다.

{{site.data.keyword.filestorage_short}} 인스턴스의 NFS 마운트 지점은 {{site.data.keyword.filestorage_short}} 목록 페이지에서 또는 API호출 - `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 가져올 수 있습니다.
{:tip}

```
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
{:codeblock}

그 후 마운트를 사용으로 설정하고 올바르게 마운트되었는지 확인하십시오.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
