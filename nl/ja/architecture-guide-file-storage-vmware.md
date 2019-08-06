---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-24"

keywords: File Storage, provisioning File Storage for VMware, NFS, File Storage, vmware,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# VMware による {{site.data.keyword.filestorage_short}} のプロビジョニング
{: #architectureguide}

{{site.data.keyword.cloud}} の vSphere 5.5 と vSphere 6.0 の環境で {{site.data.keyword.filestorage_full}} を注文して構成するための手順を以下に示します。

{{site.data.keyword.filestorage_short}} は、予測可能なレベルのパフォーマンスが求められる、入出力負荷の高いアプリケーションをサポートできるように設計されています。 プロトコル・レベルの IOPS (1 秒あたりの入出力操作数) を個々のボリュームに割り振ることで、予測可能なパフォーマンスを実現しています。

VMware データ・ストアへのアクセスに必要なホストの数が 8 を超える場合は、NFS {{site.data.keyword.filestorage_short}} を選択することがベスト・プラクティスです。
{:tip}

{{site.data.keyword.filestorage_short}} のオファリングの利用とマウントは、NFS 接続を使用して行います。 VMware デプロイメントでは、単一ボリュームを最大 64 台の ESXi ホストに共有ストレージとしてマウントすることができます。 複数のボリュームをマウントしてストレージ・クラスターを作成し、vSphere Storage Distributed Resource Scheduler (DRS) を使用することもできます。

{{site.data.keyword.filestorage_short}} の価格オプションと構成オプションは、予約したスペースと提供される IOPS の組み合わせに基づいて課金されます。

## 注文に関する考慮事項

{{site.data.keyword.filestorage_short}} を注文するときは、次の情報を考慮してください。

- サイズを決定する際には、必要なワークロードのサイズとスループットを考慮してください。 容量 (IOPS/GB) に応じて性能が直線的に高くなるエンデュランス・サービスにとっては、サイズが重要になります。 一方、パフォーマンス・サービスでは、管理者が容量と性能を個別に選択できます。 性能はスループット要件によって決まります。

  スループットは IOPS x 16 KB という式で計算します。 IOPS は、16-KB ブロック・サイズの読み取り/書き込みが 50/50 の割合で混在するものとして測定されます。<br/>ブロック・サイズを増やすとスループットが向上しますが、IOPS は減少します。 例えば、ブロック・サイズを 2 倍にして 32-KB にすると、最大スループットは維持されますが、IOPS は半減します。
  {:note}

- NFS は、例えば `lookup`、`getattr`、`readdir` など、他の多くのファイル制御操作を使用します。 読み取り/書き込み操作に加えて、これらの操作も IOPS と見なされ、操作のタイプや NFS のバージョンに応じて異なります。
- {{site.data.keyword.filestorage_short}}のボリュームは、許可されたデバイス、サブネット、または IP アドレスに公開されます。
- パス・フェイルオーバー中のストレージの切断を避けるために、{{site.data.keyword.IBM}} は、適切なタイムアウト値を設定する VMware ツールをインストールすることをお勧めします。 値を変更する必要はありません。VMware ホストが接続を失わないようにするには、デフォルト設定で十分です。
- {{site.data.keyword.cloud}} 環境では NFSv3 と NFSv4.1 の両方がサポートされます。 しかし、{{site.data.keyword.IBM}} は NFSv3 を使用することを提案します。 NFSv4.1 は、ステートフル・プロトコルである (NFSv3 のようなステートレス・プロトコルではない) ため、ネットワーク・イベント中にプロトコルの問題が発生する可能性があります。 NFSv4.1 では、すべての操作を停止してからロック再利用を実行する必要があります。 それらの操作の実行中に、切断される可能性があります。

詳しくは、VMware の次のホワイト・ペーパーを参照してください: [Best Practices for running VMware vSphere on Network Attached Storage](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){: external}
{:tip}

### NFS プロトコルと VMware 機能の対応表

| vSphere の機能 | NFS バージョン 3 | NFS バージョン 4.1 |
|-----|-----|-----|
| vMotion および Storage vMotion | ○ | ○ |
| 高可用性 (HA) | ○ | ○ |
| フォールト・トレランス (FT) | ○ | ○ |
| 分散リソース・スケジューラー (DRS) | ○< | ○ |
| ホスト・プロファイル | ○ | ○ |
| ストレージ DRS | ○ | × |
| ストレージ入出力制御 | ○ | × |
| サイト・リカバリー・マネージャー | ○ | × |
| 仮想ボリューム | ○ | × |
{: row-headers}
{: class="comparison-table"}
{: caption="表 1 - NFS プロトコルと VMware 機能の対応表。" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify the vSphere features. The column headers identify the NSF version. To see if a feature is enabled navigate to the row of the feature and look at the column that is associated with the NFS version you use."}

*出典 - [VMware - NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){: external}*


### スナップショットの使用

{{site.data.keyword.filestorage_short}} を使用すると、管理者は、各ストレージ・ボリュームのスナップショット・コピーを自動的に作成および削除するスナップショット・スケジュールを設定できます。 また、自動スナップショットのスナップショット・スケジュール (毎時、日次、週次) を追加で作成したり、事業継続と災害復旧 (BCDR) のシナリオのスナップショットを随時手動で作成したりすることもできます。 ボリューム所有者に、[{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}を介して、保存されたスナップショットと使用されたスペースについての自動アラートが送信されます。

スナップショットを使用するには、スナップショット・スペースが必要です。 スペースはボリュームを最初に注文するときに購入できます。また、最初のプロビジョニングの後で、**「ボリュームの詳細 (Volume Details)」**ページの**「アクション」**をクリックし、**「スナップショット・スペースの追加」**を選択して取得することもできます。

VMware 環境はスナップショットを認識しないことに注意してください。 {{site.data.keyword.filestorage_short}}のスナップショット機能を VMware のスナップショットと混同しないでください。 {{site.data.keyword.filestorage_short}} のスナップショット機能を使用するリカバリーは、[{{site.data.keyword.cloud}} コンソール](https://{DomainName}/){: external}から操作する必要があります。

{{site.data.keyword.filestorage_short}} のボリュームをリストアするには、{{site.data.keyword.filestorage_short}} 上のすべての VM の電源をオフにする必要があります。 ESXi ホストから一時的にボリュームをアンマウントして、復元処理中のデータ破損を回避する必要があります。

詳しくは、[スナップショット](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)の記事を参照してください。


### レプリケーションの使用

レプリケーションでは、スナップショット・スケジュールの 1 つを使用して、スナップショットを自動的にリモート・データ・センター内の宛先ボリュームにコピーします。 災害やデータ破損が発生した場合に、リモート・サイトのコピーを復旧できます。

レプリカにより、以下が可能になります。

- 宛先ボリュームにフェイルオーバーして、サイト障害やその他の災害から素早く復旧する
- DR コピー内の特定の時点にフェイルオーバーする

レプリケーションを行うと、2 つの異なる場所にあるデータの同期を保つことができます。 使用しているボリュームを複製したものを元のボリュームとは独立して使用する場合は、[複製ファイル・ボリュームの作成](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)を参照してください。
{:tip}

レプリケーションを行うには、その前にスナップショット・スケジュールを作成する必要があります。

フェイルオーバー時には、プライマリー・データ・センターのストレージ・ボリュームからリモート・データ・センターの宛先ボリュームにスイッチを「切り替え」ます。 例えば、プライマリー・データ・センターがロンドンにあり、セカンダリー・データ・センターがアムステルダムにあるとします。 障害が発生したら、アムステルダムにフェイルオーバーします。 フェイルオーバーするとは、アムステルダムの vSphere Cluster インスタンスにある新プライマリー・ボリュームに接続するということです。 ロンドンのボリュームが修復されたら、アムステルダムのボリュームのスナップショットが作成されます。 その後、ロンドンにフォールバックしてロンドンのコンピューティング・インスタンスのボリュームをプライマリーに戻します。

ボリュームをプライマリー・データ・センターにフェイルバックする前に、リモート・サイトでの使用を停止する必要があります。 新規または変更された情報のスナップショットが取得され、プライマリー・データ・センターに複製された後に、実動サイトの ESXi ホストに再度マウントできるようになります。

レプリカの構成について詳しくは、[レプリケーション](/docs/infrastructure/FileStorage?topic=FileStorage-replication)を参照してください。

破損したり、ハッキングされたり、感染したりしていても、スナップショット・スケジュールとスナップショット保持設定に従って無効なデータが複製されます。 最短のレプリケーション期間にすると、より適切なリカバリー・ポイントに復旧できます。 また、これにより、無効なデータのレプリケーションにも少ない時間で対応できます。
{:note}


## {{site.data.keyword.filestorage_short}}の注文

[拡張単一サイト VMware リファレンス・アーキテクチャー](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){: external}を使用して、VMware 環境でエンデュランスまたはパフォーマンスのオプションを指定して {{site.data.keyword.filestorage_short}} をセットアップします。

{{site.data.keyword.filestorage_short}} は、[{{site.data.keyword.cloud}} カタログ](https://{DomainName}/catalog){: external}または CLI から注文できます。 詳しくは、[{{site.data.keyword.filestorage_short}} の注文](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)を参照してください。

ストレージが 1 分もしないうちにプロビジョンされ、[{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}の**「{{site.data.keyword.filestorage_short}}」**ページに表示されます。

ボリュームがプロビジョンされたら、そのボリュームを使用する {{site.data.keyword.BluBareMetServers_full}} または {{site.data.keyword.BluVirtServers_full}} に、ストレージへのアクセスを許可する必要があります。 ボリュームを許可するには、以下の手順を実行します。

1. コンソールで、**「クラシック・インフラストラクチャー」**>**「ストレージ」**>**「{{site.data.keyword.filestorage_short}}」**に移動します。
2. マウントするファイル共有にスクロールして、**「...」**(アクション) をクリックします。その後、**「ホストの許可」**を選択します。
3. **「サブネット」**をクリックします。
4. ESXi ホスト上の VMkernel ポートに割り当てられている使用可能なサブネットのリストからサブネットを選択し、**「保存」**をクリックします。<br/>

   表示されるサブネットは、ストレージ・ボリュームと同じデータ・センター内のサブスクライブ・サブネットです。
   {:note}

サブネットが許可されたら、ストレージ・サーバーのホスト名をメモします。ホスト名は、ボリュームの {{site.data.keyword.filestorage_short}} 詳細ページに示されています。


##  VMware 仮想マシン・ホストの構成

VMware 構成プロセスを開始する前に、以下の前提条件が満たされていることを確認してください。

- VMware ESXi を搭載した {{site.data.keyword.BluBareMetServers}} が、適切なストレージ構成と ESXi ログイン資格情報を使用してプロビジョンされている。
- {{site.data.keyword.cloud}} Windows 物理サーバーまたは{{site.data.keyword.virtualmachinesshort}}が、{{site.data.keyword.BluBareMetServers}} と同じデータ・センター内にある。 {{site.data.keyword.cloud}} Windows VM のパブリック IP アドレスおよびログイン資格情報を含む。
- インターネットにアクセスできるコンピューターがあり、Web ブラウザー・ソフトウェアとリモート・デスクトップ・プロトコル (RDP) クライアントがインストールされている。


### 1. VMware ホストの構成。

1. インターネットに接続されたコンピューターから RDP クライアントを開始し、vSphere vCenter がインストールされているデータ・センターにプロビジョンされた {{site.data.keyword.BluVirtServers_full}} への RDP セッションを確立します。
2. {{site.data.keyword.BluVirtServers_short}} で Web ブラウザーを開始し、vSphere Web Client を経由して VMware vCenter に接続します。
3. **「ホーム」**画面で、**「ホストおよびクラスター (Hosts and Clusters)」**を選択します。 左側のペインを展開し、このデプロイメントに使用する **VMware ESXi サーバー**を選択します。
4. vSphere ホストに NFS クライアントを構成できるようにするため、すべてのホストで NFS クライアント用のファイアウォール・ポートが開かれていることを確認します。 (このポートは、最新リリースの vSphere では自動的に開きます。) ポートが開いているかどうかを確認するには、VMware® vCenter™ の**「ESXi ホストの管理 (ESXi host Manage)」**タブに移動し、**「設定」**を選択し、**「セキュリティー・プロファイル (Security Profile)」**を選択します。 **「ファイアウォール (Firewall)」**セクションで、**「編集」**をクリックし、**「NFS クライアント (NFS Client)」**までスクロールダウンします。
5. **「すべての IP アドレスからの接続を許可する (Allow connection from any IP address)」**が指定されているか、IP アドレスのリストが指定されていることを確認します。 <br/>
   ![接続を許可する](/images/1_4.png)
6. **「ESXi ホストの管理 (ESXi host Manage)」**タブに移動し、**「管理」**を選択してから**「ネットワーキング」**を選択して、ジャンボ・フレームを構成します。
7. **「VMkernel アダプター (VMkernel adapters)」**を選択し、**「vSwitch」**を強調表示し、**「編集」**(鉛筆アイコン) をクリックします。
8. **「NIC 設定 (NIC setting)」**を選択し、NIC MTU が 9000 に設定されていることを確認します。
9. **オプション**。 ジャンボ・フレームの設定を検証します。
   - Windows
     ```
     ping -f -l 8972 a.b.c.d
     ```
     {:pre}

   - UNIX
     ```
     ping -s 8972 a.b.c.d
     ```
     {:pre}

     値 a.b.c.d は、隣接する {{site.data.keyword.BluVirtServers_short}} インターフェースです。

     例
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

VMware およびジャンボ・フレームについて詳しくは、[ここ](https://kb.vmware.com/s/article/1003712){: external}を参照してください。
{:tip}


### 2. 仮想スイッチへのアップリンク・アダプターの追加

1. **「ESXi ホストの管理 (ESXi host Manage)」**タブに移動し、**「管理」**を選択してから**「ネットワーキング」**を選択して、新規アップリンク・アダプターを構成します。
2. **「物理アダプター (Physical adapters)」**タブを選択します。
3. **「ホスト・ネットワーキングの追加 (Add host networking)」**(正符号付きの地球儀アイコン) をクリックします。
4. 接続タイプとして**「物理ネットワーク・アダプター (Physical Network Adapter)」**を選択し、**「次へ」**をクリックします。
5. 既存の **vSwitch** を選択し、**「次へ」**をクリックします。
6. **「未使用のアダプター (Unused adapters)」**を選択し、**「アダプターの追加 (Add adapters)」**(正符号) をクリックします。
7. もう一方の「接続済み (Connected)」アダプターをクリックし、**「OK」**をクリックします。 <br/>
   ![物理アダプターをスイッチに追加する](/images/2_3.png)
8. **「次へ」**をクリックして、**「完了」**をクリックします。
9. **「仮想スイッチ (Virtual switches)」**タブに戻り、**「仮想スイッチ (Virtual Switches)」**見出しの下にある**「設定の編集 (Edit setting)」**(鉛筆のアイコン) を選択します。
10. 左側で、vSwitch の**「チーミングとフェイルオーバー (Teaming and failover)」**項目を選択します。
11. **「ロード・バランシング (Load balancing)」**オプションが**「発信元の仮想ポートに基づいたルート (Route based on the originating virtual port)」**に設定されていることを確認し、**「OK」**をクリックします。


### 3. 静的ルーティングの構成 (オプション)

このアーキテクチャー・ガイドのネットワーク構成では、最小限の数のポート・グループを使用します。 NFS ストレージ用の VMkernel ポート・グループがある場合は、追加のステップを実行する必要があります。 デフォルトでは、ESXi は NFS ボリュームと同じサブネット上にある VMkernel ポートを使用して、NFS ボリュームをマウントします。 NFS ボリュームをマウントするためにレイヤー 3 ルーティングを使用しているため、NFS ボリュームをマウントするように構成した VMkernel ポートを ESXi で使用するように強制する必要があります。 正しいポートを使用するには、ストレージ・アレイへの静的ルートを作成する必要があります。

1. 静的ルートを構成するには、パフォーマンスまたはエンデュランス・ストレージを使用する各 ESXi ホストに SSH で接続し、以下のコマンドを実行します。 `ping` コマンド (最初のコマンド) の結果として得られる IP アドレスをメモしておき、`esxcli` ネットワーク・コマンドでそれを使用します。
   ```
   ping <host name of the storage array>
   ```
   {: pre}

   NFS ストレージの DNS ホスト名は、複数の IP アドレスが割り当てられる転送ゾーン (FZ) です。 これらの IP アドレスは静的であり、その特定の DNS ホスト名に属します。 これらの IP アドレスはすべて、特定のボリュームにアクセスするために使用できます。
   {:note}

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. ESXi 5.0 以前では、静的ルートは再始動を行うと保持されません。 追加した静的ルートを保持するには、このコマンドを各ホストの `/etc/rc.local.d/` ディレクトリーにある `local.sh` ファイルに追加する必要があります。 ビジュアル・エディターを使用して `local.sh` ファイルを開き、ステップ 4.1 の第 2 のコマンドを、`exit 0` の行の前に追加します。

次の手順でボリュームをマウントする際に使用できるように、IP アドレスをメモします。<br/>このプロセスは、ESXi ホストにマウントする NFS ボリュームごとに実行する必要があります。<br/>詳しくは、VMware KB の記事、[Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/s/article/2001426){: external} を参照してください。
{:tip}


##  ESXi ホストでの {{site.data.keyword.filestorage_short}} ボリュームの作成とマウント

1. **「vCenter に移動 (Go to vCenter)」**アイコンをクリックし、次に**「ホストおよびクラスター (Hosts and Clusters)」**をクリックします。
2. **「関連オブジェクト (Related Object)」**タブで、**「データ・ストア (Datastores)」**をクリックします。
3. **「新規データ・ストアの作成 (Create a new datastore)」**アイコンをクリックします。
4. **「新規データ・ストア (New Datastore)」**画面で、VMware データ・ストアの場所を選択し、**「次へ」**をクリックします。
5. **「タイプ」**画面で**「NFS」**を選択し、**「次へ」**をクリックします。
6. その後、NFS バージョンを選択します。 NFSv3 と NFSv4.1 の両方がサポートされていますが、NFSv3 をお勧めします。

   データ・ストアにアクセスするときには、1 つの NFS バージョンだけを使用するようにしてください。 複数の異なるバージョンを使用して 1 つ以上のホストを同じデータ・ストアにマウントすると、データ破損が発生する可能性があります。
   {:important}

7. **「名前と構成 (Name and configuration)」**画面で、VMware データ・ストアに付ける名前を入力します。 さらに、NFS サーバーのホスト名を入力します。 NFS サーバーの FQDN を使用すると、その基礎サーバーへの最適なトラフィック配信が行われます。 IP アドレスも有効ですが、特定の状況を除き、あまり使用されません。 フォルダー名を `/foldername` の形式で入力します。
8. **「ホストのアクセス可能性 (Host accessibility)」**画面で、NFS VMware データ・ストアをマウントするホストを 1 つ以上選択し、**「次へ」**をクリックします。
9. 次の画面で入力を確認し、**「完了」**をクリックします。
10. 他の {{site.data.keyword.filestorage_short}} ボリュームについても同じ手順を繰り返します。

{{site.data.keyword.cloud}} は VMware データ・ストアへの接続に FQDN 名を使用することをお勧めします。 直接 IP アドレッシングを使用すると、FQDN を使用することにより提供されるロード・バランシング・メカニズムが迂回される可能性があります。
{:important}

FQDN の代わりに IP アドレスを使用するには、単に、サーバーに対して ping を実行して IP アドレスを取得します。
```
ping <host name of the storage array>
```
{: pre}

ESXi ホストから IP アドレスを取得するには、次のコマンドを使用します。 結果の 10.2.125.80 が FQDN に代わる IP です。
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## ESXi ストレージ入出力制御を有効にする (オプション)

ストレージ入出力制御 (SIOC) は、Enterprise Plus ライセンスを利用するお客様が使用できる機能です。 SIOC を有効にした環境では、単一 VM のデバイス・キューの長さが変更されます。 デバイス・キューの長さの変更により、すべての VM のストレージ・アレイ・キューが縮小されて、ストレージ・キューが均等に割り当てられます。 SIOC が作動するのは、リソースが制約され、定義したしきい値をストレージ入出力の待ち時間が超えた場合のみです。


ストレージ・デバイスが輻輳または制約されていることを SIOC が検知できるようにするために、定義されたしきい値が必要です。 輻輳しきい値待ち時間は、ストレージ・タイプによって異なります。 デフォルトではピーク・スループットの 90% が選択されます。 ピーク・スループット値のパーセンテージは、VMware データ・ストアがその推定ピーク・スループットのパーセンテージを使用している場合の推定待ち時間しきい値を示します。


VMware データ・ストアまたは VMDK にとって適切でない方法で SIOC を構成すると、パフォーマンスに重大な影響を及ぼす可能性があります。
{:important}


### VMware データ・ストアのストレージ入出力制御の構成

1. vSphere Web Client ナビゲーターで VMware データ・ストアを参照します。
2. **「管理」**タブをクリックします。
3. **「設定」**をクリックし、**「一般」**をクリックします。
4. **「データ・ストア機能 (Datastore Capabilities)」**の**「編集」**をクリックします。
5. **「ストレージ入出力制御を有効にする (Enable Storage I/O Control)」**チェック・ボックスをオンにします。<br/>
   ![NSF VMware データ・ストア](/images/3_0.png)
6. **「OK」**をクリックします。

この設定は (ホストではなく) VMware データ・ストアに固有の設定です。
{:note}


### {{site.data.keyword.BluVirtServers_short}} のストレージ入出力制御の構成

SIOC を使用して、個々の VM の個々の仮想ディスクを制限したり、異なる数の割り当てを付与したりすることもできます。 ディスクを制限し、異なる数の割り当てを付与することで、取得した {{site.data.keyword.filestorage_short}} ボリュームの IOPS 数のワークロードに環境を合わせることができます。 この制限は IOPS の単位で設定します。異なるウェイトまたは「割り当て数」を設定できます。 割り当て数を「高 (High)」(割り当て数 2,000) に設定した仮想ディスクは、「標準 (Normal)」(割り当て数 1,000) に設定したディスクの 2 倍、また、「低 (Low)」(割り当て数 500) に設定したディスクの 4 倍の入出力を受け取ります。 「標準 (Normal)」がすべての VM のデフォルト値であるため、VM での必要に合わせて「高 (High)」または「低 (Low)」に値を調整する必要があります。

VDisk の割り当て数と制限を変更するには、以下の手順を実行します。

1. **「VM およびテンプレート (VMs and Templates)」**インベントリーで「{{site.data.keyword.BluVirtServers_short}}」を選択します。
2. 入出力制御を行う {{site.data.keyword.BluVirtServers_short}} を選択します。
3. **「管理」**タブをクリックし、**「設定」**をクリックします。 **「編集」**をクリックします。
4. **ハード・ディスク**の矢印を展開します。 実際の環境に応じて、**「割り当ての変更 (Modify the Shares)」**または**「制限 - IOP (Limit-IOPs)」**を選択します。 リストから仮想ハード・ディスクを選択し、「割り当て (Shares)」の選択を変更して、{{site.data.keyword.BluVirtServers_short}} に割り振る相対的な割り当て数 (「低 (Low)」、「標準 (Normal)」、または「高 (High)」) を選択します。 **「カスタム」**をクリックして、ユーザー定義の割り当て値を入力することもできます。
5. **「制限 - IOPS (Limit - IOPS)」**列をクリックし、仮想マシンに割り振るストレージ・リソースの最大値を入力します。
6. **「OK」**をクリックします。


このプロセスは、SIOC が有効になっていない場合でも、{{site.data.keyword.BluVirtServers_short}} 内の個々の vDisk のリソース使用量の上限を設定する目的で使用できます。 これらの設定は、SIOC で使用されますが、ホストではなく個々のゲストにのみ適用されます。
{:important}


## ESXi ホスト・サイドの設定の構成

NFS ストレージ用に ESXi 5.x ホストを構成するには、いくつかの追加設定が必要です。 それぞれをどう設定する必要があるかを、次の表に示します。

|パラメーター | 設定値 |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	vSphere 5.0/5.1 の場合は 128 に設定 <br/> vSphere 5.5 以降の場合は 512 に設定 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (vSphere 6.0 以降のみ) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |
{: caption="表 2 - ホスト・サイドの設定" caption-side="top"}

### CLI を使用することによって ESXi 5.x ホスト上で拡張構成パラメーターを更新する

次の例では、CLI を使用することにより、拡張構成パラメーターを設定した後、それらを検査しています。 例で使用している `esxcfg-advcfg` ツールは、ESXi 5.x ホスト上の `/usr/sbin` ディレクトリー内にあります。

   - ESXi 5.x CLI からの拡張構成パラメーターの設定。

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and higher)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and higher)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - ESXi 5.x CLI からの拡張構成パラメーターの確認。

    ```
    #esxcfg-advcfg -g /Net/TcpipHeapSize
    #esxcfg-advcfg -g /Net/TcpipHeapMax
    #esxcfg-advcfg -g /NFS/MaxVolumes
    #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and higher)
    #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
    #esxcfg-advcfg -g /NFS/HeartbeatFrequency
    #esxcfg-advcfg -g /NFS/HeartbeatTimeout
    #esxcfg-advcfg -g /NFS/MaxQueueDepth
    #esxcfg-advcfg -g /Disk/QFullSampleSize
    #esxcfg-advcfg -g /Disk/QFullThreshold
    ```
拡張単一サイト VMware リファレンス・アーキテクチャーについて詳しくは、[こちら](/docs/infrastructure/virtualization?topic=Virtualization-advanced-single-site-vmware-reference-architecture){: external}を参照してください。
{:tip}
