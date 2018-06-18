---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# VMware を使用する場合の{{site.data.keyword.filestorage_short}}のアーキテクチャー・ガイド

{{site.data.keyword.BluSoftlayer_full}} の vSphere 5.5 と vSphere 6.0 の環境で {{site.data.keyword.filestorage_full}} を注文して構成するための手順を以下に示します。VMWare ホストへの必要な接続数が 8 を超える場合は、NFS {{site.data.keyword.filestorage_short}}を使用することがベスト・プラクティスです。

## {{site.data.keyword.filestorage_short}}の概要

{{site.data.keyword.filestorage_short}}は、予測可能なレベルのパフォーマンスが求められる、入出力負荷の高いアプリケーションをサポートできるように設計されています。 プロトコル・レベルの IOPS (1 秒あたりの入出力操作数) を個々のボリュームに割り振ることで、予測可能なパフォーマンスを実現しています。

{{site.data.keyword.filestorage_short}}のオファリングの利用とマウントは、NFS 接続を使用して行います。 VMware デプロイメントでは、単一ボリュームを最大 64 台の ESXi ホストに共有ストレージとしてマウントしたり、複数のボリュームをマウントしてストレージ・クラスターを作成し、vSphere Storage Distributed Resource Scheduling (DRS) を使用したりできます。

エンデュランスとパフォーマンスの{{site.data.keyword.filestorage_short}}の価格オプションと構成オプションは、予約したスペースと提供される IOPS の組み合わせに基づいて課金されます。

### 1. {{site.data.keyword.filestorage_short}}の考慮事項

{{site.data.keyword.filestorage_short}}を注文するときは、次の情報を考慮してください。

- サイズを決定する際には、必要なワークロードのサイズとスループットを考慮してください。 容量 (IOPS/GB) に応じて性能が直線的に高くなるエンデュランス・サービスにとっては、サイズが重要になります。 一方、パフォーマンス・サービスでは、管理者が容量と性能を個別に選択できます。性能はスループット要件によって決まります。 <br/> **注**: スループットは IOPS x 16 KB という式で計算します。 IOPS は、16 KB ブロック・サイズの読み取り/書き込みが 50/50 の割合で混在するものとして測定されます。 <br/> **注**: ブロック・サイズを大きくすると、スループットは向上しますが IOPS は減少します。 例えば、ブロック・サイズを 2 倍にして 32 KB にすると、最大スループットは維持されますが、IOPS は半減します。
- NFS は、例えば `lookup`、`getattr`、`readdir` など、他の多くのファイル制御操作を使用します。読み取り/書き込み操作に加えて、これらの操作も IOPS と見なされ、操作のタイプや NFS のバージョンに応じて異なります。
- 技術的には、複数のボリュームをまとめてストライピングして IOPS とスループットを向上させることができます。 しかし、VMware は、パフォーマンスの低下を防ぐために、単一の仮想マシン・ファイル・システム (VMFS) データ・ストアをボリュームごとに使用することを推奨しています。
- {{site.data.keyword.filestorage_short}}のボリュームは、許可されたデバイス、サブネット、または IP アドレスに公開されます。
- スナップショット・サービスとレプリケーション・サービスは、エンデュランス・{{site.data.keyword.filestorage_short}}のボリュームのネイティブでのみ使用できます。 パフォーマンス・{{site.data.keyword.filestorage_short}}には、これらの機能はありません。
- パス・フェイルオーバー中のストレージの切断を避けるために、{{site.data.keyword.IBM}} は、適切なタイムアウト値を設定する VMWare ツールをインストールすることをお勧めします。 値を変更する必要はありません。VMWare ホストが接続を失わないようにするには、デフォルト設定で十分です。
- {{site.data.keyword.BluSoftlayer_full}} 環境では NFS v3 と NFS v4.1 の両方がサポートされます。 しかし、{{site.data.keyword.IBM}} は NFS v3 を使用することをお勧めします。NFS v4.1 は、ステートフル・プロトコルである (NFSv3 のようなステートレス・プロトコルではない) ため、ネットワーク・イベント中にプロトコルの問題が発生する可能性があります。 NFS v4.1 では、すべての操作を停止してからロック再利用を行う必要があります。 それらの操作の実行中に、切断される可能性があります。

