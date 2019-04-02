---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-14"

keywords: File Storage, Endurance, Performance, IOPS, replication, billing, file storage, NFS,

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# {{site.data.keyword.filestorage_short}} について
{: #about}

{{site.data.keyword.filestorage_full}} は、永続的で高速、柔軟な、NFS ベースのネットワーク接続型の{{site.data.keyword.filestorage_short}}です。 この Network Attached Storage (NAS) 環境では、ファイル共有機能とパフォーマンスを完全に制御できます。 {{site.data.keyword.filestorage_short}}共有は、回復力のために経路指定された TCP/IP 接続を介して、最大 64 個の許可デバイスに接続できます。
{:shortdesc}

## 機能
{: #FileStorageFeatures}

{{site.data.keyword.filestorage_short}}は、他に例を見ない機能セットと、クラス最高レベルの耐久性と可用性を備えています。 {{site.data.keyword.filestorage_short}} は、業界標準とベスト・プラクティスを使用して構築されており、データの整合性を保護するように設計されています。 {{site.data.keyword.filestorage_short}} は、保守イベントや予期しない障害の発生時に可用性を維持し、一貫したパフォーマンス・ベースラインを提供します。

{{site.data.keyword.filestorage_short}}の以下のコア機能を利用してください。

- **一貫したパフォーマンス・ベースライン**
   - プロトコル・レベルの IOPS (1 秒あたりの入出力操作数) を個々のボリュームに割り振ることで実現しています。
- **{{site.data.keyword.filestorage_short}}**
   - ファイル・ベースの NFS 共有に使用可能。
- **高い耐久性と回復力**
   - オペレーティング・システム・レベルの RAID (Redundant Arrays of Independent Disks) アレイを作成して管理することなく、保守イベントや計画外の障害の間中、データの整合性を保護して可用性を維持します。
- **保存データの暗号化** [(一部のデータ・センターで使用可能)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - data-at-rest のためのプロバイダー管理の暗号化を追加コストなしで利用できます
- **オール・フラッシュ・ストレージ** [(一部のデータ・センターで使用可能)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - ボリュームの用すべてのフラッシュ・ストレージは、2 IOPS/GB 以上のレベルでプロビジョニングできます。
- **スナップショット** [(一部のデータ・センターで使用可能)](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - 特定時点のデータ・スナップショットを、中断せずにキャプチャーします。
- **レプリケーション**  [(一部のデータ・センターで使用可能)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - [一部のデータ・センター](/docs/infrastructure/FileStorage?topic=FileStorage-news)でストレージがプロビジョニングされている場合に使用可能。
   - パートナーの {{site.data.keyword.BluSoftlayer_full}} データ・センターにスナップショットを自動的にコピーします。
- **高可用性接続**
   - 冗長ネットワーキング接続を使用して可用性を最大限に高めます。
   - NFS ベースの{{site.data.keyword.filestorage_short}}の経路指定 TCP/IP 接続。
- **並行アクセス**
   - 複数のホスト (最大 64 台) が同時にファイル・ボリュームにアクセスできます。
- **クラスター・データベース**
   - クラスター・データベースなど、高度なユース・ケースをサポートします。

## 請求処理

ファイル・ボリュームの請求を時間単位にするか月単位にするかを選択できます。 LUN に対して選択した請求のタイプが、そのスナップショット・スペースとレプリカに適用されます。 例えば、時間単位の請求を使用して LUN をプロビジョンした場合、スナップショットやレプリカの料金は時間単位で請求されます。 月単位の請求を使用して LUN をプロビジョンした場合、スナップショットやレプリカの料金は月単位で請求されます。

**時間単位の請求**では、ファイル・ボリュームがアカウントに存在した時間数が、LUN が削除されたとき、または請求サイクルの終わりのどちらか早いほうのタイミングで計算されます。 時間単位の請求は、使用期間が数日または 1 カ月未満のストレージに適しています。 時間単位の請求は、[一部のデータ・センター](/docs/infrastructure/FileStorage?topic=FileStorage-news)でプロビジョンされたストレージにのみ使用できます。

**月単位の請求**では、作成日から請求サイクル終了までの日割り額が計算され、ただちに請求されます。 請求サイクルの終了前にボリュームが削除されていても、返金はされません。 月単位の請求は、長期間 (1 カ月以上) にわたって保管してアクセスする必要があるデータを使用する実動ワークロード用のストレージに適しています。


**パフォーマンス**
<table>
  <caption>表 1 は、月ごとおよび時間ごとの請求によるパフォーマンス・ストレージの価格を示しています。</caption>
  <tr>
   <th>月額料金</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>時間料金</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
</table>

**エンデュランス**
<table>
  <caption>表 2 は、月ごとおよび時間ごとの請求オプションによる各ティアのエンデュランス・ストレージの価格を示しています。</caption>
  <tr>
   <th>IOPS ティア</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>月額料金</th>
   <td>$0.06/GB</td>
   <td>$0.15/GB</td>
   <td>$0.20/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>時間料金</th>
   <td>$0.0001/GB</td>
   <td>$0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0009/GB</td>
  </tr>
</table>



## プロビジョニング

{{site.data.keyword.filestorage_short}}・ボリュームは 20 GB から 12 TB までプロビジョンできます。次の 2 つのオプションがあります。 <br/>
- 事前定義されたパフォーマンス・レベルとスナップショットやレプリケーションなどの他の機能を備えた**エンデュランス**・ティアをプロビジョニングする。
- IOPS (1 秒あたりの入出力操作数) の割り振りを指定して強力な**パフォーマンス**環境を構築する。


### エンデュランス・ティアでのプロビジョニング

エンデュランス {{site.data.keyword.filestorage_short}} には、さまざまなアプリケーション・ニーズをサポートする 4 つの IOPS パフォーマンス・ティアが用意されています。 <br />

- **0.25 IOPS/GB** は、入出力の負荷が小さいワークロード用に設計されています。 これらのワークロードには通常、どの時点においても非アクティブになっているデータの割合が高いという特徴があります。 例えば、メールボックスを保管する用途や、部門レベルでファイルを共有する用途などがあります。

- **2 IOPS/GB** は、最も汎用的な用途のために設計されています。 例えば、Web アプリケーションで使用する小規模なデータベースや、ハイパーバイザーの VM ディスク・イメージをホストする用途があります。

- **4 IOPS/GB** は、高負荷のワークロード用に設計されています。 これらのワークロードには通常、どの時点においてもアクティブになっているデータの割合が高いという特徴があります。 例えば、トランザクション・データベースなどのパフォーマンスが重要なデータベースなどの用途があります。

- **10 IOPS/GB** は、NoSQL データベースで生成されるワークロードや、分析のためのデータ処理など、最も要求の厳しいワークロード用に設計されています。 このティアは、[一部のデータ・センター](/docs/infrastructure/FileStorage?topic=FileStorage-news)のみで最大 4 TB のプロビジョニングされるストレージにおいて使用可能です。

12-TB エンデュランス・ボリュームでは、最大で 48,000 IOPS が使用可能です。

実際のワークロードに対して適切なエンデュランス・ティアを選択することが重要です。 最大のパフォーマンスを達成するために必要な適切なブロック・サイズ、イーサネット接続速度、ホストの数を使用することも同じように重要です。 これらの要素のいずれかが他の要素と合っていないと、結果のスループットに大きな影響を与える可能性があります。

### パフォーマンスでのプロビジョニング

パフォーマンスは {{site.data.keyword.filestorage_short}} のクラスの 1 つであり、パフォーマンス要件がわかっているけれどもエンデュランス・ティアにはうまく当てはまらないという入出力負荷の高いアプリケーションをサポートするために設計されています。 予測可能なパフォーマンスは、プロトコル・レベルの IOPS を個々のボリュームに割り振ることで実現されます。 20 GB から 12 TB に至る範囲のストレージ・サイズを使用して、さまざまな IOPS 率 (100 から 48,000 まで) をプロビジョンできます。

{{site.data.keyword.filestorage_short}}のパフォーマンスは、ネットワーク・ファイル・システム (NFS) 接続を介してアクセスされ、マウントされます。 {{site.data.keyword.filestorage_short}}は通常、複数のサーバーが同時にボリュームにアクセスする場合に使用されます。 表 1 のサイズと IOPS に従って一貫したパフォーマンス・ボリュームを注文できます。それらのボリュームは Linux オペレーティング・システムで使用できます。

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>表 3 は、パフォーマンス・ストレージのためのサイズと IOPS のさまざまな組み合わせを示しています。<br/><sup><img src="/images/numberone.png" alt="脚注" /></sup> 一部のデータ・センターでは、6,000 を超える IOPS 制限が使用可能です。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>サイズ (GB)</th>
            <th>最小 IOPS</th>
            <th>最大 IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2,000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6,000 または 10,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 または 20,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>2,000</td>
            <td>200</td>
            <td>6,000 または 40,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>3,000 から 7,000</td>
            <td>300</td>
            <td>6,000 または 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>8,000 から 9,000</td>
            <td>500</td>
            <td>6,000 または 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>10,000 から 12,000</td>
            <td>1,000</td>
            <td>6,000 または 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
</table>


パフォーマンス・ボリュームは、プロビジョニングされた IOPS レベルに一貫して近いレベルで動作するように設計されています。 この整合性により、特定レベルのパフォーマンスのアプリケーション環境のサイズ変更やスケーリングが容易になります。 さらに、価格対パフォーマンスの割合が理想的であるボリュームを構築して環境を最適化することができます。
