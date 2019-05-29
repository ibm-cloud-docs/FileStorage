---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-14"

keywords: File Storage, Endurance, Performance, IOPS, replication, billing, file storage, NFS,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# 关于 {{site.data.keyword.filestorage_short}}
{: #about}

{{site.data.keyword.filestorage_full}} 是一种基于 NFS 的网络连接的 {{site.data.keyword.filestorage_short}}，具有持久、快速、灵活的特点。在此网络连接的存储器 (NAS) 环境中，您对文件共享功能和性能具有完全控制权。{{site.data.keyword.filestorage_short}} 共享可通过路由 TCP/IP 连接来连接到最多 64 个已授权设备，从而实现弹性。
{:shortdesc}

## 功能
{: #FileStorageFeatures}

{{site.data.keyword.filestorage_short}} 通过一组无与伦比的功能，实现了同类最优水平的耐久性和可用性。{{site.data.keyword.filestorage_short}} 使用业界标准和最佳实践进行构建，旨在保护数据完整性。{{site.data.keyword.filestorage_short}} 可在发生维护事件和意外故障期间保持可用性，并提供一致的性能基线。

可利用 {{site.data.keyword.filestorage_short}} 的以下核心功能：

- **一致的性能基线**
   - 通过为各个卷分配协议级别每秒输入/输出操作数 (IOPS) 来提供。
- **{{site.data.keyword.filestorage_short}}**
   - 可用于基于文件的 NFS 共享。
- **持久性高，弹性大**
   - 在发生维护事件和意外故障期间保护数据完整性并保持可用性，而无需创建和管理操作系统级别的独立磁盘冗余阵列 (RAID)
- **静态数据加密** [（在精选数据中心内提供。）](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - 免费对静态数据进行提供者管理的加密
- **所有支持闪存的存储器** [（在精选数据中心内提供。）](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - 可以在 2 IOPS/GB 或更高级别供应卷的所有闪存存储器。
- **快照** [（在精选数据中心内提供。）](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - 以非破坏性方式捕获时间点数据快照。
- **复制**  [（在精选数据中心内提供。）](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - 在[精选数据中心](/docs/infrastructure/FileStorage?topic=FileStorage-news)内供应存储器时可用。
   - 自动将快照复制到合作伙伴的 {{site.data.keyword.cloud}} 数据中心。
- **高度可用的连接**
   - 使用冗余网络连接以最大限度提高可用性。
   - 基于 NFS 的 {{site.data.keyword.filestorage_short}} 路由的是 TCP/IP 连接。
- **并行访问**
   - 允许多个主机（最多 64 个）同时访问文件卷。
- **集群数据库**
   - 支持高级用例，例如集群数据库。

## 计费

可以选择按小时或按月对文件卷计费。为 LUN 选择的计费类型将应用于其快照空间和副本。例如，如果供应的 LUN 按小时计费，那么任何快照或副本费用都会按小时记帐。如果供应的 LUN 按月计费，那么任何快照或副本费用都会按月记帐。

对于**按小时计费**，在删除 LUN 时或在计费周期结束时（以先发生者为准），将计算文件卷在帐户上存在的小时数。对于使用了数天或不足一个月的存储器，按小时计费是不错的选择。按小时计费仅可用于在[精选数据中心](/docs/infrastructure/FileStorage?topic=FileStorage-news)内供应的存储器。

对于**按月计费**，将从创建日期一直到记帐周期结束按比例计算价格并立即记帐。如果在计费周期结束之前删除了卷，那么不会有任何退款。对于所用数据需要长期（一个月或更长时间）存储和访问的生产工作负载，使用按月计费的存储器是不错的选择。


**性能**
<table>
  <caption>表 1 显示了按月计费和按小时计费的性能存储器的价格。</caption>
  <tr>
   <th>每月价格</th>
   <td>0.10 美元/GB + 0.07 美元/IOP</td>
  </tr>
  <tr>
   <th>每小时价格</th>
   <td>0.0001 美元/GB + 0.0002 美元/IOP</td>
  </tr>
</table>

**耐久性**
<table>
  <caption>表 2 显示了使用按月和按小时计费选项时每个层的耐久性存储器的价格。</caption>
  <tr>
   <th>IOPS 层</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>每月价格</th>
   <td>0.06 美元/GB</td>
   <td>0.15 美元/GB</td>
   <td>0.20 美元/GB</td>
   <td>0.58 美元/GB</td>
  </tr>
  <tr>
   <th>每小时价格</th>
   <td>0.0001 美元/GB</td>
   <td>0.0002 美元/GB</td>
   <td>0.0003 美元/GB</td>
   <td>0.0009 美元/GB</td>
  </tr>
</table>



## 供应

通过以下两个选项，可以供应从 20 GB 到 12 TB 的 {{site.data.keyword.filestorage_short}} 卷：<br/>
- 供应**耐久性**层，具有预定义的性能级别和功能，如快照和复制。
- 通过分配的每秒输入/输出操作数 (IOPS) 来构建强大的**性能**环境。


### 通过耐久性层供应

耐久性 {{site.data.keyword.filestorage_short}} 在四个 IOPS 性能层中可用，以支持各种不同的应用需求。<br />

- **0.25 IOPS/GB** 适用于具有低 I/O 强度的工作负载。这些工作负载的典型特点是在任意时间都有很大比例的不活动数据。示例应用包括存储邮箱或部门级别文件共享。

- **2 IOPS/GB** 适用于最通用的用途。示例应用包括托管支持 Web 应用程序的小型数据库或用于系统管理程序的 VM 磁盘映像。

- **4 IOPS/GB** 适用于高强度工作负载。这些工作负载的典型特点是在任意时间都有较高比例的活动数据。示例应用包括事务型数据库和其他性能敏感型数据库。

- **10 IOPS/GB** 适用于要求最苛刻的工作负载，例如由 NoSQL 数据库创建的工作负载以及为 Analytics 进行的数据处理。此层仅在[精选数据中心](/docs/infrastructure/FileStorage?topic=FileStorage-news)内提供，用于供应的最高达 4 TB 的存储器。

12 TB 耐久性卷最高提供 48,000 IOPS。

为工作负载选择合适的耐久性层非常重要。同样重要的是使用达到最高性能所需的正确块大小、以太网连接速度和主机数。如果其中有任何部分与其他部分不协调，都可能会对产生的吞吐量造成重大影响。

### 通过性能供应

“性能”是一类 {{site.data.keyword.filestorage_short}}，旨在支持具有不太适合耐久性层的已知性能需求的高 I/O 应用。通过为各个卷分配协议级别 IOPS，可实现可预测的性能。可以使用大小范围在 20 GB 到 12 TB 的存储器来供应各种 IOPS 速率 (100 - 48,000)。

{{site.data.keyword.filestorage_short}} 的“性能”可通过网络文件系统 (NFS) 连接来访问和安装。在通过多个服务器同时访问卷时，通常会使用 {{site.data.keyword.filestorage_short}}。一致性性能卷可以根据表 1 中的大小和 IOPS 进行订购，并可用于 Linux 操作系统。

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>表 3 显示了用于性能存储器的大小和 IOPS 组合。<br/><sup><img src="/images/numberone.png" alt="脚注" /></sup> 在精选数据中心内提供了大于 6,000 的 IOPS 限制。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>大小 (GB)</th>
            <th>最小 IOPS</th>
            <th>最大 IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2,000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6,000 或 10,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 或 20,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>2,000</td>
            <td>200</td>
            <td>6,000 或 40,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>3,000-7,000</td>
            <td>300</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
</table>


性能卷旨在以一致地接近所供应 IOPS 级别的方式来运行。通过一致性，能更轻松地调整特定性能级别的应用程序环境的大小和扩展此环境。此外，通过构建具有理想的价格/性能比率的卷，可以优化环境。
