---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 開始使用 {{site.data.keyword.filestorage_short}}

{{site.data.keyword.filestorage_full}} 是持續性、快速且具彈性的網路連結、NFS 型 {{site.data.keyword.filestorage_short}}。在這個網路連接儲存空間 (NAS) 環境中，您可以完全控制檔案共用功能及效能。{{site.data.keyword.filestorage_short}} 共用可以透過路由 TCP/IP 連線最多連接 64 台授權裝置，來取得備援。

{{site.data.keyword.filestorage_short}} 具備無與倫比的特性集，提供同級最佳的延續性及可用性層次。{{site.data.keyword.filestorage_short}} 是使用業界標準及最佳作法所建置，其設計旨在保護資料完整性。{{site.data.keyword.filestorage_short}} 透過維護事件及非計劃性故障來維護可用性，同時提供一致的效能基準線。

請充分運用下列 {{site.data.keyword.filestorage_short}} 核心特性：

- **一致效能基準線**
   - 透過將通訊協定層次的每秒輸入/輸出作業 (IOPS) 配置給個別磁區來提供。
- **{{site.data.keyword.filestorage_short}}**
   - 可用於檔案型 NFS 共用。
- **高度可延續且具復原力**
   - 保護資料完整性，並透過維護事件及非計劃性故障來維護可用性，而不需要建立及管理作業系統層次的獨立磁碟備用陣列 (RAID) 陣列
- **靜態資料加密**[（適用於精選資料中心。）](new-ibm-block-and-file-storage-location-and-features.html)
   - 提供靜態資料的提供者管理加密，不需額外付費
- **全快閃記憶體支援的儲存空間**[（適用於精選資料中心。）](new-ibm-block-and-file-storage-location-and-features.html)
   - 磁區的全快閃記憶體儲存空間可以佈建於 2 IOPS/GB 或更高層次。
- **Snapshot**[（適用於精選資料中心。）](new-ibm-block-and-file-storage-location-and-features.html).
   - 以不中斷的方式擷取時間點資料 Snapshot。
- **抄寫**[（適用於精選資料中心。）](new-ibm-block-and-file-storage-location-and-features.html)
   - 適用於將儲存空間佈建於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)時。
   - 自動將 Snapshot 複製到夥伴 {{site.data.keyword.BluSoftlayer_full}} 資料中心。
- **高度可用的連線功能**
   - 使用備用網路連線以讓可用性最大化。
   - NFS 型 {{site.data.keyword.filestorage_short}} 已遞送的 TCP/IP 連線。
- **並行存取**
   - 容許多台主機（最多 64 台）同時存取檔案磁區。
- **叢集資料庫**
   - 支援進階使用案例（例如叢集資料庫）。

## 計費

您可以為「檔案」磁區選取按小時或按月計費。為 LUN 選取的計費類型會套用至其 Snapshot 空間及抄本。例如，如果您佈建按小時計費的 LUN，則任何 Snapshot 或抄本費用都會按小時計費。如果您佈建按月計費的 LUN，則任何 Snapshot 或抄本費用都會按月計費。

使用**按小時計費**，會在刪除 LUN 或計費週期結束時（看何者為先），計算「檔案」磁區存在於帳戶上的小時數。如果儲存空間使用期間為期只有幾天或不到一整個月，則按小時計費是一個不錯的選擇。按小時計費只適用於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的儲存空間。

使用**按月計費**，價格是從建立日期到計費週期結束為止，按比例計算，並立即計費。如果在計費週期結束之前刪除磁區，則不會退款。如果儲存空間用於正式作業工作負載，而正式作業工作負載使用需要長期（一個月或更久）儲存及存取的資料，則按月計費是一個不錯的選擇。


**效能**
<table>
  <caption>表 1 顯示「效能儲存空間」的價格（按月及按小時計費）。</caption>
  <tr>
   <th>每月價格</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>每小時價格</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
</table>

