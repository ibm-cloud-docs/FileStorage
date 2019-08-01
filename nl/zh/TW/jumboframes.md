---

copyright:
  years: 2014, 2019
lastupdated: "2019-08-01"

keywords: File Storage, file storage, NSF, networking, jumbo frames

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# {{site.data.keyword.BluSoftlayer_notm}} 中啟用巨大訊框
{: #jumboframes}

大型訊框是有效負載大於標準最大傳輸單位 (MTU) 1,500 位元組的乙太網路訊框。大型訊框用於支援最少 1 Gbps 的區域網路，且最大可達到 9,000 個位元組。

必須在來源裝置 <-> 交換器 <-> 路由器 <-> 交換器 <-> 目的地裝置中的整個網路路徑上，配置相同的巨大訊框。如果整個鏈結未設為相同，則會預設為鏈結的最低設定。{{site.data.keyword.cloud}} 目前將網路裝置設為 9,000。所有客戶裝置都需要設為相同的 9,000 值。
{:important}


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
   {:important}
