---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-08"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# コンソールによる {{site.data.keyword.filestorage_short}} の注文

容量および IOPS のニーズを満たすように、{{site.data.keyword.filestorage_short}} をプロビジョンして微調整を行うことができます。 パフォーマンスを指定する 2 つのオプションを使用して、ストレージを最大限に活用してください。

- パフォーマンスの要件が適切に定義されていないワークロードに適合する事前定義済みパフォーマンス・レベルを備えたエンデュランス IOP ティアから選択することができます。
- パフォーマンスを使用して IOPS の合計数を指定することにより、特定のパフォーマンス要件を満たすようにストレージを微調整することができます。

## 事前定義済み IOPS ティアによる {{site.data.keyword.filestorage_short}} の注文 (エンデュランス)

1. [IBM Cloud カタログ](https://{DomainName}/catalog/){:new_window}にログインし、**「ストレージ」**をクリックします。 その後、{{site.data.keyword.filestorage_short}}を選択します。 **「作成」**をクリックします。

   あるいは、[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} } にログインして、**「ストレージ」**>**「{{site.data.keyword.filestorage_short}}」**をクリックします。 右上の**「{{site.data.keyword.filestorage_short}} を注文」**をクリックします。
2. デプロイメント・**ロケーション** (データ・センター) を選択します。
   - 新規ストレージは必ず、所有するコンピュート・ホストと同じ場所に追加してください。
3. 請求処理。 拡張機能を備えたデータ・センター (アスタリスクのマークがあるもの) を選択した場合は、月単位の請求と時間単位の請求のどちらにするかを選択できます。
     1. **時間単位**の請求で、アカウントにファイル・ボリュームが存在した時間数は、LUN が削除されるとき、または請求サイクルが終わるときに計算されます。 いずれか早いほうのタイミングです。 時間単位の請求は、使用期間が数日または 1 カ月未満のストレージに適しています。 毎時請求を選択できるのは、これらの[一部のデータ・センター](new-ibm-block-and-file-storage-location-and-features.html)にプロビジョンされたストレージのみです。
     2. **月単位**の請求の場合、作成日から請求サイクル終了までの日割り額が計算され、ただちに請求されます。 請求サイクルの終了前にファイル・ボリュームが削除されていても、返金は行われません。 月単位の請求は、長期間 (1 カ月以上) にわたって保管してアクセスする必要があるデータを使用する実動ワークロードで使用するストレージに適しています。

     改善された機能で更新**されていない**データ・センターにプロビジョンされたストレージには、デフォルトで月次請求タイプが使用されます。
     {:note}
4. ストレージ・サイズを**「新しいストレージ・サイズ (New Storage Size)」**フィールドに入力します。
5. **「ストレージ IOPS オプション (Storage IOPS Options)」**セクションで**「エンデュランス (IOPS ティア) (Endurance (Tiered IOPS))」**を選択します。
6. アプリケーションが必要とする IOPS ティアを選択します。
    - **0.25 IOPS/GB** は、入出力の負荷が小さいワークロード用に設計されています。 これらのワークロードには通常、ある時間において非アクティブになっているデータの割合が高いという特徴があります。 例えば、メールボックスを保管する用途や、部門レベルでファイルを共有する用途などがあります。
    - **2 IOPS/GB** は、最も汎用的な用途のために設計されています。 例えば、Web アプリケーションで使用する小規模なデータベースや、ハイパーバイザーの仮想マシン・ディスク・イメージをホストする用途があります。
    - **4 IOPS/GB** は、高負荷のワークロード用に設計されています。 これらのワークロードには通常、ある時間においてアクティブになっているデータの割合が高いという特徴があります。 例えば、トランザクション・データベースなどのパフォーマンスが重要なデータベースなどの用途があります。
    - **10 IOPS/GB** は、NoSQL データベースで生成されるワークロードや、分析のためのデータ処理など、最も要求の厳しいワークロード用に設計されています。 このティアは、[一部のデータ・センター](new-ibm-block-and-file-storage-location-and-features.html)で最大 4 TB までプロビジョンされるストレージに使用可能です。
7. **「スナップショット・スペース・サイズの指定」**をクリックし、リストからスナップショット・サイズを選択します。 このスペースは、使用可能なスペースに加算されます。 スナップショット・スペースの考慮事項および推奨事項については、[スナップショットの注文](ordering-snapshots.html)を参照してください。
8. 右側で注文の要約を確認し、プロモーション・コードがあればそれを適用します。

   注文の処理時に割引が適用されます。
   {:note}
9. ご使用条件を確認した後、**「サード・パーティー・サービス契約を読み、同意します」**ボックスにチェック・マークを付けます。
10. **「作成」**をクリックします。 新しいストレージ割り振りが数分後に使用可能になります。

