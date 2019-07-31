---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-25"

keywords: File Storage, file storage, NFS, authorizing hosts, rewoke access, grant access, view authorizations

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# 管理 {{site.data.keyword.filestorage_short}}
{: #managingstorage}

您可以透過 {{site.data.keyword.cloud}} 主控台管理 {{site.data.keyword.filestorage_full}} 磁區。

## 授權主機存取 {{site.data.keyword.filestorage_short}}

「授權」主機是已獲得特定磁區存取權的主機。如果沒有主機授權，則無法從系統中存取或使用儲存空間。授權主機存取您的磁區，會產生使用者名稱及密碼。

您可以授權及連接與儲存空間位於相同資料中心的主機。您可以有多個帳戶，但無法授權某個帳戶的主機存取您在另一個帳戶上的儲存空間。
{:important}

1. 移至 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/){: external}。從功能表中選取**標準基礎架構**。
2. 按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**，然後按一下**磁區名稱**。
3. 捲動至頁面的**授權主機**區段。
4. 按一下右側的**授權主機**。
5. 選取裝置類型、子網路或 IP 位址，以過濾可用的主機清單。

   當清單依子網路過濾時，顯示的子網路就是與儲存空間磁區位於相同資料中心內的已訂閱子網路。
   {:note}
6. 從清單選取一台或多台主機，然後按一下**儲存**。

或者，您可以在 SLCLI 中使用下列指令。
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

  Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  -s, --subnet-id TEXT      An ID of one subnet to authorize.
  --help                    Show this message and exit.
```

## 檢視獲授權存取 {{site.data.keyword.filestorage_short}} 磁區的主機清單

1. 移至 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/){: external}。從功能表中選取**標準基礎架構**。
2. 按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**，然後按一下**磁區名稱**。
3. 將頁面向下捲動至**授權主機**區段。

在那裡，您可以看到目前已獲授權存取磁區的主機清單。

或者，您可以在 SLCLI 中使用下列指令。
```
# slcli file access-list --help
Usage: slcli file access-list [OPTIONS] VOLUME_ID

Options:
 --sortby TEXT   Column to sort by
 --columns TEXT  Columns to display. Options: id, name, type,
                 private_ip_address, source_subnet, host_iqn, username,
                 password, allowed_host_id
 -h, --help      Show this message and exit.
```


## 檢視已授權主機存取的 {{site.data.keyword.filestorage_short}} 磁區

您可以檢視主機可存取的磁區，包括建立連線所需的資訊 -「磁區名稱」、「儲存空間類型」、「目標位址」、容量及位置。

1. 移至 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/){: external}。
2. 從功能表中選取**標準基礎架構**。
3. 按一下**裝置** > **裝置清單**。
4. 按一下適當的裝置。
5. 選取「儲存空間」標籤。

您會看到此特定主機可存取的儲存空間磁區清單，這些儲存空間磁區全部是依儲存空間類型（區塊、檔案、其他）分組。從個別的**動作**功能表中，您可以授權更多的儲存空間或移除存取權。


## 裝載及卸載 {{site.data.keyword.filestorage_short}}

您可以使用**磁區詳細資料**視圖中所提供的裝載點資訊，從主機裝載 {{site.data.keyword.filestorage_short}}。請參閱[在 Linux 上存取 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)


## 撤銷主機對 {{site.data.keyword.filestorage_short}} 的存取權

如果您要停止主機對特定儲存空間磁區的存取權，則可以撤銷存取權。撤銷存取權時，會從磁區中捨棄主機連線。作業系統及應用程式無法再與磁區通訊。

若要避免主機端問題，請先從您的作業系統卸載儲存空間磁區，然後再撤銷存取權，以避免遺漏磁碟機或資料毀損。
{:important}

您可以從「裝置清單」中的「儲存空間」或從「儲存空間」視圖中，撤銷存取權。

### 從裝置清單撤銷存取權

1. 移至 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/){: external}。
2. 從功能表中選取**標準基礎架構**。
3. 按一下**裝置** > **裝置清單**。
2. 按兩下適當的裝置。
3. 選取**儲存空間**標籤。
4. 您會看到此特定主機可存取的儲存空間磁區清單，這些儲存空間磁區全部是依儲存空間類型（區塊、檔案、其他）分組。選取您要撤銷其存取權之磁區旁的個別**動作**功能表，然後按一下**撤銷存取權**。
5. 請確認您是否要撤銷磁區的存取權，因為此動作無法復原。按一下**是**來撤銷磁區存取權，或按一下**否**來取消動作。

如果您要中斷多個磁區與特定主機的連線，則需要對每個磁區重複「撤銷存取權」動作。
{:tip}


### 從儲存空間視圖撤銷存取權

1. 移至 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/){: external}。
2. 從功能表中選取**標準基礎架構**。
3. 按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**，然後選取您要從中撤銷存取權的**磁區**。
4. 捲動至頁面的**授權主機**區段。
5. 按一下要撤銷其存取權之主機旁邊的**動作**，然後選取**撤銷存取權**。
6. 請確認您是否要撤銷磁區的存取權，因為此動作無法復原。按一下**是**來撤銷磁區存取權，或按一下**否**來取消動作。

如果您要中斷多個主機與特定磁區的連線，則需要對每個主機重複「撤銷存取權」動作。
{:tip}

### 透過 SLCLI 撤銷存取。
您可以在 SLCLI 中使用下列指令。
```
# slcli file access-revoke --help
Usage: slcli file access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of one virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of one IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  -s, --subnet-id TEXT      An ID of one subnet to revoke authorization.
  --help                    Show this message and exit.
```

## 取消儲存空間磁區

如果您不再需要特定磁區，可以取消該儲存空間。若要取消儲存空間磁區，您需要先撤銷任何主機的存取權。

1. 移至 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/){: external}。從功能表中選取**標準基礎架構**。
2. 按一下**儲存空間** > **{{site.data.keyword.filestorage_short}}**。
3. 按一下要取消之磁區的**動作**，然後選取**取消 {{site.data.keyword.filestorage_short}}**。
4. 確認您是要立即取消磁區，還是在佈建磁區的週年日取消磁區。

   如果您選取在磁區週年日取消磁區的選項，則可以在其週年日之前讓取消要求失效。
   {:tip}
5. 按一下**繼續**或**關閉**。
6. 按一下確認通知勾選框，然後按一下**確認**。

或者，您可以在 SLCLI 中使用下列指令。
```
# slcli file volume-cancel --help
Usage: slcli file volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the file storage volume immediately instead of on the
                 billing anniversary
  -h, --help     Show this message and exit.
```

取消磁區時，會有 24 小時的收回等待期間。在那 24 小時內，您仍可以在主控台看到磁區。收回期間到期時，會破壞資料，並且也會從主控台移除磁區。不過，磁區的計費會立即停止。如需相關資訊，請參閱[常見問題](/docs/infrastructure/FileStorage?topic=FileStorage-file-storage-faqs)。
{:note}

 作用中的抄本可能會阻擋儲存空間磁區的收回。請確定磁區已不再裝載、主機授權已撤銷，且抄寫已取消，然後才試圖取消原始磁區。
