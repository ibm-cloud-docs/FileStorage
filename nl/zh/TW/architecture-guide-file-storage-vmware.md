---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

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


# 使用 VMware 佈建 {{site.data.keyword.filestorage_short}}
{: #architectureguide}

以下是在 {{site.data.keyword.cloud}} 為 vSphere 5.5 及 vSphere 6.0 環境訂購及配置 {{site.data.keyword.filestorage_full}} 的步驟。

{{site.data.keyword.filestorage_short}} 的設計旨在支援需要可預測效能層次的高 I/O 應用程式。透過將通訊協定層次的每秒輸入/輸出作業數 (IOPS) 配置給個別磁區，即可達成可預測效能。

如果您需要超過 8 台主機來存取 VMWare 資料儲存庫，則選擇 NFS {{site.data.keyword.filestorage_short}} 是最佳作法。
{:tip}

{{site.data.keyword.filestorage_short}} 供應項目是透過 NFS 連線進行存取及裝載。在 VMware 部署中，單一磁區最多可以裝載至 64 台 ESXi 主機作為共用儲存空間。您也可以裝載多個磁區來建立儲存空間叢集，以使用 vSphere Storage Distributed Resource Scheduler (DRS)。

{{site.data.keyword.filestorage_short}} 的定價及配置選項是根據保留空間與所提供 IOPS 的組合進行收費。

## 訂購考量

訂購 {{site.data.keyword.filestorage_short}} 時，請考量下列資訊：

- 當您決定大小時，請考量工作負載的大小以及所需的傳輸量。大小與「耐久性」服務有重大關係，「耐久性」服務會根據容量 (IOPS/GB) 以線性方式擴充效能。反之，「效能」服務可讓管理者獨立選擇容量和效能。傳輸量需求與「效能」有重大關係。

  傳輸量計算方式為 IOPS x 16 KB。IOPS 的測量基礎是 16 KB 區塊大小（具有 50/50 讀寫混合）。<br/>增加區塊大小會增加傳輸量，但會減少 IOPS。例如，將區塊大小加倍為 32 KB 區塊可維持最大傳輸量，但會讓 IOPS 減半。
{:note}

- NFS 使用許多額外的檔案控制作業（例如 `lookup`、`getattr` 及 `readdir`）。除了讀/寫作業之外，這些作業也可以計算為 IOPS，而且會依作業類型及 NFS 版本而不同。
- {{site.data.keyword.filestorage_short}} 磁區會向授權裝置、子網路或 IP 位址公開。
- 為避免在路徑失效接手期間發生儲存空間斷線，{{site.data.keyword.IBM}} 建議安裝 VMware 工具，以設定適當的逾時值。不需要變更值，預設值就足以確保 VMware 主機不會中斷連線。
- 在 {{site.data.keyword.cloud}} 環境中，同時支援 NFS 第 3 版及 NFS 4.1 版。不過，{{site.data.keyword.IBM}} 建議您使用 NFS 第 3 版。因為 NFS 4.1 版是有狀態的通訊協定（不像 NFS 第 3 版是無狀態的通訊協定），所以在網路事件期間可能會發生通訊協定問題。NFS 4.1 版必須靜止所有作業，然後完成鎖定收回。進行這些作業時，可能會發生中斷。

如需相關資訊，請參閱 VMware 的白皮書：[Best Practices for running VMware vSphere on network-attached storage](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){: external}{:tip}

### NFS 通訊協定 VMware 特性支援矩陣

|vSphere 特性|NFS 第 3 版|NFS 4.1 版|
|-----|-----|-----|
|vMotion 及 Storage vMotion|是|是|
|High Availability (HA)|是|是|
|Fault Tolerance (FT)|是|是|
|Distributed Resource Scheduler (DRS)| 是< |是|
|Host Profiles|是|是|
|Storage DRS|是|否|
|Storage I/O Control|是|否|
|Site Recovery Manager|是|否|
|Virtual Volumes|是|否|
{: row-headers}
{: class="comparison-table"}
{: caption="表 1 - NFS 通訊協定 VMware 特性支援矩陣" caption-side="top"}
{: summary="這個表格有列和欄標頭。列標頭識別 vSphere 特性。欄標頭識別 NSF 版本。若要查看特性是否已啟用，請導覽至特性的列，然後查看與您使用之 NFS 版本相關聯的欄。"}

*來源 - [VMware - NFS 通訊協定及 ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){: external}*



### 使用 Snapshot

{{site.data.keyword.filestorage_short}} 容許管理者設定 Snapshot 排程，以自動建立及刪除每個儲存空間磁區的 Snapshot 副本。他們也可以建立額外的 Snapshot 排程（每小時、每日、每週）來自動擷取 Snapshot，以及針對企業永續及災難回復 (BCDR) 情境手動建立特定 Snapshot。已保留 Snapshot 及使用空間，會透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} 將自動警示傳遞給磁區擁有者。