**耐久性**
<table>
  <caption>表 2 顯示具有按月及按小時計費選項之每個層級的「耐久性儲存空間」價格。</caption>
  <tr>
   <th>IOPS 層級</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>每月價格</th>
   <td>$0.06/GB</td>
   <td>$0.15/GB</td>
   <td>$0.20/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>每小時價格</th>
   <td>$0.0001/GB</td>
   <td>$0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0009/GB</td>
  </tr>
</table>



## 佈建

您可以使用以下兩個選項，來佈建 20 GB 到 12 TB 的 {{site.data.keyword.filestorage_short}} 磁區：<br/>
- 佈建**耐久性**層級，其特色是預先定義的效能層次，以及 Snapshot 及抄寫這類其他特性。
- 建置具有已配置每秒輸入/輸出作業 (IOPS) 的高功率**效能**環境。


### 使用耐久性層級佈建

「耐久性」{{site.data.keyword.filestorage_short}} 提供於四種 IOPS 效能層級，以支援各種應用程式需求。<br />

- **每 GB 0.25 IOPS** 是為了低 I/O 強度的工作負載而設計。這些工作負載的特點通常是隨時都有大比例的非作用中資料。應用程式範例包括儲存信箱或部門層次的檔案共用。

- **每 GB 2 IOPS** 是為了大多數通用用途而設計。應用程式範例包括管理小型資料庫，其支持 Web 應用程式或是 Hypervisor 的 VM 磁碟映像檔。

- **每 GB 4 IOPS** 是為了較高強度工作負載而設計。這些工作負載的特點通常是隨時都有大比例的作用中資料。應用程式範例包括交易式資料庫及其他對於效能敏感的資料庫。

- **每 GB 10 IOPS** 是為了最嚴苛的工作負載（例如 NoSQL 資料庫所建立的工作負載）以及進行分析用的資料處理而設計。此層級只適用於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內最多佈建 4 TB 的儲存空間。

12 TB 耐久性磁區最多提供 48,000 IOPS。

為您的工作負載選擇正確「耐久性」層級固然重要。使用達到最高效能所需的正確區塊大小、乙太網路連線速度及主機數目也同樣重要。如果這其中有任何部分與其他部分不一致，則會對產生的傳輸量有重大的影響。

### 使用效能佈建

「效能」是一種 {{site.data.keyword.filestorage_short}} 類別，其設計旨在支援高 I/O 應用程式，而這些應用程式具有瞭解且不適合「耐久性」層級的效能需求。透過將通訊協定層次 IOPS 配置到個別磁區，即可達成可預測效能。佈建範圍從 20 GB 到 12 TB 的儲存空間大小時，可以使用各種 IOPS 速率 (100 - 48,000)。

{{site.data.keyword.filestorage_short}} 的效能會透過「網路檔案系統 (NFS)」連線存取及裝載。{{site.data.keyword.filestorage_short}} 通常用於多台伺服器同時存取磁區的情況。「一致效能」磁區可以根據表 1 的「大小」及 IOPS 來進行訂購，而且可以與 Linux 作業系統搭配使用。

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>表 3 顯示「效能」儲存空間的大小與 IOPS 組合。<br/><sup><img src="/images/numberone.png" alt="註腳" /></sup> 精選資料中心內提供大於 6,000 的「IOPS 限制」。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>大小 (GB)</th>
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
            <td>6,000 或 10,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 或 20,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>2,000-3,000</td>
            <td>200</td>
            <td>6,000 或 40,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>4,000-7,000</td>
            <td>300</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
</table>


效能磁區的設計旨在使操作持續接近已佈建的 IOPS 層次。一致性可讓您更輕鬆地為具有特定效能層次的應用程式環境調整大小與進行擴充。此外，可以透過建置具有理想價格與效能比的磁區，來進行環境的最佳化。

### 佈建考量

**區塊大小**

