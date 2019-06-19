---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, locations, data centers

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 新位置和功能
{: #news}

{{site.data.keyword.cloud}} 将推出新版本的 {{site.data.keyword.filestorage_full}}！新的存储器在精选数据中心内提供，是更高 IOPS 级别的闪存支持的存储器，具有针对静态数据的磁盘级别加密。在精选数据中心内订购的所有存储器都将自动通过新版本的 {{site.data.keyword.filestorage_short}} 创建。

新卷的 NFS 安装点已更改。有关详细信息，请参阅[增强型 {{site.data.keyword.filestorage_short}} 卷的新安装点](#new-mount-point-for-enhanced-file-storage-volumes)。
{:important}

## 新位置
{: #new-locations}

新的 {{site.data.keyword.filestorage_short}} 在以下区域和数据中心内提供，日后会增加更多数据中心！

|美国 2|拉丁美洲|加拿大|欧盟|亚太地区|澳大利亚|
|-----|-----|-----|-----|-----|------|
|DAL09<br >DAL10<br />DAL12<br />DAL13<br />SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07|MEX01<br />SAO01|MON01<br />TOR01|AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />MIL01<br />OSLO1<br />PAR01|CHE01<br />HKG02<br />SEO01<br />SNG01<br />TOK02<br />TOK04<br />TOK05|MEL01<br />SYD01<br />SYD04<br />        SYD05|
{: caption="表 1 显示了数据中心可用性。每个区域单独一列。某些城市（例如，达拉斯、圣何塞、华盛顿特区、阿姆斯特丹、法兰克福、伦敦和悉尼）有多个数据中心。" caption-side="top"}


## 新功能部件和功能
{: #features}

- [对静态数据进行提供者管理的加密](/docs/infrastructure/FileStorage?topic=FileStorage-encryption)。<br/> 所有 {{site.data.keyword.filestorage_short}} 卷都将自动以加密形式免费供应。
- 10 IOPS/GB 层选项。<br/> “耐久性”类型的 {{site.data.keyword.filestorage_short}} 添加了一个新层，用于支持要求最苛刻的工作负载。
- 所有存储器均通过闪存支持。<br/>在 2 IOPS/GB 或更高级别使用“耐久性”或“性能”选项供应的 {{site.data.keyword.filestorage_short}}，通过全闪存存储器支持。
- 快照和复制支持。
- 为存储器增加了按小时计费选项，此选项计划用于使用时间不到一整月的情况。
- 供应的类型为“性能”的 {{site.data.keyword.filestorage_short}}，最高 48,000 IOPS。
- 对于季节性负载变化，可以调整 IOPS 速率来提高性能。请在[此处](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS)阅读有关此功能的更多信息。
- 使用 [{{site.data.keyword.filestorage_short}} 卷复制功能](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume)创建数据的克隆。
- 存储器可直接扩展（以 GB 为增量）到最大 12 TB，无需创建复制项或将数据手动移至更大的卷。请在[此处](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity)阅读有关此功能的更多信息。

## 功能增强的 {{site.data.keyword.filestorage_short}} 卷的新安装点

这些数据中心内供应的所有增强型 {{site.data.keyword.filestorage_short}} 卷的安装点与非加密卷不同。要确保对两个存储卷使用正确的安装点，可以在控制台的**卷详细信息**页面中查看安装点信息。还可以通过 API 调用来访问正确的安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

要能够访问所有新功能，在通过 API 下订单时，请选择 `Storage-as-a-Service Package 759`。有关通过 API 订购 {{site.data.keyword.filestorage_short}} 的更多信息，请参阅 [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){: external}。
{:important}

升级了更多数据中心时，可重新检查此处来查看这些数据中心，并可了解为 {{site.data.keyword.filestorage_short}} 添加的新特性和功能。
{:tip}