デフォルトでは、合計 250 の {{site.data.keyword.blockstorageshort}} ボリュームをプロビジョンできます。 ご使用のボリュームの数を増やすには、営業担当員にお問い合わせください。 制限の増加については、[こちら](managing-storage-limits.html)を参照してください。<br/><br/>同時許可の制限について詳しくは、[FAQ](faqs.html#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-) を参照してください。
{:tip}

## カスタム IOPS による {{site.data.keyword.filestorage_short}} の注文 (パフォーマンス)

1. [IBM Cloud カタログ](https://{DomainName}/catalog/){:new_window}にログインし、**「ストレージ」**をクリックします。 その後、{{site.data.keyword.filestorage_short}}を選択します。 **「作成」**をクリックします。

   あるいは、[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} } にログインして、**「ストレージ」**>**「{{site.data.keyword.filestorage_short}}」**をクリックします。 右上の**「{{site.data.keyword.filestorage_short}} を注文」**をクリックします。
2. **「ロケーション」**をクリックし、使用するデータ・センターを選択します。
   - 新規ストレージは必ず、所有するコンピュート・ホストと同じ場所に追加してください。
3. 請求処理。 拡張機能を備えたデータ・センター (アスタリスクのマークがあるもの) を選択した場合は、月単位の請求と時間単位の請求のどちらにするかを選択できます。
     1. **時間単位**の請求で、アカウントにファイル・ボリュームが存在した時間数は、LUN が削除されるとき、または請求サイクルが終わるときに計算されます。 いずれか早いほうのタイミングです。 時間単位の請求は、使用期間が数日または 1 カ月未満のストレージに適しています。 毎時請求を選択できるのは、これらの[一部のデータ・センター](new-ibm-block-and-file-storage-location-and-features.html)にプロビジョンされたストレージのみです。
     2. **月単位**の請求の場合、作成日から請求サイクル終了までの日割り額が計算され、ただちに請求されます。 請求サイクルの終了前にファイル・ボリュームが削除されていても、返金は行われません。 月単位の請求は、長期間 (1 カ月以上) にわたって保管してアクセスする必要があるデータを使用する実動ワークロードで使用するストレージに適しています。

     改善された機能で更新**されていない**データ・センターにプロビジョンされたストレージには、デフォルトで月次請求タイプが使用されます。
     {:note}
4. ストレージ・サイズを**「新しいストレージ・サイズ (New Storage Size)」**フィールドに入力します。
5. **「ストレージ IOPS オプション (Storage IOPS Options)」**セクションで**「パフォーマンス (IOPS 割り振り) (Performance (Allocated IOPS))」**を選択します。
6. **「パフォーマンス (IOPS 割り振り) (Performance (Allocated IOPS))」**フィールドに IOPS を入力します。
7. **「スナップショット・スペース・サイズの指定」**をクリックし、リストからスナップショット・サイズを選択します。 このスペースは、使用可能なスペースに加算されます。 スナップショット・スペースの考慮事項および推奨事項については、[スナップショットの注文](ordering-snapshots.html)を参照してください。
8. 右側で注文の要約を確認し、プロモーション・コードがあればそれを適用します。

   注文の処理時に割引が適用されます。
   {:note}
9. ご使用条件を確認した後、**「サード・パーティー・サービス契約を読み、同意します」**ボックスにチェック・マークを付けます。
10. **「作成」**をクリックします。 新しいストレージ割り振りが数分後に使用可能になります。

デフォルトでは、合計 250 の {{site.data.keyword.blockstorageshort}} ボリュームをプロビジョンできます。 ご使用のボリュームの数を増やすには、営業担当員にお問い合わせください。 制限の増加については、[こちら](managing-storage-limits.html)を参照してください。<br/><br/>同時許可の制限について詳しくは、[FAQ](faqs.html#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-) を参照してください。
{:important}


## 新しいストレージの接続

プロビジョニング要求が完了したら、ホストに対して、新しいストレージへのアクセスを許可し、接続を構成します。 ホストのホスト・オペレーティング・システムに応じて、次のうち該当するリンクを使用します。
- [Linux への{{site.data.keyword.filestorage_short}}のマウント](accessing-file-storage-linux.html)
- [CentOS への{{site.data.keyword.filestorage_short}}のマウント](mounting-nsf-file-storage.html)
- [CoreOS への{{site.data.keyword.filestorage_short}}のマウント](mounting-storage-coreos.html)
- [cPanel を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成](configure-backup-cpanel.html)
- [Plesk を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成](configure-backup-plesk.html)
- [ESXi ホストでの {{site.data.keyword.filestorage_short}} ボリュームのマウント](architecture-guide-file-storage-vmware.html)

## 災害復旧に関する考慮事項

データ損失を防いで事業継続性を保つには、サーバーとストレージを別のデータ・センターに複製することを考慮してください。 レプリケーションを行うと、スナップショットのスケジュールに基づいて、2 つの異なる場所にあるデータの同期を保つことができます。 詳しくは、[データのレプリケーション](replication.html)を参照してください。

使用しているボリュームを複製したものを元のボリュームとは独立して使用する場合は、[複製ブロック・ボリュームの作成](how-to-create-duplicate-volume.html)を参照してください。

## 請求書での {{site.data.keyword.filestorage_short}} ボリュームの識別

すべてのファイル共有が請求書の明細項目として表示されます。 エンデュランス・ボリュームは「Endurance Storage Service」と表示され、パフォーマンス・ボリュームは「Performance Storage Service」と表示されます。料金はストレージ・レベルによって異なります。 エンデュランスまたはパフォーマンスを展開すると、{{site.data.keyword.filestorage_short}}であることが分かります。
