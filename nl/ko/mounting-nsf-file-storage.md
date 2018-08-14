---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# CentOS에서 NFS/{{site.data.keyword.filestorage_short}} 마운트

CentOS 7에서 {{site.data.keyword.filestorage_full}}를 마운트하는 프로세스는 [RHEL 6에서 {{site.data.keyword.filestorage_short}} 마운트](accessing-file-storage-linux.html) 프로세스와 유사합니다. 그러나 마운트가 NFS이므로, 사용자는 마운트 파일에서 `Options=` 행을 사용하여 몇 가지 추가 옵션을 지정할 수 있습니다. 다음 예에서는 NFS가 `/data/www`에 마운트되도록 설정되어 있습니다.  

>**참고** - {{site.data.keyword.filestorage_short}} 인스턴스의 NFS 마운트 지점은 {{site.data.keyword.filestorage_short}} 목록 페이지에서, 또는 API 호출 `SoftLayer_Network_Storage::getNetworkMountAddress()`를 통해 얻을 수 있습니다. 

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
