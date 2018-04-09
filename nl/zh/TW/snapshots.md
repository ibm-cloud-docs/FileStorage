---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 使用 Snapshot

Snapshot 是一種 {{site.data.keyword.filestorage_full}} 特性。Snapshot 代表磁區在特定時間點的內容。Snapshot 可讓您保護資料而不影響效能、耗用最小空間，而且視為資料保護的第一線防禦。如果使用者意外修改或刪除具有 Snapshot 特性之磁區中的重要資料，則可以輕鬆且快速地從 Snapshot 副本中還原資料。

{{site.data.keyword.filestorage_short}} 提供兩種方式來擷取 Snapshot - 透過自動建立及刪除每一個儲存空間磁區之 Snapshot 副本的可配置 Snapshot 排程。您也可以根據需求，建立其他 Snapshot 排程、手動刪除副本，以及管理排程。第二種方法是擷取手動 Snapshot。

Snapshot 副本是 {{site.data.keyword.filestorage_short}} 磁區的唯讀映像檔，可擷取磁區某個時間點的狀態。在建立 Snapshot 副本所需的時間以及儲存空間這兩方面，Snapshot 副本都極具效率。不論儲存空間上的磁區大小或活動層次為何，只需要幾秒鐘就可以建立 {{site.data.keyword.filestorage_short}} Snapshot 副本（通常不到 1 秒）。建立 Snapshot 副本之後，對資料物件的變更會反映在物件現行版本的更新中，就像 Snapshot 副本不存在一樣。同時，資料的副本仍會完全穩定。 

Snapshot 副本不會造成任何效能額外負擔；使用者可以輕鬆地儲存多達 50 個已排定的 Snapshot，且每個 {{site.data.keyword.blockstorageshort}} 磁區可以有 50 個手動 Snapshot，這全部都可以作為資料的唯讀及線上版本進行存取。

Snapshot 可讓使用者

- 不中斷地建立時間點回復點
- 將磁區回復到前一個時間點
- 您必須購買磁區所需的部分 Snapshot 空間量，才能擷取其 Snapshot。在起始磁區訂購期間或起始佈建之後可以新增 Snapshot 空間，方法是透過「磁區詳細資料」頁面，按一下「動作」下拉按鈕，然後選取「新增 Snapshot 空間」。請注意，已排定及手動 Snapshot 會共用 Snapshot 空間，因此請相應地訂購您的空間。

## Snapshot 最佳作法
Snapshot 設計取決於客戶環境。下列設計考量可協助您計劃及實作 Snapshot 副本： 
- 	透過排程最多可以建立 50 個 Snapshot，且在每一個磁區或 LUN 上最多可以手動建立 50 個 Snapshot。 
- 	不要過度擷取快照。請確定已排定的 Snapshot 頻率符合 RTO 及 RPO 需求，以及應用程式商業需求，方法是排定每小時、每日或每週 Snapshot。 
- 	Snapshot AutoDelete 應該用來控制儲存空間耗用的成長。<br/>
    **附註**：AutoDelete 臨界值固定為 95%。
    
## Snapshot 副本如何耗用磁碟空間？
Snapshot 副本可使磁碟耗用量降到最低，方法是保留個別區塊，而非整個檔案。只有在變更或刪除作用中檔案系統中的檔案時，Snapshot 副本才會開始耗用額外空間。發生此情況時，仍會保留原始檔案區塊作為一個以上 Snapshot 副本的一部分。在作用中檔案系統中，會將已變更的區塊重新寫入至磁碟上的不同位置，或當成作用中檔案區塊完全移除。因此，除了已修改的作用中檔案系統中的區塊所使用的磁碟空間之外，仍會保留原始區塊所使用的磁碟空間，以反映作用中檔案系統在變更之前的狀態。

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
    <tbody>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Snapshot 副本前後的磁碟空間使用情形</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Snapshot 副本之前"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Snapshot 副本之後"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Snapshot 副本之後的變更"></td>
     </tr><tr>
        <td style="border: 0.0px;">建立任何 Snapshot 副本之前，只有作用中檔案系統才會耗用磁碟空間。</td>
        <td style="border: 0.0px;">建立 Snapshot 副本之後，作用中檔案系統及 Snapshot 副本會指向相同的磁碟區塊。Snapshot 副本不會使用額外的磁碟空間。</td>
        <td style="border: 0.0px;">從作用中檔案系統刪除 <i>myfile.txt</i> 之後，Snapshot 副本仍然會包括該檔案，並參照其磁碟區塊。這是刪除作用中檔案系統資料不一定會釋放磁碟空間的原因。</td>
      </tr>
    </tbody>
</table>



## 如何購買 Snapshot 空間？

為了自動或手動建立儲存空間磁區的 Snapshot，您需要購買空間來保留它們。您可以購買最多達儲存空間磁區量的容量（在起始磁區購買期間或稍後使用下面的步驟）。

