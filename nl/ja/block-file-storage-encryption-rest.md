---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, security, encryption

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# プロバイダー管理の保存データ暗号化
{: #encryption}

{{site.data.keyword.BluSoftlayer_full}} はセキュリティーのニーズを真剣に受け止め、安全を維持するためにデータの暗号化機能が重要であることを理解しています。 プロバイダー管理の暗号化により、エンデュランスとパフォーマンスのどちらのオプションでも、プロビジョンした {{site.data.keyword.filestorage_full}} はデフォルトで暗号化されます。追加コストは不要でパフォーマンスへの影響もありません。

保存データのプロバイダー管理の暗号化機能は、以下の業界標準プロトコルを使用します。

* 業界標準の AES-256 暗号化
* 鍵は、業界標準の Key Management Interoperability Protocol (KMIP) を使用して組織内で管理されます。
* ストレージは、以下の規格に準拠していることが検証されています。
    - 連邦情報処理標準 (FIPS) PUB 140-2
    - 連邦情報セキュリティー・マネジメント法 (FISMA)
    - 医療保険の相互運用性と説明責任に関する法律 (HIPAA)
    - Payment Card Industry (PCI)
    - バーゼル II
    - California Security Breach Information Act (SB 1386)
    - EU データ保護指令 (95/46/EC)

## スナップショットまたは複製ストレージの保護  

暗号化ファイル・ストレージのスナップショットとレプリカもすべてデフォルトで暗号化されます。 この機能をボリューム単位でオフにすることはできません。

## 暗号化を使用するストレージのプロビジョニング

プロバイダー管理の保存データ暗号化機能は、一部のデータ・センターで使用できます。 それらのデータ・センターで注文されたすべてのストレージは、自動的に保存データ暗号化機能を使用してプロビジョンされます。 {{site.data.keyword.filestorage_short}}暗号化を使用できるデータ・センターの現在のリストを参照するには、[ここ](/docs/infrastructure/FileStorage?topic=FileStorage-news)をクリックしてください。

{{site.data.keyword.filestorage_short}}の注文時に、アスタリスク (`*`) が付いているデータ・センターを選択してください。 「LUN/ボリューム名」フィールドの右に、ボリュームが暗号化されていることを示す鍵のアイコンが表示されます。 図 1 を参照してください。

![LUN が暗号化されていることを示す鍵アイコン](/images/encryptedstorage.png)
<caption>図 1. ボリュームが暗号化されていることを示す鍵アイコンの例。</caption>

データ・センターのアップグレードの前にプロビジョンされていた非暗号化ストレージは、自動的には暗号化**されません**。 アップグレードされたデータ・センター内に非暗号化ストレージを所有していて、それを暗号化することが必要な場合は、新規ボリュームを作成してデータを移動する必要があります。 詳しくは、[アップグレードされたデータ・センターでのファイル・ストレージのマイグレーション](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)を参照してください
{:important}
