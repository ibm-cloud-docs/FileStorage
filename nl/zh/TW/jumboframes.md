---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-11"

---
{:pre: .pre}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 針對 Windows 及 Linux 在 {{site.data.keyword.BluSoftlayer_notm}} 中啟用巨大訊框

大型訊框是有效負載大於標準最大傳輸單位 (MTU) 1,500 位元組的乙太網路訊框。大型訊框用於支援最少 1 Gbps 的區域網路，且最大可達到 9,000 個位元組。

必須在來源裝置 <-> 交換器 <-> 路由器 <-> 交換器 <-> 目的地裝置中的整個網路路徑上，配置相同的巨大訊框。如果整個鏈結未設為相同，則會預設為鏈結的最低設定。{{site.data.keyword.BluSoftlayer_full}} 目前將網路裝置設為 9,000。所有客戶裝置都需要設為相同的 9,000 值。
{:important}

## 在 Windows 中啟用巨大訊框

1. 開啟**網路及共用中心**。
2. 按一下**變更介面卡設定**。
3. 用滑鼠右鍵按一下您要啟用巨大訊框的 NIC，然後選取**內容**。
4. 在**網路**標籤下，按一下網路配接卡的**設定**。
5. 選取**進階**標籤。
6. 選取**巨大訊框**，然後將值從 **disabled** 變更為您要的值。此值（例如 9 KB 或 9014 位元組）取決於 NIC。
7. 按一下所有視窗中的**確定**。

當您進行變更時，NIC 的網路連線功能會中斷幾秒鐘。重新啟動裝置，以確認變更生效。
{:tip}


## 在 Linux 中啟用巨大訊框

1. 編輯 eth0 介面的網路配置檔。
   - CentOS、RHEL、Fedora Linux 使用者編輯 `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Debian 和 Ubuntu Linux 使用者編輯 `/etc/network/interfaces`。

2. 附加下列配置指引，以指定訊框大小（以位元組為單位）。
   - CentOS、RHEL、Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian 和 Ubuntu Linux
     ```
     MTU=9000 
     ```
     {: pre}

3. 關閉並儲存檔案。重新啟動「介面 eth0」。
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   此動作會導致網路連線短暫中斷。