1. 透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 的**儲存空間** > **{{site.data.keyword.filestorage_short}}** 標籤來存取儲存空間
2. 按一下 Snapshot 頁框中的「新增 Snapshot 空間」鏈結。按一下 Snapshot 頁框中的**立即購買 Snapshot 空間**鏈結。
3. 按一下適當數量旁的圓鈕，以選取您需要的空間量。
4. 按一下**繼續**。
5. 輸入您有的任何「促銷代碼」，然後按一下**重新計算**。「此訂單的計費」及「訂單檢閱」都會有預設值。
6. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。在幾分鐘之後，即會佈建您的 Snapshot 空間。

## 如何建立 Snapshot 排程？

Snapshot 排程可讓您決定要建立儲存空間磁區之時間點參照的頻率及時間。每個儲存空間磁區最多可以有 50 個 Snapshot。排程是透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 的**儲存空間** > **{{site.data.keyword.filestorage_short}}** 標籤來管理。

您必須先購買 Snapshot 空間（如果未在起始佈建儲存空間磁區期間購買的話），才能設定起始排程。

### 新增 Snapshot 排程

Snapshot 排程可以設定為每小時、每日及每週間隔，且各有不同的保留週期。每個儲存空間磁區最多可以有 50 個已排定的 Snapshot（這可以混合使用每小時、每日及每週排程）及手動 Snapshot。

1. 按一下儲存空間磁區，按一下**動作**下拉方框，然後按一下**排程 Snapshot**。
2. 在「新建排程 Snapshot」對話框中，有三種不同的 Snapshot 頻率可供選取。使用三者的任意組合，以建立綜合性的 Snapshot 排程。
   - 每小時
      - 指定應該在每小時的幾分執行 Snapshot；預設值是現行分鐘。
      - 指定在捨棄最舊的 Snapshot 之前，要保留的每小時 Snapshot 數目。
   - 每日
      - 指定應該在幾點幾分執行 Snapshot；預設值是現行小時及分鐘。
      - 選取在捨棄最舊的 Snapshot 之前，要保留的每小時 Snapshot 數目。
   - 每週
      - 指定應該在星期幾的幾點幾分執行 Snapshot；預設值是現行日、小時及分鐘。
      - 選取在捨棄最舊的 Snapshot 之前，要保留的每週 Snapshot 數目。
3. 按一下**儲存**，然後建立另一個頻率不同的排程。請注意，您將收到警告訊息，而且，如果已排定 Snapshot 總數超過 50，則無法儲存。

所擷取 Snapshot 的清單即會顯示在「詳細資料」頁面的 Snapshot 區段中。

## 如何擷取手動 Snapshot？

在應用程式升級或維護期間的各種時間點，都可以擷取手動 Snapshot。它們也可讓您跨多台機器擷取 Snapshot，這些機器已在應用程式層次暫時予以停用。

每個儲存空間磁區最多可以有 50 個手動 Snapshot。

1. 按一下儲存空間磁區。
2. 按一下「動作」下拉方框。
3. 按一下**擷取手動 Snapshot**。將會擷取 Snapshot，並顯示在「詳細資料」頁面的 Snapshot 區段中。它的排程會是「手動」。

## 如何查看具有「已耗用空間」及「管理功能」的 Snapshot 清單？

您可以在「詳細資料」頁面（**儲存空間** > **{{site.data.keyword.filestorage_short}}**）上查看已保留 Snapshot 及已耗用空間的清單。使用**動作**下拉功能表或頁面上各種區段中的鏈結，以在**詳細資料**頁面上處理管理功能（編輯排程以及新增其他空間）。

## 如何檢視保留的 Snapshot 清單？

保留的 Snapshot 是根據您在設定排程時於**保留最後一個欄位**中輸入的數字。您可以在 **Snapshot** 區段下檢視所擷取的 Snapshot。Snapshot 會依排程列出。

## 如何查看已使用的 Snapshot 空間量？

**詳細資料**頁面頂端的圓餅圖會顯示已使用的空間量，以及剩下的空間量。當您開始達到空間臨界值 75%、90% 及 95% 時，會收到通知。
## 如何變更磁區的 Snapshot 空間量？

您可能需要將 Snapshot 空間新增至先前沒有任何 Snapshot 空間或可能需要額外 Snapshot 空間的磁區。根據需求，您可以新增 5 GB 到 4,000 GB。附註：Snapshot 空間只能增加，而不能減少。在您確定實際需要的空間量之前，建議您選取較小的空間量。請記住，自動及手動 Snapshot 會共用相同的空間。

透過**儲存空間** > **{{site.data.keyword.filestorage_short}}** 來變更 Snapshot 空間。

