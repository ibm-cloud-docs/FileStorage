---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# cPanel を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成

この記事では、cPanel を使用してバックアップを{{site.data.keyword.filestorage_full}}に保管するように構成する手順を説明します。root を使用できるか、または sudo SSH で WebHost Manager (WHM) のフル・アクセス権限を使用できる必要があります。この例は、**CentOS 7** ホストに基づいています。

**注**: [ここ](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}で、バックアップ・ディレクトリーの構成方法についての cPanel の資料を参照できます。

1. SSH を使用してホストに接続します。

2. マウント・ポイント・ターゲットが存在することを確認します。<br />
   **注**: デフォルトでは、cPanel システムは、バックアップ・ファイルをローカルの `/backup` ディレクトリーに保存します。このドキュメントでは、`/backup` が既に存在していてバックアップが含まれていることを前提としています。そのため、`/backup2` を新しいマウント・ポイントとして使用します。
   
3. [Red Hat Enterprise Linux での{{site.data.keyword.filestorage_short}}へのアクセス](accessing-file-storage-linux.html)および [CentOS への NFS/{{site.data.keyword.filestorage_short}}のマウント](mounting-nsf-file-storage.html)の説明に従って{{site.data.keyword.filestorage_short}}を構成します。必ず、`/backup2` にマウントし、ブート時にマウントされるように `/etc/fstab` で構成してください。<br />
   **注**: デフォルトでは、NFS は、ルート権限で作成されたファイルを nobody ユーザーにダウングレードします。root クライアントが NFS 共有に対する root 権限を保持できるように、`/etc/exports` に `no_root_squash` を追加する必要があります。<br />
   **注**: [CoreOS に NFS/{{site.data.keyword.filestorage_short}}をマウントする](mounting-storage-coreos.html)手順についても説明しています。<br />

4. **オプション**: 既存のバックアップを新しいストレージにコピーします。例えば、次のように `rsync` を使用します。
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **注**: このコマンドは、データを可能な限り保持した状態で (ハードリンクは除く) 圧縮転送し、転送中のファイルに関する情報を表示し、最後に簡単なサマリーを表示します。
    
5.  WebHost Manager にログインし、**「ホーム」** > **「バックアップ (Backup)」** > **「バックアップ構成 (Backup Configuration)」**を選択してバックアップ構成に移動します。

6.  新しいマウント・ポイントにバックアップを保存するように構成を編集します。 
    - /backup/ ディレクトリーの代わりに新しい場所の絶対パスを入力して、デフォルトのバックアップ・ディレクトリーを変更します。 
    - **「バックアップ・ドライブのマウントを有効にする (Enable to mount a backup drive)」**を選択します。この設定にすると、バックアップ構成プロセスが、`/etc/fstab` ファイルにバックアップ・マウント (`/backup2`) があるか検査します。<br /> **注**: ステージング・ディレクトリーと同じ名前のマウントが存在する場合、バックアップ構成プロセスは、ドライブをマウントして情報をそのマウントにバックアップします。バックアップ・プロセスは、終了時にドライブをアンマウントします。 

7. **「バックアップ構成 (Backup Configuration)」**インターフェース下部の**「構成の保存」**をクリックして変更を適用します。

8. **オプション**: 具体的なユース・ケースやビジネス・ニーズに応じて、古いストレージをサーバーから削除し、アカウントからキャンセルします。
