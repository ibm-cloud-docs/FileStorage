---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}}（含 VMware）的架構手冊

以下是在 {{site.data.keyword.BluSoftlayer_full}} 為 vSphere 5.5 及 vSphere 6.0 環境訂購及配置 {{site.data.keyword.filestorage_full}} 的步驟。如果您需要超過 8 條與 VMWare 主機的連線，則使用 NFS {{site.data.keyword.filestorage_short}} 是最佳作法。

## {{site.data.keyword.filestorage_short}} 概觀

{{site.data.keyword.filestorage_short}} 的設計旨在支援需要可預測效能層次的高 I/O 應用程式。透過將通訊協定層次的每秒輸入/輸出作業數 (IOPS) 配置給個別磁區，即可達成可預測效能。

{{site.data.keyword.filestorage_short}} 供應項目是透過 NFS 連線進行存取及裝載。在 VMware 部署中，單一磁區最多可以裝載至 64 台 ESXi 主機作為共用儲存空間，或者您可以裝載多個磁區，以建立儲存空間叢集來使用「vSphere 儲存空間動態資源排程」。

「耐久性」及「效能」{{site.data.keyword.filestorage_short}} 的定價及配置選項，是根據保留空間與所提供 IOPS 的組合進行收費。

### 1. {{site.data.keyword.filestorage_short}} 考量

訂購 {{site.data.keyword.filestorage_short}} 時，請記住下列資訊及考量：

- 佈建 {{site.data.keyword.filestorage_short}} 磁區之後，就無法變更儲存空間大小、IOPS 及作業系統。您要對空間量、IOPS 數目或作業系統所做的任何變更都需要佈建新的磁區。必須使用 VMware Storage vMotion 將任何儲存在先前磁區中的資料移轉至新的磁區。
- 決定大小時，請考量工作負載的大小以及所需的傳輸量。大小與「耐久性」服務有重大關係，「耐久性」服務會根據容量 (IOPS/GB) 以線性方式擴充效能。這點與「效能」服務相反，「效能」服務容許管理者獨立地選擇容量及效能。傳輸量需求與「效能」有重大關係。<br/> **附註**：傳輸量計算方式為 IOPS x 16 KB。IOPS 的測量基礎是 16 KB 區塊大小，並且具有 50/50 的讀寫混合。<br/> **附註**：增加區塊大小會增加傳輸量，但會減少 IOPS。例如，將區塊大小加倍為 32KB 的區塊可維持最大傳輸量，但會將 IOPS 減半。
- NFS 利用許多其他檔案控制作業，列舉一些包括例如 lookup、getattr 及 readdir。除了讀/寫作業之外，這些作業也可以計算為 IOPS，而且會依作業類型及 NFS 版本而不同。
- 在技術上，可以將多個磁區等量配置在一起，以達到更高的 IOPS 及更多傳輸量，不過，VMware 建議每個磁區有單一的「虛擬機器檔案系統 (VMFS)」資料儲存庫，以避免效能降低。
- {{site.data.keyword.filestorage_short}} 磁區會向授權裝置、子網路或 IP 位址公開。
- Snapshot 及「抄寫」服務一開始只在「耐久性」{{site.data.keyword.filestorage_short}} 磁區上提供。「效能」{{site.data.keyword.filestorage_short}} 沒有這些功能。
- 為避免在路徑失效接手期間發生儲存空間斷線，{{site.data.keyword.IBM}} 建議安裝 VMWare 工具，以設定適當的逾時值。不需要變更值，預設值就足以確保 VMWare 主機不會中斷連線。
- 在 {{site.data.keyword.BluSoftlayer_full}} 環境中，同時支援 NFS 第 3 版及 NFS 4.1 版。不過，{{site.data.keyword.IBM}} 建議使用 NFS 第 3 版。因為 NFS 4.1 版是有狀態的通訊協定（不像 NFSv3 是無狀態的通訊協定），所以在網路事件期間可能會發生通訊協定問題。NFS 4.1 版必須靜止作業，然後執行鎖定收回。進行這些作業時，可能會發生中斷。

