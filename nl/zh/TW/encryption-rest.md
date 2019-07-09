---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18'"

keywords: File Storage, file storage, NFS, security, encryption

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 提供者管理的靜態加密
{: #encryption}

{{site.data.keyword.cloud}} 很認真看待安全，也瞭解能夠加密資料以確保資料安全的重要性。使用提供者管理的加密，依預設會加密已佈建「耐久性」或「效能」選項的 {{site.data.keyword.filestorage_full}}，不需額外付費，而且不會影響效能。

提供者管理的靜態加密特性會使用下列業界標準通訊協定：

* 業界標準 AES-256 加密
* 使用業界標準「金鑰管理交互作業通訊協定 (KMIP)」在內部管理金鑰
* 根據下列標準來驗證儲存空間：
    - 美國聯邦資訊處理標準 (FIPS) 出版品 140-2、
    - 聯邦資訊安全管理法 (FISMA)、
    - 醫療保險轉移和責任法 (HIPAA)、
    - 支付卡產業 (PCI)、
    - Basel II、
    - 加州安全違反資訊行為法案 (SB 1386)，以及
    - 歐盟資料保護指令 (95/46/EC) 規範。

## 保護 Snapshot 或已抄寫儲存空間的安全  

依預設，已加密檔案儲存空間的所有 Snapshot 及抄本也會加密。無法根據磁區來關閉此特性。

## 佈建具有加密的儲存空間

提供者管理的靜態加密特性適用於[精選資料中心](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)。在這些資料中心訂購的所有儲存空間，佈建時都會自動具有靜態資料的加密。

訂購 {{site.data.keyword.filestorage_short}} 時，請選取已標示星號 (`*`) 的資料中心。您可以看到「磁區名稱」欄位的右側有一個鎖定圖示，這表示磁區已加密。請參閱圖 1。

![鎖定圖示表示磁區已加密](/images/encryptedstorage.png)
<caption>圖 1. 指出磁區已加密的鎖定圖示範例。</caption>

資料中心升級之前佈建的任何未加密儲存空間都**不會**自動加密。如果您在已升級的資料中心內擁有未加密的儲存空間，而且想要將它加密，則需要建立磁區，並移動資料。如需相關資訊，請參閱[已升級資料中心內的檔案儲存空間移轉](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)
{:important}
