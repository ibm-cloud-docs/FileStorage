---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-07"

keywords: File Storage, file storage, NFS, provisioning, setup, configuration, mounting storage

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# 入门教程
{: #getting-started}

{{site.data.keyword.filestorage_full}} 是一种基于 NFS 的网络连接的 {{site.data.keyword.filestorage_short}}，具有持久、快速、灵活的特点。在此网络连接的存储器 (NAS) 环境中，您对文件共享功能和性能具有完全控制权。{{site.data.keyword.filestorage_short}} 共享可通过路由 TCP/IP 连接来连接到最多 64 个已授权设备，从而实现弹性。
{:shortdesc}

## 开始之前
{: #prereqs}

通过以下两个选项，可以供应从 20 GB 到 12 TB 的 {{site.data.keyword.filestorage_short}} 卷：<br/>
- 供应**耐久性**层，具有预定义的性能级别和功能，如快照和复制。
- 通过分配的每秒输入/输出操作数 (IOPS) 来构建强大的**性能**环境。

有关 {{site.data.keyword.filestorage_short}} 产品的更多信息，请参阅[关于 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-about)

## 供应注意事项

### 块大小

“耐久性”和“性能”的 IOPS 基于 16 KB 的块大小，其中读/写、随机/顺序工作负载的比例为 50/50。一个 16 KB 的块相当于对卷执行一次写操作。
{:important}

应用程序使用的块大小会直接影响存储器性能。如果应用程序使用的块大小小于 16 KB，那么在达到吞吐量限制之前，会先达到 IOPS 限制。相反，如果应用程序使用的块大小大于 16 KB，那么在达到 IOPS 限制之前，会先达到吞吐量限制。

<table>
  <caption>表 4 显示了块大小和 IOPS 如何影响吞吐量的示例。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>块大小 (KB)</th>
            <th>IOPS</th>
            <th>吞吐量（MB/秒）</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4（Linux 的典型值）</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8（Oracle 的典型值）</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32（SQL Server 的典型值）</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

### 已授权主机

另一个要考虑的因素是使用卷的主机数。如果是单个主机在访问卷，那么可能很难实现可用的最大 IOPS，尤其是在极端 IOPS 计数（10,000 以上）的情况下。如果工作负载需要高吞吐量，那么最好配置至少两台服务器来访问卷，以避免出现单服务器瓶颈。

### 网络连接

以太网连接速度必须快于卷的预期最大吞吐量。一般情况下，不要指望以太网连接饱和到超过可用带宽的 70%。例如，如果您有 6,000 IOPS 并且使用的是 16 KB 块大小，那么卷可以处理约 94 MBps 的吞吐量。如果与 LUN 之间存在 1 Gbps 以太网连接，那么当服务器尝试使用最大可用吞吐量时，此连接会成为瓶颈。这是因为 1 Gbps 以太网连接的理论限制（125 MB/秒）的 70% 仅允许 88 MB/秒。

要实现最大 IOPS，需要落实足够的网络资源。其他注意事项包括在存储器外部使用的专用网络、主机端以及特定于应用程序的调整（IP 堆栈或[队列深度](/docs/infrastructure/FileStorage?topic=FileStorage-hostqueuesettings)以及其他设置）。

存储流量包含在公共虚拟服务器的总网络使用量之内。有关服务可能施加的限制的更多信息，请参阅[虚拟服务器文档](/docs/vsi?topic=virtual-servers-about-public-virtual-servers)。


### NFS 版本

{{site.data.keyword.BluSoftlayer_full}} 环境支持 NFS V3 和 NFS V4.1。但是，首选 NFS V3，因为 NFS V4.1 是有状态协议（NFS V3 是无状态协议），在网络事件期间可能会发生协议问题。NFS V4.1 必须停止所有操作，然后才能完成锁定回收。在相对繁忙的 NFS 文件服务器上，延长的等待时间可能会导致中断。缺少 NFS V4.1 多路径和中继也会延长 NFS 操作恢复时间。

## 提交订单

准备好提交订单时，可以通过[控制台](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole)或 [SLCLI](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI) 来完成此操作。有关通过 VMware 供应文件存储器的信息，请单击[此处](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## 连接新存储器
{: #mountingstorage}

完成供应请求后，授权主机来访问新存储器并配置连接。根据主机的操作系统，访问相应的链接。
- [在 Linux 上访问 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [在 CentOS 中安装 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [在 CoreOS 中安装 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [使用 cPanel 配置 {{site.data.keyword.filestorage_short}} 进行备份](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [使用 Plesk 配置 {{site.data.keyword.filestorage_short}} 进行备份](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [在 ESXi 主机上安装 {{site.data.keyword.filestorage_short}} 卷](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## 管理新存储器

通过门户网站或 SLCLI，可以管理 {{site.data.keyword.filestorage_short}} 的各个方面，例如主机授权和取消。有关更多信息，请参阅[管理 {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage)。
