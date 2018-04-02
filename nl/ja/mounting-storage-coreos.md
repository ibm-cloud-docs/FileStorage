---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# CoreOS への{{site.data.keyword.filestorage_short}}のマウント

CoreOS は、さまざまなインフラストラクチャーで大規模かつスケーラブルなデプロイメントを簡単に管理できるように構築された強力な Linux ディストリビューションです。Chrome OS のビルドに基づいて、CoreOS は軽量のホスト・システムを維持し、すべてのアプリケーションに Docker コンテナーを使用します。

## ポータブル・ストレージのマウント

CoreOS では、システム・レベルのマウントは読み取り専用のディレクトリーに入っているため、2 次マウント・ファイルはすべて */etc/systemd/system* ディレクトリーに入れます。MOUNTPOINT.mount ファイルを作成します。この .mount ファイルの Where セクションはファイル名と一致しなければなりません。マウント・ポイントが / の直下にない場合は、path-to-mount.mount という構文を使用してファイルに名前を付ける必要があります。次の例でわかるように、ここではポータブル・ストレージ・ドライブを `/mnt/www` にマウントするので、`mnt-www.mount` という名前をファイルに付けます。

fdisk または parted を使用してパーティションを作成し、作成するファイル・システムが `.mount` ファイルにリストしたものと一致するようにしておく必要があります。そうしないと、サービスは開始に失敗します。


```
[Unit]
Description = Mount for Portable Storage

[Mount]
What=/dev/xvdc1
Where=/mnt/www
Type=ext4

[Install]
WantedBy = multi-user.target
```

CoreOS は systemd を使用するので、リブート後もマウント・ポイントを存続させるには、`*.mount` ファイルを有効にする必要があります。`--now` フラグを使用すると、ただちにパーティションがマウントされ、ブート時に開始するように設定されます。

`$ systemctl enable --now mnt-www.mount`

## NFS/{{site.data.keyword.filestorage_short}}のマウント

エンデュランス/パフォーマンスの{{site.data.keyword.filestorage_short}}のマウントのプロセスもほとんど同じですが、マウントが NFS なので、マウント・ファイルで Options= 行を使用して追加のオプションをいくつか指定できます。次の例では、`/data/www` にマウントするように NFS を設定します。{{site.data.keyword.filestorage_short}}・インスタンスの NFS マウント・ポイントは、{{site.data.keyword.filestorage_short}}のリスト・ページから、または SoftLayer_Network_Storage::getNetworkMountAddress() の API 呼び出しで取得できます。

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
 
## NAS/CIFS のマウント

CIFS 共有のマウントは CoreOS ではネイティブにサポートされていませんが、ホスト・システムに NAS 共有をマウントさせる非常に簡単な回避策があります。コンテナーを使用して mount.cfis モジュールをビルドし、それを CoreOS システムにコピーするのです。
 
CoreOS システムで、次のコマンドを実行して、Fedora コンテナーをダウンロードしてドロップインします。<br/>
`docker run -t -i -v /tmp:/host_tmp fedora /bin/bash`
 
コンテナーに入ったら、次のコマンドを実行して cifs ユーティリティーをビルドします。
```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
 
mount.cifs ファイルがホスト・マシンにコピーされたので、 `exit` コマンドを発行するか、**ctrl+d** を押して、docker コンテナーを終了できます。CoreOS システムに戻ったら、次のコマンドを使用して CIFS 共有をマウントできます。<br/>
`/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount`
 
## ISCSI のマウント

これは現在 CoreOS ではサポートされていませんが、今後リリースされる見込みです (https://github.com/coreos/bugs/issues/634)。
