---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-12"

---
{:new_window: target="_blank"}

# 擴充檔案共用容量

使用這個新增特性，現行 {{site.data.keyword.filestorage_full}} 使用者可以立即擴充其 {{site.data.keyword.filestorage_short}} 的大小（以 GB 為增量單位，最多可到 12 TB）。他們不需要建立重複項目，或者手動將資料移轉至較大的磁區。調整大小時，不會發生中斷，也不會無法存取儲存空間。 

會自動更新磁區的計費，以將新價格的按比例差額新增至現行計費週期。然後，即會在下一個計費週期收取新的完整金額。

只有[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內才提供此特性。 

## 可擴充儲存空間的優點

- **成本管理** - 您知道資料可能會成長，但一開始只需要較小的儲存空間量。擴充能力可讓客戶在一開始時節省儲存空間成本，然後不斷成長以因應需要。  

- **成長中的儲存空間需求** - 快速成長的客戶需要一種可快速且輕鬆地增加其儲存空間大小的方式，以管理成長。

## 在抄寫上擴充儲存空間容量的效果

主要儲存空間上的擴充動作會導致自動調整抄本大小。

## 限制

此特性只適用於佈建在具有加強功能之[資料中心](new-ibm-block-and-file-storage-location-and-features.html)內的儲存空間。在這些資料中心內佈建的加密儲存空間最多可以增加到 12 TB。 

已佈建「耐久性」之 {{site.data.keyword.filestorage_short}} 的現有大小限制仍然適用（10 IOPS 層級最多為 4 TB，而所有其他層級最多為 12 TB）。

## 重新調整儲存空間大小

1. 從 {{site.data.keyword.slportal}} 中，按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 型錄中，按一下**基礎架構** > **儲存空間** > **{{site.data.keyword.filestorage_short}}**。
2. 從清單中選取磁區，然後按一下**動作** > **修改磁區**
3. 輸入新的儲存空間大小（以 GB 為單位）。
4. 檢閱您的選取項目及新的定價。
5. 按一下**我已閱讀主要服務合約...**，然後按一下**下訂單**。
6. 在幾分鐘之後，就可以使用您的新儲存空間配置。
