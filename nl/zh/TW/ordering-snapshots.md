---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-24"

keywords: File Storage, file storage, NFS, snapshot, ordering snapshot, snapshot space

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 訂購 Snapshot
{: #ordering-snapshots}

若要自動或手動建立儲存空間磁區的 Snapshot，您需要購買空間來存放它們。您可以購買最多達到儲存空間磁區量的容量（在起始磁區購買期間或之後使用以下步驟購買）。

## 決定要訂購多少 Snapshot 空間

一般來說，Snapshot 會根據兩項重要因素來使用 Snapshot 空間：
- 您的作用中檔案系統隨著時間的變更程度、
- 您計劃保留 Snapshot 的時間長度。  

計算所需空間量的方式為**（變更率）**x**（保留的時數/天數/週數/月數資料）**。  

第一個 Snapshot 所使用的空間量微不足道，因為它只是指出作用中檔案系統區塊的 meta 資料（指標）副本。
{:note}

具有許多變更及冗長保留期間的磁區，比起具有適度變更及適度保留排程的磁區，會需要更多的空間。第一種類型的範例是高變更率資料庫。第二種類型的範例是 VMware 資料儲存庫。

如果您擷取 500 GB 實際資料的 12 個每小時 Snapshot，並且在每個 Snapshot 之間有 1% 的變更，則 Snapshot 最終會使用 60 GB。

*（5 GB 變更率）x（12 個每小時 Snapshot）=（60 GB 已使用空間）*

反之，如果情況為 500 GB 實際資料，加上 12 個每小時 Snapshot，而且每小時看到 10% 的變更，則使用的 Snapshot 空間是 600 GB。

*（50 GB 變更率）x（12 個每小時 Snapshot）=（600 GB 已使用空間）*

因此，當您決定需要多少 Snapshot 空間時，請仔細考慮變更率。它對您需要多少 Snapshot 空間有巨大影響。較大的磁區較有可能頻繁地變更。不過，具有 5 GB 變更的 500 GB 磁區，與具有 5 GB 變更的 10 TB 磁區，兩者會使用相同的 Snapshot 空間量。

此外，對於大部分工作負載而言，磁區越大，一開始需要預留的空間就越少。這主要是因為基礎資料效率，以及 Snapshot 在環境中如何運作的本質所致。

## 透過 {{site.data.keyword.cloud_notm}} 主控台訂購 Snapshot 空間

1. 登入 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/){: external}，然後按一下左上方的功能表圖示。
2. 選取**標準基礎架構**。
3. 透過**儲存空間** > **{{site.data.keyword.filestorage_short}}** 來存取「儲存空間」。
4. 按一下 Snapshot 頁框中的**新增 Snapshot 空間**。
5. 選取您需要的空間量和付款方法。
6. 按一下**繼續**。
7. 輸入您有的任何「促銷代碼」，然後按一下**重新計算**。**此訂單的計費**及**訂單檢閱**具有預設值。

   折扣會在處理訂單時套用。
   {:note}
8. 勾選**我已閱讀主要服務合約，並同意其中的條款**勾選框，然後按**下訂單**。在幾分鐘之後，即會佈建您的 Snapshot 空間。

## 透過 SLCLI 訂購 Snapshot 空間

```
# slcli file snapshot-order --help
Usage: slcli file snapshot-order [OPTIONS] VOLUME_ID

Options:
  --capacity INTEGER    Size of snapshot space to create in GB  [required]
  --tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) of the file
                        volume for which space is ordered [optional, and only
                        valid for endurance storage volumes]
  --upgrade             Flag to indicate that the order is an upgrade
  -h, --help            Show this message and exit.
```
