---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: File Storage, modify volume, NFS, file storage, expand capacity

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 扩展文件共享容量
{: #expandCapacity}

利用此新功能，当前 {{site.data.keyword.filestorage_full}} 用户能够直接将其 {{site.data.keyword.filestorage_short}} 的大小扩展到最大 12 TB（以 GB 为增量）。用户无需创建复制项或将数据手动迁移到更大的卷。在调整大小时，不会发生针对存储器的任何中断或访问权缺乏问题。

对卷的记帐会自动更新，以将新价格的按比例差值添加到当前计费周期。然后，在下一个计费周期中采用整个新金额记帐。

此功能仅在[精选数据中心](/docs/infrastructure/FileStorage?topic=FileStorage-news)内提供。

## 可扩展存储器的优点

- **成本管理** - 您可能知道自己的数据存在增长的潜力，但一开始需要的存储量较小。通过扩展能力，客户能够节省起始存储成本，并能根据自己的需求进行增长。  

- **不断增长的存储需求** - 遇到快速增长超过预期的客户需要一种方法来迅速、轻松地增大其存储器大小，以管理这种增长情况。

## 扩展存储容量对复制的影响

对主存储器执行扩展操作将会自动调整副本大小。

## 限制
{: #limitsofextension}

此功能仅可用于在具有增强功能的[数据中心](/docs/infrastructure/FileStorage?topic=FileStorage-news)内供应的存储器。这些数据中心内供应的加密存储器可以增大到最高 12 TB。

使用“耐久性”类型供应的 {{site.data.keyword.filestorage_short}} 的现有大小限制仍然适用（对于 10 IOPS 层，最高为 4 TB，对于其他所有层，最高为 12 TB）。

## 调整存储器大小
{: #resizingsteps}

1. 转至 [{{site.data.keyword.cloud}} 控制台](https://{DomainName}/){: external}。在菜单中，选择**经典基础架构**。单击**存储** > **{{site.data.keyword.filestorage_short}}**。
2. 从列表中选择卷，然后单击**操作** > **修改卷**。
3. 输入新的存储器大小（以 GB 为单位）。
4. 复查您的选择和新的定价。
5. 单击**我已阅读主服务协议...**，然后单击**下订单**。
6. 新的存储器分配会在几分钟后可用。

或者，您可以在 SLCLI 中使用以下命令。
```
# slcli file volume-modify --help
用法：slcli file volume-modify [OPTIONS] VOLUME_ID

选项：
  -c, --new-size INTEGER        文件卷的新大小（以 GB 为单位）。***如果未指定
                                大小，那么将使用卷的原始大小。***
                                可能的大小为：[20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                最小值为：[卷的原始大小]
  -i, --new-iops INTEGER        性能存储器 IOPS，介于 100 到 6000 之间，且为 100 的倍数 [仅针对性能卷]
                                ***如果未指定 IOPS 值，那么将使用该卷的原始 IOPS 值。 ***
                                要求：[如果该卷的原始 IOPS/GB 小于 0.3，那么新 IOPS/GB 也必须小于 0.3。
                                如果该卷的原始 IOPS/GB 大于或者等于 0.3，那么该卷的新 IOPS/GB 也必须
                                大于或者等于 0.3。]
  -t, --new-tier [0.25|2|4|10]  耐久性存储器层 (IOPS/GB) [仅针对耐久性卷]
                                ***如果未指定层，那么将使用该卷的原始层。***
                                要求：[如果该卷的原始 IOPS/GB 为 0.25，那么
                                该卷的新 IOPS/GB 也必须为 0.25。如果该卷的原始 IOPS/GB
                                大于 0.25，那么该卷的新 IOPS/GB 也必须大于 0.25。]
  -h, --help                    显示此消息并退出。
```