「耐久性」及「效能」的 IOPS 都是根據 16 KB 區塊大小（具有 50/50 讀寫 50% 隨機工作負載）。16 KB 區塊相當於寫入一次磁區。
{:important}

應用程式所使用的區塊大小會直接影響儲存空間效能。如果應用程式所使用的區塊大小小於 16 KB，則 IOPS 限制會比傳輸量限制更早實現。反之，如果應用程式所使用的區塊大小大於 16 KB，則傳輸量限制會比 IOPS 限制更早實現。

<table>
  <caption>表 4 顯示區塊大小及 IOPS 如何影響傳輸量的範例。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>區塊大小 (KB)</th>
            <th>IOPS</th>
            <th>傳輸量（MB/秒）</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4（一般用於 Linux）</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8（一般用於 Oracle）</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32（一般用於 SQL Server）</td>
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

**授權主機**

另一個要考量的因素是使用磁區的主機數目。如果有單一主機正在存取該磁區，可能難以實現最大可用 IOPS，特別是極端的 IOPS 計數 (10,000)。如果您的工作負載需要高傳輸量，則最好至少將一對伺服器配置為存取您的磁區，以避免單一伺服器瓶頸。

**網路連線**

乙太網路連線的速度必須比來自您磁區的預期最大傳輸量更快。一般而言，請不要預期乙太網路連線飽和度超過可用頻寬的 70%。例如，如果您有 6,000 IOPS 而且要使用 16 KB 區塊大小，則磁區能處理大約 94 MBps 的傳輸量。如果您的 LUN 有一條 1 Gbps 乙太網路連線，則當伺服器嘗試使用最大可用傳輸量時，它會變成瓶頸。原因是 1 Gbps 乙太網路連線理論限制（每秒 125 MB）的 70% 只容許每秒 88 MB。

為達到最大 IOPS，需要有足夠的網路資源。其他考量包括儲存空間及主機端之外的專用網路使用情形，以及應用程式特定的調整（IP 堆疊或[佇列深度](set-host-queue-depth-settings-performance-and-endurance-storage.html)，以及其他設定）。

儲存空間資料流量包含在「公用虛擬伺服器」的網路總用量中。若要進一步瞭解該服務可能強制的限制，請參閱[虛擬伺服器文件](https://{DomainName}/docs/vsi/vsi_public.html#public-virtual-servers)。


**NFS 版本**

在 {{site.data.keyword.BluSoftlayer_full}} 環境中，同時支援 NFS 第 3 版及 NFS 4.1 版。不過，偏好使用 NFS 第 3 版，因為 NFS 4.1 版是有狀態的通訊協定（不像 NFSv3 是無狀態的通訊協定），而且在網路事件期間可能會發生通訊協定問題。NFS 4.1 版必須靜止所有作業，然後完成鎖定收回。在相當忙碌的 NFS 檔案伺服器上，延遲增加可能會造成中斷。缺少 NFS 4.1 版多路徑和幹線也可能會延長 NFS 作業回復時間。

## 提交訂單

當您準備好提交訂單時，可以透過[主控台](provisioning-block_storage.html)或 [SLCLI](ordering-through-cli.html) 下訂單。若要使用 VMware 佈建檔案儲存空間，請按一下[這裡](architecture-guide-file-storage-vmware.html)

## 連接新的儲存空間

當您的佈建要求完成時，請授權主機存取新的儲存空間，並配置連線。請根據主機的作業系統，遵循適當的鏈結。
- [存取 Linux 上的 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html)
- [在 CentOS 中裝載 {{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html)
- [在 CoreOS 上裝載 {{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html)
- [配置 {{site.data.keyword.filestorage_short}} 以便使用 cPanel 進行備份](configure-backup-cpanel.html)
- [配置 {{site.data.keyword.filestorage_short}} 以便使用 Plesk 進行備份](configure-backup-plesk.html)
- [在 ESXi 主機上裝載 {{site.data.keyword.filestorage_short}} 磁區](architecture-guide-file-storage-vmware.html)