需要有「Snapshot 空間」，才能使用 Snapshot。您可以在初次訂購磁區時購買空間，或在初次佈建之後，透過**磁區詳細資料**頁面購買空間，方法是按一下**動作**然後選取**新增 Snapshot 空間**。

請務必注意，VMware 環境並不知道 Snapshot。{{site.data.keyword.filestorage_short}} Snapshot 功能不得與 VMware Snapshot 混淆。必須從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} 處理任何使用 {{site.data.keyword.filestorage_short}} Snapshot 特性的回復。

還原 {{site.data.keyword.filestorage_short}} 磁區時，需要關閉 {{site.data.keyword.filestorage_short}} 上所有 VM 的電源。磁區必須暫時從 ESXi 主機卸載，以避免在處理程序期間發生任何資料損毀。

如需相關資訊，請參閱 [Snapshot](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots) 文章。


### 使用抄寫

抄寫使用您的其中一個 Snapshot 排程，自動將 Snapshot 複製到遠端資料中心內的目的地磁區。如果發生災難性事件或資料毀損，則可以在遠端網站中回復副本。

您可以使用抄本來進行下列作業

- 透過失效接手至目的地磁區，快速從網站故障及其他災難回復
- 失效接手至 DR 副本中的特定時間點

抄寫是將資料同步保留在兩個不同位置。如果您想要複製您的磁區，並與原始磁區分開使用，請參閱[建立重複的檔案磁區](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)。
{:tip}

您必須先建立 Snapshot 排程，才能進行抄寫。

當您失效接手時，您會將「開關」從主要資料中心的儲存空間磁區快速切換到遠端資料中心的目的地磁區。例如，您的主要資料中心是在「倫敦」，而次要資料中心是在「阿姆斯特丹」。如果發生故障事件，您會失效接手至「阿姆斯特丹」。失效接手的意思是從「阿姆斯特丹」的 vSphere Cluster 實例連接至現行主要磁區。修復「倫敦」中的磁區之後，會擷取「阿姆斯特丹」磁區的 Snapshot。然後，您可以從「倫敦」的運算實例失效回復至「倫敦」及那個再度成為主要磁區的磁區。

在磁區撤回至主要資料中心之前，需要先停止在遠端網站的使用。會先建立任何新資訊或已變更資訊的 Snapshot，並抄寫至主要資料中心，然後才能在正式作業網站 ESXi 主機上重新進行裝載。

如需配置抄本的相關資訊，請參閱[抄寫](/docs/infrastructure/FileStorage?topic=FileStorage-replication)。

會根據 Snapshot 排程及 Snapshot 保留，來抄寫無效的資料（不論是毀損、受到駭客入侵還是感染病毒）。使用最小的抄寫時間範圍可以提供更好的回復點目標。不過，它也提供較少的時間來對無效資料的抄寫做出反應。
{:note}


## 訂購 {{site.data.keyword.filestorage_short}}

使用[進階單一 VMware 參照架構](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){: external}，在 VMWare 環境中設定採用「耐久性」或「效能」選項的 {{site.data.keyword.filestorage_short}}。

{{site.data.keyword.filestorage_short}} 可透過 [IBM Cloud 型錄](https://{DomainName}/catalog){: external}或 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}訂購。如需相關資訊，請參閱[訂購 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)

在一分鐘以內即會佈建儲存空間，並且它會顯示在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} 的 **{{site.data.keyword.filestorage_short}}** 頁面上。

佈建磁區後，必須授權將使用磁區的 {{site.data.keyword.BluBareMetServers_full}} 或 {{site.data.keyword.BluVirtServers_full}} 來存取儲存空間。請使用下列步驟來授權磁區。

