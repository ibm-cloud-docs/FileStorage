---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-28"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# 開始のチュートリアル
{: #gettingstarted}

{{site.data.keyword.filestorage_full}} は、永続的で高速、柔軟な、NFS ベースのネットワーク接続型の{{site.data.keyword.filestorage_short}}です。 この Network Attached Storage (NAS) 環境では、ファイル共有機能とパフォーマンスを完全に制御できます。 {{site.data.keyword.filestorage_short}}共有は、回復力のために経路指定された TCP/IP 接続を介して、最大 64 個の許可デバイスに接続できます。
{:shortdesc}

## 始めに
{: #prereqs}

{{site.data.keyword.filestorage_short}}・ボリュームは 20 GB から 12 TB までプロビジョンできます。次の 2 つのオプションがあります。 <br/>
- 事前定義されたパフォーマンス・レベルとスナップショットやレプリケーションなどの他の機能を備えた**エンデュランス**・ティアをプロビジョニングする。
- IOPS (1 秒あたりの入出力操作数) の割り振りを指定して強力な**パフォーマンス**環境を構築する。

{{site.data.keyword.filestorage_short}} のオファリングについて詳しくは、[{{site.data.keyword.filestorage_short}} について](/docs/infrastructure/FileStorage?topic=FileStorage-about)を参照してください。

### プロビジョニングの考慮事項

**ブロック・サイズ**

エンデュランスとパフォーマンスの IOPS はどちらも、16-KB のブロック・サイズと、50/50 の読み取り/書き込みの 50 % のランダム・ワークロードに基づいています。 16-KB のブロックは、ボリュームへの 1 回の書き込みに相当します。
{:important}

アプリケーションで使用されるブロック・サイズは、ストレージのパフォーマンスに直接影響を及ぼします。 アプリケーションが使用するブロック・サイズが 16 KB よりも小さい場合、スループット制限の前に IOPS 制限が適用されます。 アプリケーションが使用するブロック・サイズが 16 KB よりも小さい場合、スループット制限の前に IOPS 制限が適用されます。

<table>
  <caption>表 4 は、ブロック・サイズと IOPS がスループット与える影響の例を示しています。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>ブロック・サイズ (KB)</th>
            <th>IOPS</th>
            <th>スループット (MB/秒)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (Linux の場合の標準)</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (Oracle の場合の標準)</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (SQL Server の場合の標準)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**許可ホスト**

考慮すべき別の要素は、ボリュームを使用しているホストの数です。 ボリュームにアクセスするホストが 1 つだけの場合、特に IOPS カウントが極端に大きい (10,000 を超える) 場合は、最大 IOPS を実現することが難しい可能性があります。 ワークロードで高スループットが必要な場合は、シングル・サーバーのボトルネックを回避するために、少なくとも 2 台のサーバーがボリュームにアクセスできるように構成することをお勧めします。

**ネットワーク接続**

イーサネット接続の速度は、ボリュームに期待する最大スループットよりも速くなければなりません。 一般に、イーサネット接続が使用可能な帯域幅の 70% を超えることは期待ないでください。 例えば、6,000 IOPS で 16-KB のブロック・サイズを使用している場合、ボリュームは約 94-MB/秒のスループットを処理できます。 1-Gbps イーサネットが LUN に接続されている場合、使用可能な最大スループットをサーバーが使用しようとすると、その接続がボトルネックになります。 1-Gbps イーサネット接続 (125 MB/秒) の理論上の限界を 70% とすると、88 MB/秒のスループットしか得られないことになるためです。

最大 IOPS を得るには、十分なネットワーク・リソースを配備する必要があります。 その他の考慮事項として、ストレージ外の専用ネットワーク使用、およびホスト・サイドおよびアプリケーション固有のチューニング (IP スタック、[キュー項目数](/docs/infrastructure/FileStorage?topic=FileStorage-hostqueuesettings)、およびその他の設定) があります。

パブリック Virtual Server のネットワーク使用量合計にストレージ・トラフィックが含まれます。 サービスによって課せられる可能性がある制限について詳しくは、[Virtual Server の資料](/docs/vsi?topic=virtual-servers-about-public-virtual-servers)を参照してください。

**NFS バージョン**

{{site.data.keyword.BluSoftlayer_full}} 環境では NFS v3 と NFS v4.1 の両方がサポートされます。 しかし、NFS v4.1 はステートフル・プロトコルであり (NFSv3 のようなステートレスではなく)、ネットワーク・イベント中にプロトコルの問題が発生する可能性があるため、望ましいのは NFS v3 です。 NFS v4.1 では、すべての操作を停止してからロック再利用を実行する必要があります。 比較的ビジーな NFS ファイル・サーバーでは、待ち時間が長くなって中断が生じる可能性があります。 また、NFS v4.1 のマルチパスおよびトランキングがないため、NFS 操作のリカバリー範囲が広くなる可能性があります。

## 注文の送信

注文を送信する準備ができたら、[コンソール](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)または [SLCLI](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI) を使用して注文することができます。 VMware を使用したプロビジョニング・ファイル・ストレージの場合は、[ここ](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)をクリックします

## 新しいストレージの接続
{: #mountingstorage}

プロビジョニング要求が完了したら、ホストに対して、新しいストレージへのアクセスを許可し、接続を構成します。 ホストのホスト・オペレーティング・システムに応じて、次のうち該当するリンクを使用します。
- [Linux での {{site.data.keyword.filestorage_short}} へのアクセス](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [CentOS への{{site.data.keyword.filestorage_short}}のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [CoreOS への{{site.data.keyword.filestorage_short}}のマウント](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [cPanel を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Plesk を使用してバックアップするための{{site.data.keyword.filestorage_short}}の構成](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [ESXi ホストでの {{site.data.keyword.filestorage_short}} ボリュームのマウント](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## 新しいストレージの管理

ポータルまたは SLCLI により、ホストの許可や取り消しなど、{{site.data.keyword.filestorage_short}} のさまざまな側面を管理できます。詳しくは、[{{site.data.keyword.filestorage_short}} の管理](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)を参照してください。
