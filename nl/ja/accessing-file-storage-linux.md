---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:pre: .pre}

# Linux への {{site.data.keyword.filestorage_short}} のマウント

まず、{{site.data.keyword.filestorage_full}} ボリュームにアクセスするホストに、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}で許可を与えてください。

1. {{site.data.keyword.filestorage_short}}のリスト・ページで、新しい共有に関連付けられている**「アクション」**をクリックし、**「ホストの許可」**をクリックします。
2. リストからホストを選択し、**「送信」**をクリックします。 このアクションにより、共有へのアクセスがホストに許可されます。

## {{site.data.keyword.filestorage_short}}の共有のマウント

Linux ベースの {{site.data.keyword.BluSoftlayer_full}} コンピューティング・インスタンスをネットワーク・ファイル・システム (NFS) 共有に接続するには、以下の手順を使用します。 この例は、Red Hat Enterprise Linux 6 に基づいています。オペレーティング・システム (OS) ベンダーの資料に従って、この手順をその他の Linux ディストリビューション用に調整することができます。

>**注** - ファイル・ストレージ・インスタンスのマウント・ポイントは、{{site.data.keyword.filestorage_short}}のリスト・ページから取得できます。また、API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して取得することもできます。

1. 必要なパッケージ/ツールをインストールします。
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}
    
2. リモート共有をマウントします。
   ```
   # mount -t "nfs version" -o "options" <mount_point> /mnt
   ```
       
   例
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```
 
3. マウントが成功したことを確認します。
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```
    
4. マウント・ポイントに移動し、ファイルを読み取り/書き込みます。
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   >**注** - root を使用して作成したファイルの所有権は `nobody:nobody` になります。 所有権を正しく表示するには、`idmapd.conf` を正しいドメイン設定に更新する必要があります。 『**NFS 用の no_root_squash の実装方法**』セクションを参照してください。
    
5. 始動時にリモート共有をマウントします。 セットアップを完了するには、ファイル・システム・テーブル (`/etc/fstab`) を編集して、始動時に自動的にマウントされるエントリーのリストにリモート共有を追加します。

   ```
   (hostname):/(username) /mnt "nfs version" "options" 0 0
   ```
    
   例
    
   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```
    
6. 構成ファイルにエラーがないことを確認してください。

   ```
   # mount -fav
   ```
   {:pre}
    
   コマンドがエラーなしで完了したら、セットアップは完了です。

   >**注:** - NFS 4.1 を使用する場合は、mount コマンドに `sec=sys` を追加して、ファイル所有権の問題を回避してください。

 
## NFS 用の `no_root_squash` の実装 (オプション)

`no_root_squash` を構成すると、root クライアントが NFS 共有に対する root 権限を保持できます。 
- NFSv3 の場合は、クライアントが何もしなくても、`no_root_squash` は正常に機能します。
- NFSv4 の場合は、nfsv4 ドメインを `slnfsv4.com` に設定し、`rpcidmapd` を開始するか、または使用している OS で使用されている同様のサービスを開始する必要があります。

例

1. ホストから、`/etc/idmapd.conf` でドメイン設定を行います。

   ```
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The following should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```
    
2. `nfsidmap -c` を実行します。
3. `rpcidmapd` を開始します。
   ```
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
