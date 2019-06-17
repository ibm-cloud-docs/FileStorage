---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, mounting file storage, Linux, CentOS, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# CentOS への {{site.data.keyword.filestorage_short}} のマウント
{: #mountingCentOS}

{{site.data.keyword.filestorage_full}} を CentOS 7 にマウントするには、まず [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}または SLCLI によってホストを許可する必要があります。その後、[Linux への {{site.data.keyword.filestorage_short}} のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)の説明のとおりに、NFS ユーティリティーをインストールします。

CentOS の場合、マウント・ファイルの `Options=` 行を使用していくつかの追加のオプションを指定できます。 次の例では、`/data/www` にマウントするように NFS を設定します。

{{site.data.keyword.filestorage_short}}・インスタンスの NFS マウント・ポイントは、{{site.data.keyword.filestorage_short}}のリスト・ページから取得できます。また、API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して取得することもできます。
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

次に、マウントを有効にして、適切にマウントされたことを確認します。

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
