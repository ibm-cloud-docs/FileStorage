---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Plesk を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成
{: #PleskBackup}

これらの手順を使用することにより、Plesk でのバックアップ用に{{site.data.keyword.filestorage_full}}を構成することができます。 root を使用できるか、または sudo SSH で Plesk の管理者レベルのフル・アクセス権限を使用できる必要があります。 この例は、CentOS7 ホストに基づいています。

詳しくは、[バックアップとリストアに関する Plesk の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window} を参照してください。
{:tip}

1. SSH を使用してホストに接続します。
2. マウント・ポイント・ターゲットが存在することを確認します。 <br />

   Plesk には、バックアップを保管するための 2 つのオプションがあります。 1 つは内部 Plesk ストレージであり、それは Plesk サーバー上にあるストレージです。 もう 1 つは外部 FTP ストレージであり、それは Web またはローカル・ネットワーク内のいずれかの外部サーバー上にあるストレージです。 Plesk のボックスでは一般的に、内部バックアップは `/var/lib/psa/dumps` に保管され、`/tmp` が一時ディレクトリーとして使用されます。 この例では、一時ディレクトリーはローカルに保持されますが、`dumps` ディレクトリーは {{site.data.keyword.filestorage_short}} ターゲット (`/backup/psa/dumps`) に移動します。 FTP ユーザー資格情報は必要ありません。
   {:note}
3. [Red Hat Enterprise Linux での {{site.data.keyword.filestorage_short}} へのアクセス](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)および [CentOS での NFS/{{site.data.keyword.filestorage_short}} のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)または [CoreOS での NFS/{{site.data.keyword.filestorage_short}} のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)の説明に従って {{site.data.keyword.filestorage_short}} を構成します。 ボリュームを `/backup` にマウントし、始動時にマウントが有効になるようにファイル・システム・テーブル (`/etc/fstab`) で構成してください。 <br />

   デフォルトでは、NFS は、root 権限で作成されたファイルを nobody ユーザーにダウングレードします。 root クライアントが NFS 共有に対する root 権限を保持できるように、`/etc/exports` に `no_root_squash` を追加する必要があります。
   {:tip}
4. **オプション**。 既存のバックアップを新しいストレージにコピーします。 例えば、次のように `rsync` を使用します。
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   このコマンドは、データを圧縮転送し、可能な限りデータを保持します (ハード・リンクは除く)。 さらに、転送中のファイルに関する情報を表示し、最後に簡単なサマリーを表示します。
   {:tip}
5. `/etc/psa/psa.conf` を編集して、新しいターゲットを `DUMP_D` 値に指定します。
    - `DUMP_D /backup/psa/dumps` のようになります。
6. **オプション**。 具体的なユース・ケースやビジネス・ニーズに応じて、古いストレージをサーバーから削除し、アカウントからキャンセルします。
