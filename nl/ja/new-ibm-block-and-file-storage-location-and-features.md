---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}}と{{site.data.keyword.filestorage_short}}の新しいロケーションと機能

{{site.data.keyword.BluSoftlayer_full}} は、新しいバージョンの{{site.data.keyword.blockstorageshort}}と {{site.data.keyword.filestorage_full}} を導入しています。  一部のデータ・センターで提供されているこの新しいストレージは、保存データをディスク・レベルで暗号化する、IOPS レベルの高いフラッシュ・ストレージを基盤としています。 これらの一部のデータ・センターでプロビジョンしたストレージはすべて、自動的に新しいバージョンの{{site.data.keyword.blockstorageshort}}と{{site.data.keyword.filestorage_short}}を使用してプロビジョンされます。

**注:** 新しいボリュームの NFS マウント・ポイントは変更されています。 詳しくは、下記の『**暗号化された {{site.data.keyword.filestorage_short}}・ボリューム用の新しいマウント・ポイント**』を参照してください。

新しい{{site.data.keyword.filestorage_short}}は、現在、以下の地域/データ・センターで提供されていますが、対応するデータ・センターが近々追加されます。
<table style="width:100%;">
	<caption>使用可能なデータ・センター</caption>
	<tbody>
		<tr>
			<td><strong>米国 2</strong></td>
			<td><strong>EU</strong></td>
			<td><strong>オーストラリア</strong></td>
			<td><strong>カナダ</strong></td>
			<td><strong>ラテンアメリカ</strong></td>
			<td><strong>アジア太平洋</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br /><br /></p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				FRA04<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
				SEO01<br />
				SNG01<br />
				                                CHE01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


新しいストレージの機能と性能を次に示します。

-  [保存データのプロバイダー管理の暗号化](block-file-storage-encryption-rest.html)。 すべての{{site.data.keyword.blockstorageshort}}と{{site.data.keyword.filestorage_short}}は、追加料金なしで、自動的に暗号化されてプロビジョンされます。
-  10 IOPS/GB ティア・オプション。 最も要求の厳しいワークロードをサポートするために、新しいティアが、エンデュランス・タイプの{{site.data.keyword.blockstorageshort}}と{{site.data.keyword.filestorage_short}}に追加されました。
-  オール・フラッシュ・ストレージ。  2 IOPS/GB 以上のエンデュランスまたはパフォーマンスを使用してプロビジョンされる{{site.data.keyword.blockstorageshort}}と{{site.data.keyword.filestorage_short}}は、オール・フラッシュ・ストレージを基盤としています。
-  エンデュランスまたはパフォーマンスを使用してプロビジョンされた{{site.data.keyword.blockstorageshort}}と{{site.data.keyword.filestorage_short}}に対するスナップショットおよびレプリケーションのサポート。
-  使用予定期間が 1 カ月未満のストレージのために、時間単位の請求オプションが追加されました。 
-  パフォーマンスを使用してプロビジョンされた{{site.data.keyword.blockstorageshort}}と{{site.data.keyword.filestorage_short}}の IOPS が最大 48,000 に。
-  シーズンに応じて負荷が変化する場合のパフォーマンスの向上のために、IOPS レートは調整可能です。 この機能について詳しくは、[こちら](adjustable-iops.html)を参照してください。
-  [{{site.data.keyword.filestorage_short}}・ボリューム複製機能](how-to-create-duplicate-volume.html)を使用して、データの新規クローンを作成します。
- ストレージは、運用を停止することなく、GB 単位で最大 12 TB まで拡張できます。複製を作成したり、大きくしたボリュームにデータを手動でマイグレーションしたりする必要はありません。 この機能について詳しくは、[こちら](expandable_file_storage.html)を参照してください。

## 暗号化された{{site.data.keyword.filestorage_short}}・ボリューム用の新しいマウント・ポイント

これらのデータ・センターでプロビジョンされたすべての暗号化{{site.data.keyword.filestorage_short}}・ボリュームは、非暗号化ボリュームとは異なるマウント・ポイントになります。  暗号化ファイル・ストレージ・ボリュームと非暗号化ファイル・ストレージ・ボリュームの両方に正しいマウント・ポイントを使用するには、UI の「ボリュームの詳細 (Volume Details)」ページでマウント・ポイント情報を参照するか、API 呼び出し (`SoftLayer_Network_Storage::getNetworkMountAddress()`) を使用して正しいマウント・ポイントを取得してください。

追加のデータ・センターがいつアップグレードされたのかを確認したり、{{site.data.keyword.blockstorageshort}}と{{site.data.keyword.filestorage_short}}に新たに追加された機能や性能がないかを確認したりするには、またここを参照してください。