#### NFS プロトコルと VMware 機能の対応表。
<table>
  <caption>表 1 は、2 種類のバージョンの NFS に適用される vSphere 機能を示しています</caption>
 <thead>
  <tr>
   <th>vSphere の機能</th>
   <th>NFS バージョン 3</th>
   <th>NFS バージョン 4.1</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vMotion および Storage vMotion</td>
   <td>○</td>
   <td>○</td>
  </tr>
  <tr>
   <td>高可用性 (HA)</td>
   <td>○</td>
   <td>○</td>
  </tr>
  <tr>
   <td>フォールト・トレランス (FT)</td>
   <td>○</td>
   <td>○</td>
  </tr>
  <tr>
   <td>分散リソース・スケジューラー (DRS)</td>
   <td>○</td>
   <td>○</td>
  </tr>
  <tr>
   <td>ホスト・プロファイル</td>
   <td>○</td>
   <td>○</td>
  </tr>
  <tr>
   <td>ストレージ DRS</td>
   <td>○</td>
   <td>×</td>
  </tr>
  <tr>
   <td>ストレージ入出力制御</td>
   <td>○</td>
   <td>×</td>
  </tr>
  <tr>
   <td>サイト・リカバリー・マネージャー</td>
   <td>○</td>
   <td>×</td>
  </tr>
  <tr>
   <td>仮想ボリューム</td>
   <td>○</td>
   <td>×</td>
  </tr>
 </tbody>
</table>
*出典: [VMWare - NFS Protocols and ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*





### 2. エンデュランス・{{site.data.keyword.filestorage_short}}のスナップショット

エンデュランス・{{site.data.keyword.filestorage_short}}を使用すると、管理者は、各ストレージ・ボリュームのスナップショット・コピーを自動的に作成および削除するスナップショット・スケジュールを設定できます。 また、自動スナップショットのスナップショット・スケジュール (毎時、日次、週次) を追加で作成したり、事業継続と災害復旧 (BCDR) のシナリオのスナップショットを随時手動で作成したりすることもできます。ボリューム所有者に、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}を介して、保存されたスナップショットと使用されたスペースについての自動アラートが送信されます。

スナップショットを使用するには、「スナップショット・スペース」が必要であることに注意してください。 スペースはボリュームを最初に注文するときに取得できます。また、最初のプロビジョニングの後で、**「ボリュームの詳細 (Volume Details)」**ページの**「アクション」**をクリックし、**「スナップショット・スペースの追加」**を選択して取得することもできます。

VMware 環境はスナップショットを認識しないことに注意してください。 エンデュランス・{{site.data.keyword.filestorage_short}}のスナップショット機能を VMware のスナップショットと混同しないでください。 エンデュランス・{{site.data.keyword.filestorage_short}}のスナップショット機能を使用したリカバリーは、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}から行う必要があります。 

エンデュランス・{{site.data.keyword.filestorage_short}}のボリュームを復元するには、エンデュランス・{{site.data.keyword.filestorage_short}}上のすべての VM の電源をオフにする必要があります。 ESXi ホストから一時的にボリュームをアンマウントして、復元処理中のデータ破損を回避する必要があります。

スナップショットの構成方法について詳しくは、[スナップショット](snapshots.html)の記事を参照してください。


### 3. ファイル・ストアのレプリケーション

レプリケーションでは、スナップショット・スケジュールの 1 つを使用して、スナップショットを自動的にリモート・データ・センター内の宛先ボリュームにコピーします。 災害やデータ破損が発生した場合に、リモート・サイトのコピーを復旧できます。