1. 按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 在**耐久性**或**效能磁區動作**功能表上，選取**存取主機**。
3. 按一下**子網路**。
4. 從指派給 ESXi 主機上 VMkernel 埠的可用子網路清單中進行選擇，然後按一下**提交**。<br/>

   顯示的子網路就是與儲存空間磁區位於相同資料中心內的已訂閱子網路。
   {:note}

授權子網路之後，請記下您在裝載磁區時想要使用之「耐久性」或「效能」儲存空間伺服器的主機名稱。按一下特定磁區，即可在 {{site.data.keyword.filestorage_short}} 詳細資料頁面上找到這項資訊。


##  配置 VMware 虛擬機器主機

開始 VMware 配置處理程序之前，請確定符合下列必要條件：

- 具有 VMware ESXi 的 {{site.data.keyword.BluBareMetServers}} 是使用適當的儲存空間配置及 ESXi 登入認證所佈建。
- {{site.data.keyword.cloud}} Windows 實體或 {{site.data.keyword.virtualmachinesshort}}，位於與 {{site.data.keyword.BluBareMetServers}} 相同的資料中心內。包括 {{site.data.keyword.cloud}} Windows VM 的「公用 IP 位址」及登入認證。
- 具有網際網路存取的電腦，並且已安裝 Web 瀏覽器軟體及「遠端桌面通訊協定 (RDP)」用戶端。


### 1. 配置「VMware 主機」。

1. 從已連接網際網路的電腦啟動 RDP 用戶端，並建立與已安裝 vSphere vCenter 之相同資料中心內佈建的 {{site.data.keyword.BluVirtServers_full}} 的 RDP 階段作業。
2. 從 {{site.data.keyword.BluVirtServers_short}} 中，啟動 Web 瀏覽器，並透過 vSphere Web Client 連接至 VMware vCenter。
3. 從**首頁**畫面中，選取**主機及叢集**。展開左側的窗格，然後選取要用於此部署的 **VMware ESXi 伺服器**。
4. 確定在所有主機上已開啟 NFS 用戶端的防火牆埠，才能在 vSphere 主機上配置 NFS 用戶端。（此埠會自動在較新版本的 vSphere 中開啟。）若要檢查是否已開啟埠，請移至 VMware® vCenter™ 中的 **ESXi 主機管理**標籤、選取**設定**，然後選取**安全設定檔**。在**防火牆**區段中，按一下**編輯**，然後向下捲動至 **NFS 用戶端**。
5. 確定已提供**容許來自任何 IP 位址或 IP 位址清單的連線**。<br/>
   ![容許連線](/images/1_4.png)
6. 移至 **ESXi 主機管理**標籤、選取**管理**，然後選取**網路**，來配置「巨大訊框」。
7. 選取 **VMkernel 配接卡**、強調顯示 **vSwitch**，然後按一下**編輯**（鉛筆圖示）。
8. 選取 **NIC 設定**，然後確定 NIC MTU 已設為 9000。
9. **選用**。驗證巨大訊框設定。
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

     值 a.b.c.d 是鄰接的 {{site.data.keyword.BluVirtServers_short}} 介面。

     範例
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

