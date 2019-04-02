---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-26"

keywords: File Storage, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# FAQ
{: #faqs}

## どの{{site.data.keyword.filestorage_short}} ボリュームが暗号化されているのかはどうすればわかりますか?
{: faq}

カスタマー・ポータルで{{site.data.keyword.filestorage_short}}のリストを参照してください。 ボリューム名の右に、ボリュームが暗号化されていることを示す鍵のアイコンが表示されます。

## 非暗号化{{site.data.keyword.filestorage_short}}を購入したデータセンターが、暗号化できるようにアップグレードされた場合、非暗号化{{site.data.keyword.filestorage_short}}を暗号化できますか?
{: faq}

データ・センターのアップグレードの前にプロビジョンされていた{{site.data.keyword.filestorage_short}}は、暗号化できません。 アップグレードされたデータ・センターで新たにプロビジョンされた{{site.data.keyword.filestorage_short}}は、自動的に暗号化されます。 自動で行われるので、プロビジョニング設定を選択したり除外したりすることはできません。 非暗号化ストレージのデータを暗号化するには、新しいボリュームを作成し、ホスト・ベースのマイグレーションを実行した新しい暗号化ボリュームに、データをコピーします。 詳しくは、[ファイル・ストレージのマイグレーション](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)を参照してください。

## {{site.data.keyword.filestorage_short}}をプロビジョンしているのが、アップグレードされたデータ・センターかどうかはどうすればわかりますか?
{: faq}

{{site.data.keyword.filestorage_short}}の注文フォームでは、アップグレードされたすべてのデータ・センターがアスタリスク (`*`) 付きで表示されます。 注文プロセスの中で、暗号化ストレージがプロビジョンされますというメッセージも表示されます。 ストレージがプロビジョンされると、ボリュームが暗号化されていることを示すアイコンがストレージ・リストに表示されます。

暗号化ボリュームおよびファイル共有はすべて、アップグレードされたデータ・センターでのみプロビジョンされます。 アップグレードされたデータ・センターと使用可能な機能の完全なリストについては、[こちら](/docs/infrastructure/FileStorage?topic=FileStorage-news)を参照してください。

## エンデュランスが 10 IOPS ティアの {{site.data.keyword.filestorage_short}} がプロビジョニングされるデータ・センターとそうでないセンターがあるのはなぜですか?
{: faq}

{{site.data.keyword.filestorage_short}} のエンデュランス・タイプ 10 IOPS/GB ティアは、一部のデータ・センターでのみ提供されており、近々、新しいデータ・センターが追加される予定です。 アップグレードされたデータ・センターと使用可能な機能の完全なリストについては、[こちら](/docs/infrastructure/FileStorage?topic=FileStorage-news)を参照してください。

## {{site.data.keyword.filestorage_short}}の正しいマウント・ポイントはどうすればわかりますか?
{: faq}

拡張されたデータ・センターでプロビジョンされるすべての暗号化{{site.data.keyword.filestorage_short}}・ボリュームは、非暗号化ボリュームとは異なるマウント・ポイントになります。 正しいマウント・ポイントを使用するには、UI の**「ボリュームの詳細 (Volume Details)」**ページでマウント・ポイント情報を参照してください。 API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して正しいマウント・ポイントを取得することもできます。

## ボリュームをいくつプロビジョンできますか?
{: faq}

デフォルトでは、合計 250 のブロック・ストレージ・ボリュームとファイル・ストレージ・ボリュームをプロビジョンできます。 制限を大きくするには、営業担当員にお問い合わせください。 詳しくは、[ストレージ制限の管理](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)を参照してください。

## プロビジョンされた{{site.data.keyword.filestorage_short}}・ボリュームの使用を共有できるインスタンス数はいくつですか?
{: faq}

ファイル・ボリュームあたりの許可数のデフォルトの制限は 64 です。 この制限を大きくするには、営業担当員にお問い合わせください。

## {{site.data.keyword.filestorage_short}}1 つのホストにボリュームをいくつ接続できますか?
{: faq}

{{site.data.keyword.BluSoftlayer_full}} 制限ではなく、ホスト・オペレーティング・システムが処理できる内容によって異なります。 マウントできるファイル共有数に関する制限については、ご使用の OS の資料を参照してください。

## ファイル・ボリューム・サイズに応じて許可されるファイル共有数はいくつですか? ボリューム・サイズに応じて許可される最大ファイル共有数はいくつですか?
{: faq}

<table>
  <caption>表 1 は、ボリューム・サイズに基づいて許可される i ノードの最大数を示しています。 左側の列はボリューム・サイズです。 右側は i ノードおよびファイル共有の数です。</caption>
  <thead>
    <tr>
      <th>ボリューム・サイズ</th>
      <th>i ノードとファイル共有の数</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB - 39 GB</td>
      <td>622,484</td>
    </tr>
    <tr>
      <td>40 GB - 79 GB</td>
      <td>1,245,084</td>
    </tr>          
    <tr>
      <td>80 GB - 99 GB</td>
      <td>2,490,263</td>
    </tr>          
    <tr>
      <td>100 GB - 249 GB</td>
      <td>3,112,863</td>
    </tr>          
    <tr>
      <td>250 GB - 499 GB</td>
      <td>7,782,300</td>
    </tr>          
    <tr>
      <td>500 GB - 999 GB</td>
      <td>15,564,695</td>
    </tr>
    <tr>
      <td>1 TB</td>
      <td>31,876,593</td>
    </tr>
    <tr>
      <td>2 TB</td>
      <td>63,753,186</td>
    </tr>
    <tr>
      <td>3 TB</td>
      <td>95,629,970</td>
    </tr>
    <tr>
      <td>4 TB - 12 TB</td>
      <td>127,506,359</td>
    </tr>
   </tbody>
</table>

## IOPS の測定
{: faq}

IOPS は、16-KB ブロックで読み取り 50 パーセント書き込み 50 パーセントがランダムに行われるという負荷プロファイルに基づいて測定されます。 このプロファイルと異なるワークロードでは、パフォーマンスが低下する場合があります。

## パフォーマンスの測定に小さいブロック・サイズを使用するとどうなりますか?
{: faq}

小さいブロック・サイズを使用する場合でも、最大の IOPS が得られます。 しかし、スループットはこの場合より低くなります。 例えば、6000 IOPS のボリュームの場合、ブロック・サイズが変わると、スループットは以下のようになります。

- 16 KB * 6000 IOPS == ~93.75 MB/秒
- 8 KB * 6000 IOPS == ~46.88 MB/秒
- 4 KB * 6000 IOPS == ~23.44 MB/秒


## 割り振られた IOPS はインスタンス単位で適用されるのですか、ボリューム単位で適用されるのですか?
{: faq}

IOPS はボリューム・レベルで適用されます。 言い換えると、1 つの 6000 IOPS ボリュームに接続された 2 つのホストの間で、その 6000 IOPS はシェアされます。

## 期待スループットを得るためには、ボリュームをプリウォームしておく必要がありますか?
{: faq}

　プリウォームの必要はありません。 ボリュームをプロビジョニングすると、ただちに指定したスループットになります。

## より高速なイーサネット接続を使用すると、より多くのスループットを実現できますか?
{: faq}

スループットの限度はボリューム単位のレベルで設定されます。 より高速なイーサネット接続を使用しても、この限度は増えません。 一方、低速イーサネット接続を使用すると、帯域幅がボトルネックになる可能性があります。

## ファイアウォールやセキュリティー・グループはパフォーマンスに影響を与えますか?
{: faq}

VLAN 上でストレージ・トラフィックを実行して、ファイアウォールをバイパスするのが最善です。 ソフトウェア・ファイアウォールを介してストレージ・トラフィックを実行すると、待ち時間が長くなり、ストレージ・パフォーマンスに悪影響を与えます。

## {{site.data.keyword.filestorage_short}} にどれくらいのパフォーマンス待ち時間を見込めばよいでしょうか?   
{: faq}

ストレージ内の目標待ち時間は 1 ms 未満です。 ストレージは共有ネットワーク上のコンピューティング・インスタンスに接続されるため、正確なパフォーマンス待ち時間は、操作時のネットワーク・トラフィックに依存します。

## {{site.data.keyword.filestorage_short}}・ボリュームが削除されるとデータはどうなりますか?
{: faq}

{{site.data.keyword.filestorage_full}} では、お客様に対して物理ストレージ上のファイル共有が提供されます。この物理ストレージは再使用前にワイプされます。 NIST 800-88 Guidelines for Media Sanitization などのコンプライアンスに関して特別な要件があるお客様の場合、ストレージを削除する前にデータ・サニタイズ手順を実行する必要があります。

## サポートされている NFS バージョン
{: faq}

{{site.data.keyword.BluSoftlayer_full}} 環境では、NFS v3 と NFS v4.1 の両方がサポートされています。 

推奨されているバージョンは NFS v3 です。NFS v3 はステートレス・プロトコルで、ネットワーク・イベント発生時の回復力がより高いためです。 

NFS v3 は、root クライアントが NFS 共有に対する root 権限を保持することを許可する `no_root_squash` をネイティブでサポートしています。NFS v4.1 では、この機能は、ドメイン情報を編集して `rpcidmapd` または類似したサービスを実行することによって有効にできます。詳しくは、[NFS 用の no_root_squash の実装](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux#norootsquash)を参照してください。

vSphere ソリューションに関しては、NFS v3 は v4.1 よりも多くの機能をサポートしています。それらの機能の中には、ストレージ DRS および Site Recovery Manager があります。


## クラウド・データ・センターから破棄したドライブはどうなりますか?
{: faq}

ドライブが破棄される場合、処分する前に IBM でそれらを破壊します。 ドライブは使用できなくなります。 そのドライブに書き込まれたデータにはアクセスできなくなります。