レプリカにより、以下が可能になります。

- 宛先ボリュームにフェイルオーバーして、サイト障害やその他の災害から素早く復旧する
- DR コピー内の特定の時点にフェイルオーバーする

レプリケーションを行うには、その前にスナップショット・スケジュールを作成する必要があります。 フェイルオーバー時には、プライマリー・データ・センターのストレージ・ボリュームからリモート・データ・センターの宛先ボリュームにスイッチを「切り替え」ます。 例えば、プライマリー・データ・センターがロンドンにあり、セカンダリー・データ・センターがアムステルダムにあるとします。 障害が発生したら、アムステルダムにフェイルオーバーします。つまり、アムステルダムの vSphere Cluster インスタンスにある新プライマリー・ボリュームに接続します。

ロンドンのボリュームが修復されたら、アムステルダムのボリュームのスナップショットが作成されます。 その後、ロンドンにフォールバックしてロンドンのコンピューティング・インスタンスのボリュームをプライマリーに戻します。 

ボリュームをプライマリー・データ・センターにフェイルバックする前に、リモート・サイトでの使用を停止する必要があります。 新規または変更された情報のスナップショットが取得され、プライマリー・データ・センターに複製された後に、実動サイトの ESXi ホストに再度マウントできるようになります。

レプリケーションの構成方法について詳しくは、[レプリケーション](replication.html)の情報ページを参照してください。

**注**: 破損したり、ハッキングされたり、感染したりしていても、スナップショット・スケジュールとスナップショット保持設定に従って無効なデータが複製されます。 最短のレプリケーション期間にすると、より適切なリカバリー・ポイントに復旧できます。 また、これにより、無効なデータのレプリケーションにも少ない時間で対応できます。


## {{site.data.keyword.filestorage_short}}の注文

VMware ESXi 5 環境用の{{site.data.keyword.filestorage_short}}を注文して構成できます。 以下の情報と拡張単一サイト VMware のリファレンス・アーキテクチャーを使用して、これらのストレージ・オプションのいずれかを VMware 環境にセットアップできます。


{{site.data.keyword.filestorage_short}}は、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}から**「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**にアクセスして「{{site.data.keyword.filestorage_short}}」ページで注文できます。


### 1. {{site.data.keyword.filestorage_short}}を注文する

{{site.data.keyword.filestorage_short}}を注文するには、以下の手順を実行します。
1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}のホーム・ページの**「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。
2. **「{{site.data.keyword.filestorage_short}}」**ページの**「{{site.data.keyword.filestorage_short}}を注文」**をクリックします。
3. **「ストレージ・タイプの選択」**リストから**「エンデュランス」**または**「パフォーマンス」**を選択します。
4. 場所を選択します。 拡張機能を備えたデータ・センターには、アスタリスクが表示されます。 必ず、先に注文した ESXi ホストと同じ場所に新しいストレージが追加されるようにしてください。
5. 請求方法を選択します。 請求オプションとして毎月または毎時を選択できます。
6. ストレージ・スペース量を GB 単位で選択します。 TB の場合、1 TB は 1,000 GB、12 TB は 12,000 GB に相当します。
7. IOPS 量を 100 単位で入力するか、IOPS ティアを選択します。
8. スナップショット・スペースのサイズを指定します。
9. **「次へ進む (Continue)」**をクリックします。
10. プロモーション・コードがある場合は入力し、**「再計算」**をクリックします。
11. 注文を確認します。
12. **「マスター・サービス契約を読み、その契約条件に同意します」**チェック・ボックスにチェック・マークを付けます。
13. **「注文する」**をクリックして注文を送信するか、**「キャンセル」**をクリックして注文を送信せずにフォームを閉じます。

ストレージが 1 分もしないうちにプロビジョンされ、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}の**「{{site.data.keyword.filestorage_short}}」**ページに表示されます。



### 2. {{site.data.keyword.filestorage_short}}の使用をホストに許可する

