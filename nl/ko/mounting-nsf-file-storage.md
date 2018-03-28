---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# CentOS에서 NFS/{{site.data.keyword.filestorage_short}} 마운트

CentOS 7에서 Endurance/Performance {{site.data.keyword.filestorage_full}}를 마운트하는 프로세스는 [RHEL 6에서 {{site.data.keyword.filestorage_short}} 마운트](https://console.stage1.bluemix.net/docs/infrastructure/FileStorage/accessing-file-storage-linux.html) 프로세스와 거의 비슷하지만, 마운트는 NFS이기 때문에 mount 파일에서 *Options=* 행을 사용하여 몇 가지 추가 옵션을 지정할 수 있습니다. 다음 예제에서는 /data/www에서 마운트할 NFS를 설정합니다. {{site.data.keyword.filestorage_short}} 인스턴스의 NFS 마운트 포인트는 {{site.data.keyword.filestorage_short}} 목록 페이지에서 또는 API -`SoftLayer_Network_Storage::getNetworkMountAddress()` 호출을 통해 확보할 수 있습니다. 

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

이제 마운트를 사용하고 올바르게 마운트되었는지 확인할 수 있습니다. 

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