####  NFS 通訊協定 VMware 特性支援矩陣。
<table>
 <tbody>
  <tr>
   <th>vSphere 特性</th>
   <th>NFS 第 3 版</th>
   <th>NFS 4.1 版</th>
  </tr>
  <tr>
   <td>vMotion 及 Storage vMotion</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>高可用性 (HA)</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>容錯 (FT)</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>分散式資源排程器 (DRS)</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>主機設定檔</td>
   <td>是</td>
   <td>是</td>
  </tr>
  <tr>
   <td>儲存空間 DRS</td>
   <td>是</td>
   <td>否</td>
  </tr>
  <tr>
   <td>儲存空間 I/O 控制</td>
   <td>是</td>
   <td>否</td>
  </tr>
  <tr>
   <td>Site Recovery Manager</td>
   <td>是</td>
   <td>否</td>
  </tr>
  <tr>
   <td>虛擬磁區</td>
   <td>是</td>
   <td>否</td>
  </tr>
 </tbody>
</table>
*來源：[VMWare - NFS 通訊協定及 ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*





### 2. 耐久性 {{site.data.keyword.filestorage_short}} Snapshot

「耐久性」{{site.data.keyword.filestorage_short}} 容許管理者設定 Snapshot 排程，以自動建立及刪除每一個儲存空間磁區的 Snapshot 副本。他們也可以建立其他 Snapshot 排程（每小時、每日、每週）來自動取得 Snapshot，以及針對企業永續及災難回復 (BCDR) 情境手動建立特定 Snapshot。會透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 將已保留 Snapshot 及耗用空間的自動警示傳遞給磁區擁有者。

請注意，需要有「Snapshot 空間」才能使用 Snapshot。您可以在初次訂購磁區時或初次佈建之後，透過**磁區詳細資料**頁面獲得空間，方法是按一下「動作」下拉按鈕，然後選取**新增 Snapshot 空間**。

請務必注意，VMware 環境不知道 Snapshot。「耐久性」{{site.data.keyword.filestorage_short}} Snapshot 功能不得與 VMware Snapshot 混淆。必須從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 處理任何使用「耐久性」{{site.data.keyword.filestorage_short}} Snapshot 特性的回復。還原「耐久性」{{site.data.keyword.filestorage_short}} 磁區時，需要關閉位於「耐久性」{{site.data.keyword.filestorage_short}} 上所有 VM 的電源，並暫時從 ESXi 主機卸載磁區，以避免在處理程序期間發生資料損毀。

如需有關如何配置 Snapshot 的詳細資料，請參閱 [Snapshot](snapshots.html) 文章。


### 3. 檔案儲存庫抄寫

抄寫使用您的其中一個 Snapshot 排程，自動將 Snapshot 複製到遠端資料中心內的目的地磁區。如果發生資料毀損或災難性事件，則可以在遠端網站中回復副本。


抄本可讓您

- 透過失效接手至目的地磁區，快速從網站故障及其他災難回復
- 失效接手至 DR 副本中的特定時間點

您必須先建立 Snapshot 排程，才能進行抄寫。當您失效接手時，您會將「開關」從主要資料中心的儲存空間磁區快速切換到遠端資料中心的目的地磁區。例如，您的主要資料中心是「倫敦」，而次要資料中心是「阿姆斯特丹」。如果發生故障事件，您會失效接手至「阿姆斯特丹」- 從「阿姆斯特丹」的 vSphere Cluster 實例連接至現行主要磁區。


修復「倫敦」中的磁區之後，會建立「阿姆斯特丹」磁區的 Snapshot，以從「倫敦」的運算實例撤回至「倫敦」及那個再度成為主要磁區的磁區。在磁區撤回至主要資料中心之前，需要先停止在遠端網站的使用。會先建立任何新資訊或已變更資訊的 Snapshot，並抄寫至主要資料中心，然後才能在正式作業網站 ESXi 主機上重新進行裝載。


如需有關如何配置抄寫的詳細資料，請參閱[抄寫](replication.html)資訊頁面。

**附註**：會根據 Snapshot 排程及 Snapshot 保留，來抄寫無效的資料（不論是毀損、受到駭客入侵還是感染病毒）。使用最小的抄寫時間範圍可以提供更好的回復點目標。它也可能使得需要較少時間即可對無效資料的抄寫做出反應。





## 訂購 {{site.data.keyword.filestorage_short}}

您可以針對 VMware ESXi 5 環境訂購及配置 {{site.data.keyword.filestorage_short}}。將下列資訊與「進階單一網站 VMware 參照架構」一起使用，以在 VMware 環境中設定其中一個儲存空間選項。


{{site.data.keyword.filestorage_short}} 可以透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 訂購，方法是透過**儲存空間** > **{{site.data.keyword.filestorage_short}}** 來存取 {{site.data.keyword.filestorage_short}} 頁面。


### 1. 訂購 {{site.data.keyword.filestorage_short}}

請使用下列步驟來訂購 {{site.data.keyword.filestorage_short}}：
1. 按一下 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 首頁上的**儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 按一下 **{{site.data.keyword.filestorage_short}}** 頁面上的**訂購 {{site.data.keyword.filestorage_short}}** 鏈結。
3. 從「選取儲存空間類型」下拉清單中，選取**耐久性**/**效能**。
4. 選取位置。具有改良功能的資料中心會以 `*` 表示。確定新的「儲存空間」將會新增至與先前訂購的 ESXi 主機相同的位置。
5. 選取計費方法。可用的計費選項為按每月及按每小時計費。
6. 選取所需的儲存空間量（以 GB 為單位）。若為 TB，1 TB 等於 1,000 GB，而 12 TB 等於 12,000 GB。
7. 輸入所需的 IOPS 數量（間隔為 100），或選取「IOPS 層級」。
8. 指定「Snapshot 空間」的大小。
9. 按一下**繼續**。
10. 如果您有促銷代碼，請輸入促銷代碼，然後按一下**重新計算**。
11. 檢閱訂單。
12. 勾選**我已閱讀主要服務合約，並同意其中的條款**勾選框。
13. 按一下**下訂單**來提交訂單，或按一下**取消**來關閉表單，而不提交訂單。

在一分鐘以內即會佈建儲存空間，並且它會顯示在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 的 **{{site.data.keyword.filestorage_short}}** 頁面上。



### 2. 授權主機以使用 {{site.data.keyword.filestorage_short}}

佈建磁區之後，必須授權將使用磁區的 {{site.data.keyword.BluBareMetServers_full}} 或 {{site.data.keyword.BluVirtServers_full}} 來存取儲存空間。請使用下列步驟來授權磁區：

1. 按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 在**耐久性**或**效能磁區動作**功能表上，選取**存取主機**。
3. 選取**子網路**圓鈕。
4. 從指派給 ESXi 主機上 vmkernel 埠的可用子網路清單中進行選擇，然後按一下**提交**。<br/> **附註**：所顯示的子網路，將是與儲存空間磁區相同資料中心內的已訂閱子網路。


授權子網路之後，請記下您在裝載磁區時想要使用之「耐久性」或「效能」儲存空間伺服器的主機名稱。按一下特定磁區，即可在 {{site.data.keyword.filestorage_short}} 詳細資料頁面上找到這項資訊。


##  配置 VMware 虛擬機器主機

### 1. 必要條件

開始 VMware 配置處理程序之前，請確定符合下列必要條件：

- 具有 VMware ESXi 的 {{site.data.keyword.BluBareMetServers}} 是使用適當的儲存空間配置及 ESXi 登入認證所佈建。
- {{site.data.keyword.BluSoftlayer_full}} Windows 實體或 {{site.data.keyword.virtualmachinesshort}}，位於與 {{site.data.keyword.BluBareMetServers}} 相同的資料中心內。包括 {{site.data.keyword.BluSoftlayer_full}} Windows VM 的「公用 IP 位址」及登入認證。
- 具有網際網路存取的電腦，並且已安裝 Web 瀏覽器軟體及「遠端桌面通訊協定 (RDP)」用戶端。


### 2. VMware 主機配置步驟。

若要配置虛擬主機，請完成下列步驟：

1. 從已連接網際網路的電腦啟動 RDP 用戶端，並建立與已安裝 vSphere vCenter 之相同資料中心內佈建的 {{site.data.keyword.BluVirtServers_full}} 的 RDP 階段作業。
2. 從 {{site.data.keyword.BluVirtServers_short}} 中，啟動 Web 瀏覽器，並透過 vSphere Web 用戶端連接至 VMware vCenter。
3. 從**首頁**畫面中，選取**主機及叢集**。展開左側的畫面，然後選取要用於此部署的 **VMware ESXi 伺服器**。
4. 確定在所有主機上已開啟 NFS 用戶端的防火牆埠，以便在 vSphere 主機上配置 NFS 用戶端。這會在較新版本的 vSphere 中自動開啟。若要檢查是否已開啟埠，請移至 VMware® vCenter™ 中的 **ESXi 主機管理**標籤、選取**設定**，然後選取**安全設定檔**。在**防火牆**區段中，按一下**編輯**，然後向下捲動至 **NFS 用戶端**。
5. 確定已提供**容許來自任何 IP 位址或 IP 位址清單的連線**。<br/>
   ![容許連線](/images/1_4.png)
6. 移至 **ESXi 主機管理**標籤、選取**管理**，然後選取**網路**，來配置「巨大訊框」。
7. 選取 **VMkernel 配接卡**、強調顯示 **vSwitch**，然後按一下**編輯**。（「鉛筆」圖示）
8. 選取 **NIC 設定**，然後確保 NIC MTU 已設為 9000。
   - 必要時，請將 MTU 設為 9000，然後按一下**確定**。
9. 需要時，可以驗證巨大訊框設定，如下所示：
   - 從 Windows：ping -f -l 8972 a.b.c.d
   - 從 Unix：ping -s 8972 a.b.c.d
   其中 a.b.c.d 是使用指令的鄰接 {{site.data.keyword.BluVirtServers_short}} 介面：
   顯示的輸出類似於：
   ```ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
   8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
   ```

如需 VMware 及「巨大訊框」的相關資訊，請參閱[這裡](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003712){:new_window}。


### 3. 將上行鏈路配接卡新增至虛擬交換器

1. 移至 **ESXi 主機管理**標籤，選取**管理**，然後選取**網路**，來配置新的上行鏈路配接卡。
2. 選取**實體配接卡**標籤
3. 按一下**新增主機網路**。（具有加號的「地球」圖示）
4. 選取連線類型作為**實體網路配接卡**，然後按**下一步**。
5. 選取現有 **vSwitch**，然後按**下一步**。
6. 選取**未用的配接卡**，然後按一下**新增配接卡**。（加號）
7. 按一下其他「已連接」配接卡，然後按一下**確定**。<br/>
   ![將實體配接卡新增至交換器](/images/2_3.png)
8. 按**下一步**及**完成**。
9. 導覽回**虛擬交換器**標籤，然後選取**虛擬交換器**標題下的上方**編輯設定**圖示。（「鉛筆」圖示）
10. 選取左側的 **vSwitch 小組**及失效接手項目。驗證**負載平衡**選項已設為**根據原始虛擬埠來遞送**，然後按一下**確定**。


### 4. 配置 ESXi 靜態遞送（選用）

此架構手冊的網路配置使用最少數目的埠群組。如果您已設定 NFS 儲存空間的 VMkernal 埠群組，則必須執行其他步驟。依預設，ESXi 將使用位在與 NFS 磁區相同子網路上的 vmkernel 埠，以在「耐久性/效能」儲存空間上裝載 NFS 磁區。因為我們使用第 3 層遞送來裝載 NFS 磁區，所以必須強制 ESXi 使用我們已配置的 vmkernel 埠來裝載 NFS 磁區。若要這麼做，我們必須建立一個指向「耐久性/效能」儲存空間陣列的靜態路由。

1. 若要配置靜態路由，請以 SSH 方式連接至每一台利用「效能」或「耐久性」的 ESXi 主機，並執行下列指令。請注意，您必須取得產生的 IP 位址 ping 指令（第一個指令），並將它與下面顯示的 esxcli network 指令搭配使用：
   - `ping <hostname of the endurance/performance array>`

      **附註**：NFS 儲存空間 DNS 主機名稱是獲指派多個 IP 位址的「轉遞區域 (FZ)」。這些 IP 位址是靜態的，並屬於該特定 DNS 主機名稱。您可以使用其中任何一個 IP 位址來存取特定的磁區。
   - `esxcli network ip route ipv4 add -gateway GATEWAYIP -network <result of ping command>/32`

2. 請注意，在 ESXi 5.0 及更早版本上，靜態路由在各次重新開機之間無法持續保存。為了確定任何已新增的靜態路由都能持續保存，需要將這些指令新增至每一台主機上的 local.sh 檔案，該檔案位於 /etc/rc.local.d/ 目錄中。若要這麼做，請使用視覺化編輯器開啟 local.sh 檔案，並新增上述指令，以在 exit 0 這一行上方執行。
   - 記下 IP 位址，因為它可以在下一步中用於裝載磁區。
   - 需要對計劃要裝載至 ESXi 主機的每一個 NFS 磁區執行此處理程序。
   - 以下是 VMware KB 文章的鏈結：[Configuring static routes for VMkernel ports on an ESXi host](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2001426){:new_window}。


##  在 ESXi 主機上裝載 {{site.data.keyword.filestorage_short}} 磁區

1. 按一下網頁頂端的**移至 vCenter** 圖示，然後按一下**主機及叢集**。
2. 按一下**相關物件**標籤下的**資料儲存庫**。按一下**建立新的資料儲存庫**圖示。
3. 在**新建資料儲存庫**畫面上，選取資料儲存庫的位置（您的 ESXi 主機），然後按**下一步**。
4. 在**類型**畫面上，選取 **NFS** 圓鈕，然後按**下一步**。
5. 在**名稱及配置**畫面上，輸入您想要稱呼資料儲存庫的名稱。此外，請輸入 NFS 伺服器的主機名稱。使用 NFS 伺服器的 FQDN 可產生對基礎伺服器的最佳資料流量分佈。IP 位址也有效，但不常使用，而且只有在特定實例中才會使用。請以 /foldername 形式輸入資料夾名稱。
6. 在**主機可存取性**畫面上，選取您想要裝載 NFS 資料儲存庫的所有主機，然後按**下一步**。
7. 檢閱下一個畫面上的輸入，然後按一下**完成**。
8. 針對任何其他 {{site.data.keyword.filestorage_short}} 磁區重複進行。

**附註**：{{site.data.keyword.BluSoftlayer_full}} 建議使用 FQDN 名稱來連接至資料儲存庫。使用直接 IP 定址可能會略過使用 FQDN 所提供的負載平衡機制。若要使用 IP 位址，而非 FQDN，請執行下列指令來取得 IP 位址。

  - `ping <hostname of the endurance/performance array>`
  - 從 ESXi 主機：
    ```
    ~ # vmkping nfsdal0902a-fz.service.softlayer.com
    PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
    64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
    10.2.125.80 is the IP address associated with the FQDN
    ```

## 啟用 ESXi 儲存空間 I/O 控制設定（選用）

「儲存空間 I/O 控制 (SIOC)」是一種特性，可供使用 Enterprise Plus 授權的客戶使用。在環境中啟用 SIOC 時，會變更單一 VM 的裝置佇列長度。裝置佇列長度變更，會將所有 VM 的儲存空間陣列佇列，減少為儲存空間佇列的相等共用及節流控制。只有在資源受限且儲存空間 I/O 延遲高於定義的臨界值時，才會使用 SIOC。


為了讓 SIOC 判斷儲存裝置何時壅塞或受限，需要已定義的臨界值。不同儲存空間類型的壅塞臨界值延遲會不同；預設選項是尖峰傳輸量的 90%。尖峰傳輸量值的百分比指出資料儲存庫使用這個百分比的預估尖峰傳輸量時的預估延遲臨界值。


請注意，不正確地針對資料儲存庫或 VMDK 配置 SIOC，可能會嚴重影響效能。



### 1. 資料儲存庫的儲存空間 I/O 控制

使用下列步驟，以針對「耐久性」及「效能」儲存空間使用建議值啟用 SIOC：

1. 在「vSphere Web 用戶端」導覽器中，瀏覽至資料儲存庫。
2. 按一下**管理**標籤。
3. 按一下**設定**，然後按一下**一般**。
4. 針對**資料儲存庫功能**，按一下**編輯**。
5. 選取**啟用儲存空間 I/O 控制**勾選框。<br/>
   ![NSFDataStore](/images/3_0.png)
6. 按一下**確定**。

**附註**：此設定是資料儲存庫特有的，而不是主機特有的。


### 2. {{site.data.keyword.BluVirtServers_short}} 的儲存空間 I/O 控制

您也可以使用 SIOC 限制個別 VM 的個別虛擬磁碟，或授與不同的共用給它們。限制磁碟以及授與不同的共用可讓您比對環境，並讓環境符合所獲得 {{site.data.keyword.filestorage_short}} 磁區 IOPS 數目的工作負載。限制是由 IOPS 設定，而且可以設定不同的加權或「共用」。共用設為「高」（2,000 個共用）的虛擬磁碟所收到的 I/O 是設為「正常」（1,000 個共用）的磁碟的兩倍，且是設為「低」（500 個共用）的磁碟的四倍。「正常」是所有 VM 的預設值，因此您只需要針對實際有需要的 VM 調整高於或低於「正常」的值。


請使用下列步驟來變更 vDisk 共用及限制：

1. 在 **VM 及範本**庫存中，選擇 {{site.data.keyword.BluVirtServers_short}}。圖示位於網頁的左上角。
2. 選取「I/O 控制」的 {{site.data.keyword.BluVirtServers_short}}。
3. 按一下**管理**標籤，然後按一下**設定**。按一下**編輯**。
4. 展開「硬碟」下拉箭頭。適當地修改您環境的「共用」或「限制 IOPS」。從清單中選擇虛擬硬碟，然後修改「共用」選項來選擇要配置給 {{site.data.keyword.BluVirtServers_short}} 的相對共用數量（「低」、「正常」或「高」）。您也可以按一下**自訂**，然後輸入使用者定義的共用值。
5. 按一下「限制 IOPS」直欄，然後輸入要配置給虛擬機器的儲存空間資源上限。
6. 按一下**確定**


   **附註**：即使未啟用 SIOC 時，也會使用上述處理程序來設定 {{site.data.keyword.BluVirtServers_short}} 中個別 vDisk 的資源耗用限制。這些設定專用於個別訪客，而不是主機（雖然是由 SIOC 使用它們）。





## ESXi 主機端設定

設定 NFS 儲存空間的 ESXi 5.x 主機需要一些其他設定。

|參數      | 設為   ... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	若為 vSphere 5.0/5.1，設為 128 <br/> 若為 vSphere 5.5 或更新版本，設為 512 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256（僅限 vSphere 6.0 或更新版本）|
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


- 在 ESXi 5.x 主機上使用 CLI 作為進階配置參數：下列範例使用 CLI 來設定這些進階配置參數，然後檢查它們。在 ESXi 5.x 主機的 /usr/sbin 目錄中，可以找到範例中所使用的 esxcfg-advcfg 工具。

   - 從 ESXi 5.x CLI 設定進階配置參數：
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

- 從 ESXi 5.x CLI 檢查進階配置參數：
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


## 在 SoftLayer 中啟用巨大訊框 - Windows 及 Linux

{{site.data.keyword.BluSoftlayer_full}} 表示，為了能夠完整實現儲存空間上的速度，需要以 9,000 個 MTU 啟用「巨大訊框」。


巨大訊框是有效負載大於標準最大傳輸單位 (MTU) 1,500 位元組的乙太網路訊框。巨大訊框用於支援最少 1 Gbps 的區域網路，且最大可達到 9,000 個位元組。


必須在來源裝置 <-> 交換器 <-> 路由器 <-> 交換器 <-> 目的地裝置中的整個網路路徑上，配置相同的「巨大訊框」。如果整個鏈結未設為相同，則會預設為鏈結的最低設定。SoftLayer 目前將其網路裝置設為 9,000。所有客戶裝置都需要設為相同。

### Windows

1. 開啟**網路及共用中心**。
2. 按一下**變更介面卡設定**。
3. 用滑鼠右鍵按一下您要啟用巨大訊框的 NIC，然後選取**內容**。
4. 在**網路**標籤下，按一下網路配接卡的**設定**。
5. 選取**進階**標籤。
6. 選取**巨大訊框**，然後根據 NIC，將值從 disabled 變更為所需的值（例如 9kB MTU 或 9,014 個位元組）。
7. 按一下所有對話框的**確定**。

請注意，當您進行變更時，NIC 的網路連線功能會中斷幾秒鐘。您也應該重新開機，以確保變更生效。



### LINUX

1. 編輯 eth0 介面的網路配置檔，例如，/etc/sysconfig/network-script/ifcfg-eth0 (CentOS/RHEL/Fedora Linux)：
`# vi /etc/sysconfig/network-script/ifcfg-eth0`

2. 附加下列配置指引，以指定訊框大小（以位元組為單位）：MTU 9000

3. 關閉並儲存檔案。重新啟動介面 eth0：
`# /etc/init.d/networking restart (This will cause a brief loss of network connectivity)`


Debian/Ubuntu Linux 使用者的附註：
Debian/Ubuntu Linux 使用者應該將 MTU=9000 新增至 /etc/network/interfaces 配置檔。

在[這裡](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}進一步瞭解「進階單一網站 VMware 參照架構」。