ボリュームがプロビジョンされたら、そのボリュームを使用する {{site.data.keyword.BluBareMetServers_full}} または {{site.data.keyword.BluVirtServers_full}} に、ストレージへのアクセスを許可する必要があります。 ボリュームを許可するには、以下の手順を実行します。

1. **「ストレージ」** > **「{{site.data.keyword.filestorage_short}}」**をクリックします。
2. **「エンデュランス・ボリューム・アクション (Endurance Volume Actions)」**または**「パフォーマンス・ボリューム・アクション (Performance Volume Actions)」**メニューで**「アクセス・ホスト (Access Host)」**を選択します。
3. **「サブネット」**ラジオ・ボタンを選択します。
4. ESXi ホスト上の vmkernel ポートに割り当てられている使用可能なサブネットのリストからサブネットを選択し、**「送信」**をクリックします。<br/> **注**: ストレージ・ボリュームと同じデータ・センター内のサブスクライブされたサブネットが表示されます。


サブネットが許可されたら、ボリュームのマウント時に使用するエンデュランス・ストレージ・サーバーまたはパフォーマンス・ストレージ・サーバーのホスト名をメモします。 この情報は、特定のボリュームをクリックすると、{{site.data.keyword.filestorage_short}}の詳細ページに表示されます。


##  VMware 仮想マシン・ホストの構成

### 1. 前提条件

VMware 構成プロセスを開始する前に、以下の前提条件が満たされていることを確認してください。

- VMware ESXi を搭載した {{site.data.keyword.BluBareMetServers}} が、適切なストレージ構成と ESXi ログイン資格情報を使用してプロビジョンされている。
- {{site.data.keyword.BluSoftlayer_full}} Windows 物理サーバーまたは{{site.data.keyword.virtualmachinesshort}}が、{{site.data.keyword.BluBareMetServers}} と同じデータ・センター内にある。 {{site.data.keyword.BluSoftlayer_full}} Windows VM のパブリック IP アドレスおよびログイン資格情報を含む。
- インターネットにアクセスできるコンピューターがあり、Web ブラウザー・ソフトウェアとリモート・デスクトップ・プロトコル (RDP) クライアントがインストールされている。


### 2. VMware ホスト構成手順

仮想ホストを構成するには、以下の手順を実行します。

1. インターネットに接続されたコンピューターから RDP クライアントを起動し、vSphere vCenter がインストールされているのと同じデータ・センターにプロビジョンされた {{site.data.keyword.BluVirtServers_full}} への RDP セッションを確立します。
2. {{site.data.keyword.BluVirtServers_short}} で Web ブラウザーを起動し、vSphere Web Client を経由して VMware vCenter に接続します。
3. **「ホーム」**画面で**「ホストおよびクラスター (Hosts and Clusters)」**を選択します。 左側のパネルを展開し、このデプロイメントに使用する **VMware ESXi サーバー**を選択します。
4. vSphere ホストに NFS クライアントを構成するために、すべてのホストで NFS クライアント用のファイアウォール・ポートが開かれていることを確認します。 これは、最新リリースの vSphere では自動的に開かれます。 ポートが開いているかどうかを確認するには、VMware® vCenter™ の**「ESXi ホストの管理 (ESXi host Manage)」**タブに移動し、**「設定」**を選択し、**「セキュリティー・プロファイル (Security Profile)」**を選択します。 **「ファイアウォール (Firewall)」**セクションで**「編集」**をクリックし、**「NFS クライアント (NFS Client)」**までスクロールダウンします。
5. **「すべての IP アドレスからの接続を許可する (Allow connection from any IP address)」**が指定されているか、IP アドレスのリストが指定されていることを確認します。 <br/>
   ![接続を許可する](/images/1_4.png)
