---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-11"

---
{:new_window: target="_blank"}
 {:tip: .tip}
 {:note: .note}
 {:important: .important}

# {{site.data.keyword.filestorage_short}}の新しいロケーションと機能

{{site.data.keyword.BluSoftlayer_full}} は、新しいバージョンの {{site.data.keyword.filestorage_full}} を導入しています。 一部のデータ・センターで提供されているこの新しいストレージは、保存データをディスク・レベルで暗号化する、IOPS レベルの高いフラッシュ・ストレージを基盤としています。 これらの一部のデータ・センターで注文されたストレージはすべて、自動的に新しいバージョンの{{site.data.keyword.filestorage_short}}を使用して作成されます。

新しいボリュームの NFS マウント・ポイントは変更されています。 詳しくは、『[拡張{{site.data.keyword.filestorage_short}}・ボリュームの新しいマウント・ポイント](#new-mount-point-for-enhanced-file-storage-volumes)』セクションを参照してください。
{:important}

新しい{{site.data.keyword.filestorage_short}}は、以下の地域およびデータ・センターで提供されています。今後さらに多くのデータ・センターで提供される予定です。

<table role="presentation">
  <tr>
    <td><strong>米国 2</strong></td>
    <td><strong>EU</strong></td>
    <td><strong>オーストラリア</strong></td>
    <td><strong>カナダ</strong></td>
    <td><strong>ラテンアメリカ</strong></td>
    <td><strong>アジア太平洋</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
        SYD05<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*表 1 は、使用可能なデータ・センターを示しています。 地域ごとに列を分けています。 一部の都市 (ダラス、サンノゼ、ワシントン DC、アムステルダム、フランクフルト、ロンドン、シドニーなど) には複数のデータ・センターがあります。*

新しいストレージの機能と性能を次に示します。

- [保存データのプロバイダー管理の暗号化](block-file-storage-encryption-rest.html)。 <br/> すべての{{site.data.keyword.filestorage_short}}・ボリュームは、追加料金なしで、自動的に暗号化されてプロビジョンされます。
- 10 IOPS/GB ティア・オプション。 <br/> 最も要求の厳しいワークロードをサポートするために、新しいティアが、エンデュランス・タイプの{{site.data.keyword.filestorage_short}}に追加されました。
- オール・フラッシュ・ストレージ。 <br/> 2 IOPS/GB 以上のエンデュランス・オプションまたはパフォーマンス・オプションを使用してプロビジョンされる{{site.data.keyword.filestorage_short}}は、オール・フラッシュ・ストレージを基盤としています。
- スナップショットとレプリケーションのサポート。
- 使用予定期間が 1 カ月未満のストレージのために、時間単位の請求オプションが追加されました。
- パフォーマンス・タイプを使用してプロビジョンされる{{site.data.keyword.filestorage_short}}の IOPS は最大 48,000 です。
- シーズンに応じて負荷が変化する場合にパフォーマンスを向上させるために、IOPS レートを調整可能です。 この機能について詳しくは、[こちら](adjustable-iops.html)を参照してください。
- [{{site.data.keyword.filestorage_short}}・ボリューム複製機能](how-to-create-duplicate-volume.html)を使用して、データのクローンを作成します。
- ストレージは、GB 単位で最大 12 TB までただちに拡張できます。拡張したボリュームに複製を作成したりデータを手動で移動したりする必要はありません。 この機能について詳しくは、[こちら](expandable_file_storage.html)を参照してください。

## 拡張{{site.data.keyword.filestorage_short}}・ボリュームの新しいマウント・ポイント

これらのデータ・センターでプロビジョンされる拡張{{site.data.keyword.filestorage_short}}・ボリュームはすべて、非暗号化ボリュームとは異なるマウント・ポイントになります。 両方のストレージ・ボリュームに正しいマウント・ポイントを使用するために、コンソールの**「ボリュームの詳細 (Volume Details)」**ページでマウント・ポイント情報を確認することができます。 API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して正しいマウント・ポイントを取得することもできます。

すべての新機能にアクセスできるようにするには、API で注文する際に `Storage-as-a-Service Package 759` を選択します。 API で {{site.data.keyword.filestorage_short}} を注文する方法について詳しくは、[order_file_volume ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window} を参照してください。
{:important}

追加のデータ・センターがアップグレードされていないか確認したり、新しいフィーチャーや機能が{{site.data.keyword.filestorage_short}}に追加されていないか確認したりするには、このページをもう一度参照してください。
{:tip}