如需 VMware 及「巨大訊框」的相關資訊，請參閱[這裡](https://kb.vmware.com/s/article/1003712){: external}。
{:tip}


### 2. 將上行鏈路配接卡新增至虛擬交換器

1. 移至 **ESXi 主機管理**標籤，選取**管理**，然後選取**網路**，來配置新的上行鏈路配接卡。
2. 選取**實體配接卡**標籤
3. 按一下**新增主機網路**（具有加號的「地球」圖示）。
4. 選取連線類型作為**實體網路配接卡**，然後按**下一步**。
5. 選取現有 **vSwitch**，然後按**下一步**。
6. 選取**未用的配接卡**，然後按一下**新增配接卡**（加號）。
7. 按一下其他「已連接」配接卡，然後按一下**確定**。<br/>
   ![將實體配接卡新增至交換器](/images/2_3.png)
8. 按**下一步**及**完成**。
9. 回到**虛擬交換器**標籤，然後選取**虛擬交換器**標題下的**編輯設定**（鉛筆圖示）。
10. 在左側，選取 vSwitch **小組及失效接手**項目。
11. 驗證**負載平衡**選項已設為**根據原始虛擬埠來遞送**，然後按一下**確定**。


### 3. 配置靜態遞送（選用）

此架構手冊的網路配置使用最少數目的埠群組。如果您有 NFS 儲存空間的 VMkernel 埠群組，則必須採取額外的步驟。依預設，ESXi 會使用位在與 NFS 磁區相同子網路上的 VMkernel 埠來裝載 NFS 磁區。因為使用第 3 層遞送來裝載 NFS 磁區，所以必須強制 ESXi 使用已配置成裝載 NFS 磁區的 VMkernel 埠。為了使用正確的埠，必須建立一個指向儲存空間陣列的靜態路由。

1. 若要配置靜態路由，請以 SSH 方式連接至每台利用「效能」或「耐久性」儲存空間的 ESXi 主機，並執行下列指令。請記下 `ping` 指令（第一個指令）所產生的 IP 位址，並將它與 `esxcli` network 指令搭配使用。
   ```
   ping <host name of the storage array>
   ```
   {: pre}

   NFS 儲存空間 DNS 主機名稱是獲指派多個 IP 位址的「轉遞區域 (FZ)」。這些 IP 位址是靜態的，並屬於該特定 DNS 主機名稱。您可以使用其中任何一個 IP 位址來存取特定的磁區。
{:note}

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. 在 ESXi 5.0 及更早版本上，靜態路由在各次重新啟動之間無法持續保存。為了確定任何已新增的靜態路由都能持續保存，需要將此指令新增至每台主機上的 `local.sh` 檔案，該檔案位於 `/etc/rc.local.d/` 目錄中。請使用視覺化編輯器開啟 `local.sh` 檔案，並且在步驟 4.1 中新增第二個指令。位置是在 `exit 0` 這一行前面。

記下 IP 位址，因為它可以在下一步中用於裝載磁區。<br/>需要對計劃要裝載至 ESXi 主機的每一個 NFS 磁區執行此處理程序。<br/>如需相關資訊，請參閱 VMware KB 文章：[在 ESXi 主機上配置 VMkernel 埠的靜態路由](https://kb.vmware.com/s/article/2001426){: external}。{:tip}


##  在 ESXi 主機上建立及裝載 {{site.data.keyword.filestorage_short}} 磁區

1. 按一下**移至 vCenter** 圖示，然後按一下**主機及叢集**。
2. 在**相關物件**標籤上，按一下**資料儲存庫**。
3. 按一下**建立新的資料儲存庫**圖示。
4. 在**新建資料儲存庫**畫面上，選取 VMware 資料儲存庫的位置，然後按**下一步**。
5. 在**類型**畫面上，選取 **NFS**，然後按**下一步**。
6. 然後，選取 NFS 版本。NFS 第 3 版及 NFS 4.1 版都受到支援，但建議 NFS 第 3 版。

   請確定只使用一種 NFS 版本來存取資料儲存庫。使用不同版本將一個以上的主機裝載到相同的資料儲存庫，結果可能導致資料毀損。
   {:important}

7. 在**名稱及配置**畫面上，輸入您要稱呼 VMware 資料儲存庫的名稱。此外，請輸入 NFS 伺服器的主機名稱。使用 NFS 伺服器的 FQDN 可產生對基礎伺服器的最佳資料流量分佈。IP 位址也有效，但不常使用，而且只有在特定實例中才會使用。請以 `/foldername` 形式輸入資料夾名稱。
8. 在**主機可存取性**畫面上，選取您要裝載 NFS VMware 資料儲存庫的一或多台主機，然後按**下一步**。
9. 檢閱下一個畫面上的輸入，然後按一下**完成**。
10. 針對任何其他 {{site.data.keyword.filestorage_short}} 磁區重複進行。

{{site.data.keyword.cloud}} 建議使用 FQDN 名稱來連接至 VMware 資料儲存庫。使用直接 IP 定址可能會略過使用 FQDN 所提供的負載平衡機制。
{:important}

若要使用 IP 位址，而非 FQDN，只要 ping 伺服器即可取得 IP 位址。
```
ping <host name of the storage array>
```
{: pre}

若要從 ESXi 主機取得 IP 位址，請使用下列指令。產生的 10.2.125.80 就是具有 FQDN 的 IP。
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## 啟用 ESXi Storage I/O Control（選用）

Storage I/O Control (SIOC) 是一種特性，可供使用 Enterprise Plus 授權的客戶使用。在環境中啟用 SIOC 時，會變更單一 VM 的裝置佇列長度。裝置佇列長度變更，會將所有 VM 的儲存空間陣列佇列減少為相等共用。只有在資源受限且儲存空間 I/O 延遲超過定義的臨界值時，才會使用 SIOC。


為了讓 SIOC 判斷儲存裝置何時壅塞或受限，需要已定義的臨界值。不同儲存空間類型的壅塞臨界值延遲會不同。預設選項是尖峰傳輸量的 90%。尖峰傳輸量值的百分比指出 VMware 資料儲存庫使用這個百分比的預估尖峰傳輸量時的預估延遲臨界值。


為 VMware 資料儲存庫或 VMDK 配置 SIOC 的方式若不正確，可能會嚴重影響效能。
{:important}


### 為 VMWare 資料儲存庫配置 Storage I/O Control

1. 在 vSphere Web Client 導覽器中，瀏覽至 VMWare 資料儲存庫。
2. 按一下**管理**標籤。
3. 按一下**設定**，然後按一下**一般**。
4. 針對**資料儲存庫功能**，按一下**編輯**。
5. 選取**啟用 Storage I/O Control** 勾選框。<br/>
   ![NSF VMware 資料儲存庫](/images/3_0.png)
6. 按一下**確定**。

此設定是 VMware 資料儲存庫特有的，而不是主機特有的。
{:note}


### 為 {{site.data.keyword.BluVirtServers_short}} 配置 Storage I/O Control

您也可以使用 SIOC 限制個別 VM 的個別虛擬磁碟，或授與不同的共用給它們。藉由限制磁碟並授與不同的共用，可讓您配合所獲得 {{site.data.keyword.filestorage_short}} 磁區 IOPS 數目的工作負載來調整環境。限制是由 IOPS 設定，而且可以設定不同的加權或「共用」。共用設為「高」（2,000 個共用）的虛擬磁碟所收到的 I/O 是設為「正常」（1,000 個共用）的磁碟的兩倍，且是設為「低」（500 個共用）的磁碟的四倍。「正常」是所有 VM 的預設值，因此您必須針對有需要的 VM 將該值調整為「高」或「低」。

請使用下列步驟來變更 VDisk 共用及限制。

1. 在 **VM 及範本**庫存中，選擇 {{site.data.keyword.BluVirtServers_short}}。
2. 選取「I/O 控制」的 {{site.data.keyword.BluVirtServers_short}}。
3. 按一下**管理**標籤，然後按一下**設定**。按一下**編輯**。
4. 展開**硬碟**箭頭。選取適合您環境的**修改共用**或**限制 IOPS**。從清單中選擇虛擬硬碟，然後修改「共用」選項來選擇要配置給 {{site.data.keyword.BluVirtServers_short}} 的相對共用數目（「低」、「正常」或「高」）。您也可以按一下**自訂**，然後輸入使用者定義的共用值。
5. 按一下**限制 IOPS** 直欄，然後輸入要配置給虛擬機器的儲存空間資源上限。
6. 按一下**確定**。


此處理程序是用來設定 {{site.data.keyword.BluVirtServers_short}} 中個別 vDisk 的資源耗用限制（即使未啟用 SIOC）。這些設定專用於個別訪客，而不是主機（雖然是由 SIOC 使用它們）。
{:important}


## 配置 ESXi 主機端設定

針對 NFS 儲存空間配置 ESXi 5.x 主機時，需要額外設定。此表格顯示需要的每個設定。

|參數      |設為   ... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	若為 vSphere 5.0/5.1，設為 128 <br/> 若為 vSphere 5.5 或更新版本，設為 512 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256（僅限 vSphere 6.0 或更新版本）|
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |
{: caption="表 2 - 主機端設定" caption-side="top"}

### 使用 CLI 來更新 ESXi 5.x 主機上的進階配置參數

下列範例使用 CLI 來設定進階配置參數，然後檢查它們。在 ESXi 5.x 主機的 `/usr/sbin` 目錄中，可以找到範例中所使用的 `esxcfg-advcfg` 工具。

   - 從 ESXi 5.x CLI 設定進階配置參數。

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

  - 從 ESXi 5.x CLI 檢查進階配置參數。

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
在[這裡](/docs/infrastructure/virtualization?topic=Virtualization-advanced-single-site-vmware-reference-architecture){: external}進一步瞭解「進階單一網站 VMware 參照架構」。
{:tip}
