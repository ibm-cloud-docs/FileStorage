---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# CentOS への NFS/{{site.data.keyword.filestorage_short}}のマウント

CentOS 7 へのエンデュランス/パフォーマンス {{site.data.keyword.filestorage_full}} のマウントのプロセスは [RHEL 6 への{{site.data.keyword.filestorage_short}}のマウント](https://console.stage1.bluemix.net/docs/infrastructure/FileStorage/accessing-file-storage-linux.html)のプロセスとほとんど同じですが、マウントが NFS なので、マウント・ファイルで *Options=* 行を使用して追加のオプションをいくつか指定できます。次の例では、/data/www にマウントするように NFS を設定します。 {{site.data.keyword.filestorage_short}}・インスタンスの NFS マウント・ポイントは、{{site.data.keyword.filestorage_short}}のリスト・ページから、または `SoftLayer_Network_Storage::getNetworkMountAddress()` の API 呼び出しで取得できます。

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

次は、マウントを有効にして、適切にマウントされたことを確認します。

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
