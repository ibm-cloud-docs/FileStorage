---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-02"

keywords: File Storage, file storage, NFS, provisioning, setup, configuration, mounting storage

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# 入門指導教學
{: #getting-started}

{{site.data.keyword.filestorage_full}} 是持續性、快速且具彈性的網路連結、NFS 型 {{site.data.keyword.filestorage_short}}。在這個網路連接儲存空間 (NAS) 環境中，您可以完全控制檔案共用功能及效能。{{site.data.keyword.filestorage_short}} 共用可以透過路由 TCP/IP 連線最多連接 64 台授權裝置，來取得備援。{:shortdesc}

## 開始之前
{: #prereqs}

您可以使用以下兩個選項，來佈建 20 GB 到 12 TB 的 {{site.data.keyword.filestorage_short}} 磁區：<br/>
- 佈建**耐久性**層級，其特色是預先定義的效能層次，以及 Snapshot 及抄寫這類其他特性。
- 建置具有已配置每秒輸入/輸出作業 (IOPS) 的高功率**效能**環境。

如需 {{site.data.keyword.filestorage_short}} 供應項目的相關資訊，請參閱[關於 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-about)

## 佈建考量

### 區塊大小

「耐久性」及「效能」的 IOPS 都是根據 16 KB 區塊大小（具有 50/50 讀寫 50/50 隨機/循序工作負載）。16 KB 區塊相當於寫入一次磁區。
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
          <tr>
            <td>1024</td>
            <td>16</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

### 授權主機

另一個要考量的因素是使用磁區的主機數目。如果有單一主機正在存取該磁區，可能難以實現最大可用 IOPS，特別是極端的 IOPS 計數 (10,000)。如果您的工作負載需要高傳輸量，則最好至少將一對伺服器配置為存取您的磁區，以避免單一伺服器瓶頸。

### 網路連線

乙太網路連線的速度必須比來自您磁區的預期最大傳輸量更快。一般而言，請不要預期乙太網路連線飽和度超過可用頻寬的 70%。例如，如果您有 6,000 IOPS 而且要使用 16 KB 區塊大小，則磁區能處理大約 94 MBps 的傳輸量。如果您的 LUN 有一條 1 Gbps 乙太網路連線，則當伺服器嘗試使用最大可用傳輸量時，它會變成瓶頸。原因是 1 Gbps 乙太網路連線理論限制（每秒 125 MB）的 70% 只容許每秒 88 MB。

為達到最大 IOPS，需要有足夠的網路資源。其他考量包括儲存空間及主機端之外的專用網路使用情形，以及應用程式特定的調整（IP 堆疊或[佇列深度](/docs/infrastructure/FileStorage?topic=FileStorage-hostqueuesettings)，以及其他設定）。

儲存空間資料流量包含在「公用虛擬伺服器」的網路總用量中。若要進一步瞭解該服務可能強制的限制，請參閱[虛擬伺服器文件](/docs/vsi?topic=virtual-servers-about-public-virtual-servers)。


### NFS 版本

在 {{site.data.keyword.BluSoftlayer_full}} 環境中，同時支援 NFS 第 3 版及 NFS 4.1 版。不過，偏好使用 NFS 第 3 版，因為 NFS 4.1 版是有狀態的通訊協定（不像 NFSv3 是無狀態的通訊協定），而且在網路事件期間可能會發生通訊協定問題。NFS 4.1 版必須靜止所有作業，然後完成鎖定收回。在相當忙碌的 NFS 檔案伺服器上，延遲增加可能會造成中斷。缺少 NFS 4.1 版多路徑和幹線也可能會延長 NFS 作業回復時間。

## 提交訂單

當您準備好提交訂單時，可以透過[主控台](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)或 [SLCLI](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI) 下訂單。若要使用 VMware 佈建檔案儲存空間，請按一下[這裡](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## 連接新的儲存空間
{: #mountingstorage}

當您的佈建要求完成時，請授權主機存取新的儲存空間，並配置連線。請根據主機的作業系統，遵循適當的鏈結。
- [在 Linux 上存取 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [在 CentOS 中裝載 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [在 CoreOS 上裝載 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [配置 {{site.data.keyword.filestorage_short}} 以便使用 cPanel 進行備份](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [配置 {{site.data.keyword.filestorage_short}} 以便使用 Plesk 進行備份](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [在 ESXi 主機上裝載 {{site.data.keyword.filestorage_short}} 磁區](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## 管理新的儲存空間

透過入口網站或 SLCLI，您可以管理 {{site.data.keyword.filestorage_short}} 的各種層面，例如主機授權和取消。如需相關資訊，請參閱[管理 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)。
