---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, cPanel, backups

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# cPanel を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成
{: #cPanelBackups}

これらの手順を使用することにより、バックアップが cPanel によって {{site.data.keyword.filestorage_full}} に保存されるように構成することができます。 root を使用できるか、または sudo SSH で WebHost Manager (WHM) のフル・アクセス権限を使用できる必要があります。 この例は、**CentOS 7** ホストに基づいています。

詳しくは、[cPanel - Configuring backup directory](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){: external} を参照してください。
{:tip}

1. SSH を使用してホストに接続します。
2. マウント・ポイント・ターゲットが存在することを確認します。 <br />

   デフォルトで、cPanel システムはバックアップ・ファイルを `/backup` ディレクトリーにローカルに保存します。 このドキュメントでは、`/backup` フォルダーが存在していてバックアップが含まれていることを前提としています。`/backup2` を新しいマウント・ポイントとして使用することができます。
   {:note}

3. [Red Hat Enterprise Linux での {{site.data.keyword.filestorage_short}} へのアクセス](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)および [CentOS での {{site.data.keyword.filestorage_short}} のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)/[CoreOS での NFS/{{site.data.keyword.filestorage_short}} のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)の説明に従って {{site.data.keyword.filestorage_short}} を構成します。 ボリュームを `/backup2` にマウントし、始動時にマウントされるようにファイル・システム・テーブル (`/etc/fstab`) で構成してください。 <br />

   デフォルトでは、NFS は、root 権限で作成されたファイルを nobody ユーザーにダウングレードします。 root クライアントが NFS 共有に対する root 権限を保持できるように、`/etc/exports` に `no_root_squash` を追加する必要があります。
   {:tip}

4. **オプション**。 既存のバックアップを新しいストレージにコピーします。 例えば、`rsync` を使用できます。
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    このコマンドはデータを圧縮して送信し、ハード・リンクを除いてデータを可能な限り保持します。 さらに、転送中のファイルに関する情報を表示し、最後に簡単なサマリーを表示します。
    {:tip}

5. WebHost Manager にログインし、**「ホーム」**>**「バックアップ」**>**「バックアップ構成 (Backup Configuration)」**を選択してバックアップ構成に移動します。

6. 新しいマウント・ポイントにバックアップを保存するように構成を編集します。
    - `/backup/` ディレクトリーの代わりに新しい場所の絶対パスを入力して、デフォルトのバックアップ・ディレクトリーを変更します。
    - **「バックアップ・ドライブのマウントを有効にする (Enable to mount a backup drive)」**を選択します。 この設定にすると、構成プロセスが、`/etc/fstab` ファイルにバックアップ・マウント (`/backup2`) があるか検査します。 <br />

      ステージング・ディレクトリーと同じ名前のマウントが存在する場合、バックアップ構成プロセスは、ドライブをマウントして情報をそのマウントにバックアップします。 バックアップ・プロセスは、終了時にドライブをアンマウントします。
      {:note}
7. **「構成の保存」**をクリックして変更を適用します。
8. **オプション**。 具体的なユース・ケースやビジネス・ニーズに応じて、古いストレージをサーバーから削除し、アカウントからキャンセルします。
