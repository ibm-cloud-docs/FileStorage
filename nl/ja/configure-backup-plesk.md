---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Plesk を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成

この記事では、Plesk でのバックアップのために{{site.data.keyword.blockstoragefull}}を構成する手順を説明します。root を使用できるか、または sudo SSH で Plesk の管理者レベルのフル・アクセス権限を使用できる必要があります。この例は、CentOS7 ホストに基づいています。

**注**: [ここ](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}で、バックアップとリストアに関する Plesk の資料を参照できます。

1. SSH を使用してホストに接続します。

2. マウント・ポイント・ターゲットが存在することを確認します。<br />
   **注**: Plesk には、バックアップを保管するための 2 つのオプションがあります。1 つは内部 Plesk ストレージ (Plesk サーバー上にあるバックアップ・ストレージ) で、もう 1 つは外部 FTP ストレージ (Web またはローカル・ネットワークの外部サーバー上にあるバックアップ・ストレージ) です。Plesk のボックスでは一般的に、内部バックアップは `/var/lib/psa/dumps` に保管され、`/tmp` が一時ディレクトリーとして使用されます。この例では、一時ディレクトリーはローカルのままにしますが、ダンプ・ディレクトリーを STaaS ターゲット (`/backup/psa/dumps`) に移します。FTP ユーザー資格情報は必要ありません。
   
3. [Red Hat Enterprise Linux での{{site.data.keyword.filestorage_short}}へのアクセス](accessing-file-storage-linux.html)および [CentOS への NFS/{{site.data.keyword.filestorage_short}}のマウント](mounting-nsf-file-storage.html)の説明に従って{{site.data.keyword.filestorage_short}}を構成します。必ず、`/backup` にマウントし、ブート時にマウントされるように `/etc/fstab` で構成してください。<br />
   **注**: デフォルトでは、NFS は、ルート権限で作成されたファイルを nobody ユーザーにダウングレードします。root クライアントが NFS 共有に対する root 権限を保持できるように、`/etc/exports` に `no_root_squash` を追加する必要があります。<br />
   **注**: [CoreOS に NFS/{{site.data.keyword.filestorage_short}}をマウントする](mounting-storage-coreos.html)手順についても説明しています。<br />

4. **オプション**: 既存のバックアップを新しいストレージにコピーします。例えば、次のように `rsync` を使用します。
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **注**: このコマンドは、データを可能な限り保持した状態で (ハードリンクは除く) 圧縮転送し、転送中のファイルに関する情報を表示し、最後に簡単なサマリーを表示します。
    
5. `/etc/psa/psa.conf` を編集して、新しいターゲットを `DUMP_D` 値に指定します。 
    -  `DUMP_D /backup/psa/dumps` のような指定になります。 

6. **オプション**: 具体的なユース・ケースやビジネス・ニーズに応じて、古いストレージをサーバーから削除し、アカウントからキャンセルします。