6. **「ESXi ホストの管理 (ESXi host Manage)」**タブに移動し、**「管理」**を選択してから**「ネットワーキング」**を選択して、ジャンボ・フレームを構成します。
7. **「VMkernel アダプター (VMkernel adapters)」**を選択し、**「vSwitch」**を強調表示し、**「編集」**をクリックします。 (鉛筆アイコン)
8. **「NIC 設定 (NIC setting)」**を選択し、NIC MTU が 9000 に設定されていることを確認します。
   - 必要に応じて、MTU を 9000 に設定し、**「OK」**をクリックします。
9. 必要に応じて、ジャンボ・フレーム設定を以下のように検証できます。
   - Windows の場合: ping -f -l 8972 a.b.c.d
   - UNIX の場合: ping -s 8972 a.b.c.d
   このコマンドで a.b.c.d は、隣接する {{site.data.keyword.BluVirtServers_short}} インターフェースです。
   出力は次のようになります。
   ```
   ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
   8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
   ```

VMware およびジャンボ・フレームについて詳しくは、[こちら](https://kb.vmware.com/s/article/1003712){:new_window}を参照してください。


### 3. 仮想スイッチへのアップリンク・アダプターの追加

1. **「ESXi ホストの管理 (ESXi host Manage)」**タブに移動し、**「管理」**を選択してから**「ネットワーキング」**を選択して、新規アップリンク・アダプターを構成します。
2. **「物理アダプター (Physical adapters)」**タブを選択します。
3. **「ホスト・ネットワーキングの追加 (Add host networking)」**をクリックします。 (正符号付きの地球儀アイコン)
4. 接続タイプとして**「物理ネットワーク・アダプター (Physical Network Adapter)」**を選択し、**「次へ」**をクリックします。
5. 既存の **vSwitch** を選択し、**「次へ」**をクリックします。
6. **「未使用のアダプター (Unused adapters)」**を選択し、**「アダプターの追加 (Add adapters)」**をクリックします。 (正符号)
7. もう一方の「接続済み (Connected)」アダプターをクリックし、**「OK」**をクリックします。 <br/>
   ![物理アダプターをスイッチに追加する](/images/2_3.png)
8. **「次へ」**をクリックして、**「完了」**をクリックします。
9. **「仮想スイッチ (Virtual switches)」**タブに戻り、**「仮想スイッチ (Virtual Switches)」**見出しの下にある**「設定の編集 (Edit setting)」**アイコンを選択します。 (鉛筆アイコン)
10. 左側で、vSwitch の**「チーミングとフェイルオーバー (Teaming and failover)」**項目を選択します。
11. **「ロード・バランシング (Load balancing)」**オプションが**「発信元の仮想ポートに基づいたルート (Route based on the originating virtual port)」**に設定されていることを確認し、**「OK」**をクリックします。


### 4. ESXi 静的ルーティングの構成 (オプション)

このアーキテクチャー・ガイドのネットワーク構成では、最小限の数のポート・グループを使用します。 NFS ストレージ用に VMkernal ポート・グループをセットアップした場合は、追加の手順を実行する必要があります。 デフォルトでは、ESXi は NFS ボリュームと同じサブネット上にある vmkernel ポートを使用して、エンデュランス/パフォーマンス・ストレージに NFS ボリュームをマウントします。 NFS ボリュームをマウントするためにレイヤー 3 ルーティングを使用しているため、NFS ボリュームをマウントするように構成した vmkernel ポートを ESXi に強制的に使用させる必要があります。 このために、エンデュランス/パフォーマンス・ストレージ・アレイへの静的ルートを作成する必要があります。

1. 静的ルートを構成するには、パフォーマンスまたはエンデュランスを使用する各 ESXi ホストに SSH で接続し、以下のコマンドを実行します。 ping コマンド (最初のコマンド) の結果として得られる IP アドレスを、以下の esxcli ネットワーク・コマンドで使用する必要があります。
   - `ping <hostname of the endurance/performance array>`

      **注**: NFS ストレージの DNS ホスト名は、複数の IP アドレスが割り当てられる転送ゾーン (FZ) であることに注意してください。 これらの IP アドレスは静的であり、その特定の DNS ホスト名に属します。  これらの IP アドレスはすべて、特定のボリュームにアクセスするために使用できます。
   - `esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32`

2. ESXi 5.0 以前では、静的ルートはリブートをまたいで保持されません。 追加した静的ルートを保持させるには、これらのコマンドを各ホストの /etc/rc.local.d/ ディレクトリーにある local.sh ファイルに追加する必要があります。 そのためには、Visual Editor を使用して local.sh ファイルを開き、上記のコマンドを行終了 0 の上に追加して実行されるようにします。
   - 次の手順でボリュームをマウントする際に使用できるように、IP アドレスをメモします。
   - このプロセスは、ESXi ホストにマウントする NFS ボリュームごとに実行する必要があります。
   - VMware KB の記事「[Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/s/article/2001426){:new_window}」を参照してください。


##  ESXi ホストへの{{site.data.keyword.filestorage_short}}・ボリュームのマウント

1. Web ページの上部にある**「vCenter に移動 (Go to vCenter)」**アイコンをクリックし、次に**「ホストおよびクラスター (Hosts and Clusters)」**をクリックします。
2. **「関連オブジェクト (Related Object)」**タブの下にある**「データ・ストア (Datastores)」**をクリックします。 **「新規データ・ストアの作成 (Create a new datastore)」**アイコンをクリックします。
3. **「新規データ・ストア (New Datastore)」**画面で、データ・ストア (使用する ESXi ホスト) の場所を選択し、**「次へ」**をクリックします。
4. **「タイプ」**画面で**「NFS」**ラジオ・ボタンを選択し、**「次へ」**をクリックします。
5. **「名前と構成 (Name and configuration)」**画面で、データ・ストアに付ける名前を入力します。 さらに、NFS サーバーのホスト名を入力します。 NFS サーバーの FQDN を使用すると、その基礎サーバーへの最適なトラフィック配信が行われます。 IP アドレスも有効ですが、特定の状況を除き、あまり使用されません。 フォルダー名を /foldername の形式で入力します。
6. **「ホストのアクセス可能性 (Host accessibility)」**画面で、NFS データ・ストアをマウントするホストをすべて選択し、**「次へ」**をクリックします。
7. 次の画面で入力を確認し、**「完了」**をクリックします。
8. 他の{{site.data.keyword.filestorage_short}}・ボリュームについても同じ手順を繰り返します。

**注**: {{site.data.keyword.BluSoftlayer_full}} はデータ・ストアへの接続に FQDN 名を使用することをお勧めします。 直接 IP アドレッシングを使用すると、FQDN を使用することで機能するロード・バランシング・メカニズムがバイパスされる可能性があります。 FQDN の代わりに IP アドレスを使用するには、以下のコマンドを実行して IP アドレスを取得します。

  - `ping <hostname of the endurance/performance array>`
  - ESXi ホストで以下を実行します。
    ```
    ~ # vmkping nfsdal0902a-fz.service.softlayer.com
    PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
    64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
    10.2.125.80 is the IP address associated with the FQDN
    ```

## ESXi ストレージ入出力制御を有効にする (オプション)

ストレージ入出力制御 (SIOC) は、Enterprise Plus ライセンスを利用するお客様が使用できる機能です。 SIOC を有効にした環境では、単一 VM のデバイス・キューの長さが変更されます。 デバイス・キューの長さの変更により、すべての VM のストレージ・アレイ・キューが縮小されて、ストレージ・キューが均等に割り当てられ、調整されるようになります。 SIOC は、リソースが制約されていて、定義したしきい値をストレージ入出力の待ち時間が超える場合にのみ有効です。


ストレージ・デバイスが輻輳または制約されていることを SIOC が検知できるようにするために、定義されたしきい値が必要です。 輻輳しきい値待ち時間は、ストレージ・タイプによって異なります。デフォルトではピーク・スループットの 90% が選択されます。 ピーク・スループット値のパーセンテージは、データ・ストアがその推定ピーク・スループットのパーセンテージを使用している場合の推定待ち時間しきい値を示します。


データ・ストアまたは VMDK にとって適切でない SIOC を構成すると、パフォーマンスに重大な影響を及ぼす可能性があることに注意してください。



### 1. データ・ストアのストレージ入出力制御

エンデュランス・ストレージとパフォーマンス・ストレージの推奨値を使用して SIOC を有効にするには、以下の手順を実行します。

1. vSphere Web Client ナビゲーターでデータ・ストアを参照します。
2. **「管理」**タブをクリックします。
3. **「設定」**をクリックし、**「一般」**をクリックします。
4. **「データ・ストア機能 (Datastore Capabilities)」**の**「編集」**をクリックします。
5. **「ストレージ入出力制御を有効にする (Enable Storage I/O Control)」**チェック・ボックスをオンにします。<br/>
   ![NSF データ・ストア](/images/3_0.png)
6. **「OK」**をクリックします。

**注**: この設定は (ホストではなく) データ・ストアに固有の設定です。


### 2. {{site.data.keyword.BluVirtServers_short}} のストレージ入出力制御

SIOC を使用して、個々の VM の個々の仮想ディスクを制限したり、異なる数の割り当てを付与したりすることもできます。 ディスクを制限し、異なる数の割り当てを付与することで、取得した{{site.data.keyword.filestorage_short}}・ボリュームの IOPS 数のワークロードに環境を合わせることができます。 この制限は IOPS の単位で設定します。異なるウェイトまたは「割り当て数」を設定できます。 割り当て数を「高 (High)」(割り当て数 2,000) に設定した仮想ディスクは、「標準 (Normal)」(割り当て数 1,000) に設定したディスクの 2 倍、また、「低 (Low)」(割り当て数 500) に設定したディスクの 4 倍の入出力を受け取ります。 「標準 (Normal)」がすべての VM のデフォルト値であるため、実際に入出力を必要とする VM に合わせて「標準 (Normal)」より上または下に値を調整する必要があります。


vDisk の割り当て数と制限を変更するには、以下の手順を実行します。

1. **「VM およびテンプレート (VMs and Templates)」**インベントリーで「{{site.data.keyword.BluVirtServers_short}}」を選択します。 アイコンは、Web ページの左上にあります。
2. 入出力制御を行う {{site.data.keyword.BluVirtServers_short}} を選択します。
3. **「管理」**タブをクリックし、**「設定」**をクリックします。 **「編集」**をクリックします。
4. ハード・ディスクのプルダウンの矢印を展開します。 環境に応じて、「割り当て (Shares)」または「制限 - IOPS (Limit-IOPs)」を変更します。 リストから仮想ハード・ディスクを選択し、「割り当て (Shares)」の選択を変更して、{{site.data.keyword.BluVirtServers_short}} に割り振る相対的な割り当て量 (「低 (Low)」、「標準 (Normal)」、または「高 (High)」) を選択します。 **「カスタム」**をクリックして、ユーザー定義の割り当て値を入力することもできます。
5. 「制限 - IOPS (Limit - IOPS)」列をクリックし、仮想マシンに割り振るストレージ・リソースの上限を入力します。
6. **「OK」**をクリックします。


   **注**: 上記のプロセスは、SIOC が有効になっていない場合でも、{{site.data.keyword.BluVirtServers_short}} 内の個々の vDisk のリソース使用量の上限を設定する目的で使用できます。 これらの設定は、SIOC で使用されますが、ホストではなく個々のゲストにのみ適用されます。





## ESXi ホスト・サイドの設定

NFS ストレージを使用できるように ESXi 5.x ホストをセットアップするには、いくつかの追加設定が必要です。

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


- ESXi 5.x ホストで CLI を使用して拡張構成パラメーターを設定する場合:
  以下の例では、CLI を使用してこれらの拡張構成パラメーターを設定し、設定後に確認しています。 例で使用している esxcfg-advcfg ツールは、ESXi 5.x ホスト上の /usr/sbin ディレクトリー内にあります。

   - ESXi 5.x CLI からの拡張構成パラメーターの設定:
   ```
   #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
   #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
   #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and above)
   #esxcfg-advcfg -s 256 /NFS/MaxVolumes
   #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and above)
   #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
   #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
   #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
   #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
   #esxcfg-advcfg -s 8 /Disk/QFullThreshold
   ```

- ESXi 5.x CLI からの拡張構成パラメーターの確認:
   ```
   #esxcfg-advcfg -g /Net/TcpipHeapSize
   #esxcfg-advcfg -g /Net/TcpipHeapMax
   #esxcfg-advcfg -g /NFS/MaxVolumes
   #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 and above)
   #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -g /NFS/HeartbeatFrequency
   #esxcfg-advcfg -g /NFS/HeartbeatTimeout
   #esxcfg-advcfg -g /NFS/MaxQueueDepth
   #esxcfg-advcfg -g /Disk/QFullSampleSize
   #esxcfg-advcfg -g /Disk/QFullThreshold
   ```


## SoftLayer でのジャンボ・フレームの有効化 - Windows および Linux

{{site.data.keyword.BluSoftlayer_full}} は、ストレージの速度を完全に実現するためには、ジャンボ・フレームを 9,000 MTU で有効にする必要があることを明記しています。


ジャンボ・フレームとは、標準最大伝送単位 (MTU) である 1,500 バイトよりペイロードが大きいイーサネット・フレームです。 ジャンボ・フレームは、1 Gbps 以上をサポートするローカル・エリア・ネットワークで使用され、9,000 バイトのサイズにすることができます。


ソース・デバイスから <-> スイッチ <-> ルーター <-> スイッチ <-> 宛先デバイスまでのネットワーク・パス全体で、同じジャンボ・フレームを構成する必要があります。 チェーン全体で同じ設定になっていないと、チェーン全体がデフォルトの最小設定に変更されます。 {{site.data.keyword.BluSoftlayer_full}} のネットワーク・デバイスは現在 9,000 に設定されています。 すべてのカスタマー・デバイスを同じ設定にする必要があります。

### Windows

1. **「ネットワークと共有センター」**を開きます。
2. **「アダプターの設定の変更」**をクリックします。
3. ジャンボ・フレームを有効にする NIC を右クリックし、**「プロパティ」**を選択します。
4. **「ネットワーク」**タブでネットワーク・アダプターの**「構成」**をクリックします。
5. **「詳細設定」**タブを選択します。
6. **「ジャンボフレーム」**を選択し、NIC に応じて、値を「無効」から適切な値 (9kB MTU、9,014 バイトなど) に変更します。
7. すべてのダイアログで**「OK」**をクリックします。

変更を加えると、数秒間 NIC がネットワーク接続を失うので注意してください。 また、変更を有効にするためにリブートする必要があります。



### LINUX

1. eth0 インターフェースのネットワーク構成ファイル (例: /etc/sysconfig/network-script/ifcfg-eth0 (CentOS / RHEL / Fedora Linux)) を編集します。`# vi/etc/sysconfig/network-script/ifcfg-eth0`

2. フレームのサイズをバイト単位で指定する構成ディレクティブ「MTU 9000」を追加します。

3. ファイルを閉じて保存します。 インターフェース eth0 を再始動します。`# /etc/init.d/networking restart (これにより、ネットワーク接続が一時的に失われます)`


注 (Debian / Ubuntu Linux ユーザーのみ): Debian / Ubuntu Linux ユーザーは、MTU=9000 を /etc/network/interfaces 構成ファイルに追加する必要があります。

拡張単一サイト VMware リファレンス・アーキテクチャーについて詳しくは、[こちら](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}を参照してください。