1. 按一下儲存空間磁區，按一下**動作**下拉方框，然後按一下**新增其他 Snapshot 空間**。
2. 從提示中，選取大小範圍。大小範圍通常是從 0 到您的磁區大小。
3. 按一下**繼續**，以佈建額外的空間。
4. 輸入您有的任何「促銷代碼」，然後按一下**重新計算**。**此訂單的計費**及**訂單檢閱**都會顯示預設值。
5. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。在幾分鐘之後，即會佈建您的額外 Snapshot 空間。

## 如何在接近我的 Snapshot 空間限制及 Snapshot 已刪除時收到通知？

當您達到三個不同的空間臨界值（75%、90% 及 95%）時，會透過「支援中心」內的支援問題單將通知傳送至帳戶上的「主要使用者」。

- 75% 容量：Snapshot 空間使用率超過 75% 時會傳送警告。如果您注意到該警告，並手動新增空間，或刪除已保留且不需要的 Snapshot，則會註記動作，並關閉問題單。如果您未執行任何動作，則必須手動確認該問題單，然後將其關閉。
- 90% 容量：Snapshot 空間使用率超過 90% 時會傳送第二個警告。就像達到 75% 的容量時，如果您採取必要的動作來減少使用的空間，則會註記動作，並關閉問題單。如果您未執行任何動作，則必須手動確認該問題單，然後將其關閉。
- 95% 容量：傳送最終警告。如果未採取任何動作讓空間低於臨界值，則會產生通知，並自動進行刪除，以便可以建立未來的 Snapshot。會刪除已排定的 Snapshot（從最舊的 Snapshot 開始），直到使用率低於 95% 為止，而且每次使用率超出 95% 時都會繼續進行刪除，直到它低於臨界值為止。如果手動增加空間或刪除 Snapshot，則會重設警告，並在再次超出臨界值時重新發出。如果未採取任何動作，則這會是收到的唯一警告。

## 如何刪除 Snapshot 排程？

透過**儲存空間** > **{{site.data.keyword.filestorage_short}}** 可以取消 Snapshot 排程。

1. 在**詳細資料**頁面的 **Snapshot 排程**頁框中，按一下要刪除的排程。
2. 按一下要刪除之排程旁的勾選框，然後按一下**儲存**。<br/>
注意：如果您要使用抄寫特性，則請確定所刪除的排程不是抄寫所使用的排程。如需刪除抄寫排程的相關資訊，請按一下[這裡](replication.html)。

## 如何刪除 Snapshot？

可以手動移除不再需要的 Snapshot，以釋放空間供未來的 Snapshot 使用。透過**儲存空間** > **{{site.data.keyword.filestorage_short}}** 來進行刪除。

1. 按一下儲存空間磁區，然後向下捲動至 Snapshot 區段，以查看現有 Snapshot 清單。
2. 按一下特定 Snapshot 旁的「動作」下拉清單，然後按一下**刪除**來刪除 Snapshot。這不會影響相同排程上的任何未來或過去 Snapshot，因為 Snapshot 之間沒有相依關係。

當您達到空間限制時，會自動刪除未依上述方式刪除的手動 Snapshot（最舊的最先刪除）。

## 如何使用 Snapshot 將我的儲存空間磁區還原至特定時間點？

因為使用者錯誤或資料毀損，所以您可能需要將儲存空間磁區還原至特定時間點。

1. 從主機中卸載並分離您的儲存空間磁區。
   - 按一下[這裡](accessing-file-storage-linux.html)，以取得 Linux 上的 {{site.data.keyword.filestorage_short}} 指示。
2. 按一下 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中的**儲存空間**、**{{site.data.keyword.filestorage_short}}**。
3. 向下捲動並按一下要還原的磁區。「詳細資料」頁面的 Snapshot 區段會顯示所有已儲存 Snapshot 的清單及其大小和建立日期。
4. 按一下要使用之 Snapshot 的**動作**按鈕，然後按一下**還原**。 

   **附註**：執行還原會導致在擷取所使用 Snapshot 之後建立或修改的資料流失。完成時，儲存空間磁區將回到擷取 Snapshot 時所處的相同狀態。系統會出現提示來通知您這種情況。
5. 按一下**是**，以起始還原。您將會在頁面頂端收到一則訊息，指出使用選取的 Snapshot 還原磁區。此外，{{site.data.keyword.filestorage_short}} 上的磁區旁會出現一個圖示，指出正在進行作用中交易。將游標移至圖示上方會產生一個對話框，指出交易。完成交易之後，圖示即會消失。
6. 將儲存空間磁區裝載並重新連接至主機。
   - 按一下[這裡](accessing-file-storage-linux.html)，以取得 Linux 上的 {{site.data.keyword.filestorage_short}} 指示。
   
**附註**：還原磁區將導致在還原的 Snapshot 之前刪除所有 Snapshot。

