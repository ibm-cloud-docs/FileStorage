---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-24"

keywords: File Storage, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# 常见问题
{: #file-storage-faqs}

## 如何判断哪些 {{site.data.keyword.filestorage_short}} 卷已加密？
{: faq}

查看客户门户网站中 {{site.data.keyword.filestorage_short}} 的列表。您将在卷名右侧看到“锁定”图标，指示已对这些卷进行加密。

## 如果数据中心内有在升级前供应的非加密 {{site.data.keyword.filestorage_short}}，那么在数据中心升级进行加密后我能对此 {{site.data.keyword.filestorage_short}} 进行加密吗？
{: faq}

无法对在数据中心升级之前供应的 {{site.data.keyword.filestorage_short}} 加密。在已升级的数据中心内供应的新 {{site.data.keyword.filestorage_short}} 会自动加密。此过程是自动的，不是可以选择也可以留空的供应设置。通过创建新卷，然后使用基于主机的迁移将数据复制到新的加密卷，可以对非加密存储器上的数据进行加密。有关更多信息，请参阅[迁移文件存储器](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)。

## 怎样知道是在已升级的数据中心内供应 {{site.data.keyword.filestorage_short}}？
{: faq}

在 {{site.data.keyword.filestorage_short}} 订购表单中，所有已升级的数据中心都会标有星号 (`*`)。在订购过程中，会向您指示您正在供应具有加密功能的存储器。供应存储器后，在存储器列表中会看到相应图标，指示该卷已加密。

所有加密卷和文件共享仅在已升级的数据中心内供应。您可以在[此处](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)找到已升级的数据中心和可用功能的完整列表。

## 为什么一些数据中心内供应耐久性 10 IOPS 层的 {{site.data.keyword.filestorage_short}}，而其他数据中心内不供应？
{: faq}

{{site.data.keyword.filestorage_short}} 耐久性类型 10 IOPS/GB 层仅在精选数据中心内提供，很快会增加新的数据中心。您可以在[此处](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC)找到已升级的数据中心和可用功能的完整列表。

## 如何找到 {{site.data.keyword.filestorage_short}} 的正确安装点？
{: faq}

增强型数据中心内供应的所有加密 {{site.data.keyword.filestorage_short}} 卷的安装点与非加密卷不同。要确保使用的是正确的安装点，可以在 UI 中的**卷详细信息**页面中查看安装点信息。还可以通过 API 调用来访问正确的安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

## 可以供应多少个卷？
{: faq}

缺省情况下，总共可以供应 250 个块存储卷和文件存储卷。要增大限制，请联系销售代表。有关更多信息，请参阅[管理存储限制](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits)。

## 一个供应的 {{site.data.keyword.filestorage_short}} 卷可以由多少个实例共享使用？
{: faq}

每个文件卷的缺省授权数限制为 64。要增大此限制，请联系销售代表。

## 一个主机可连接多少个 {{site.data.keyword.filestorage_short}} 卷？
{: faq}

这取决于主机操作系统的处理能力，而不受 {{site.data.keyword.cloud}} 的限制。有关可安装的文件共享数的限制，请参阅操作系统文档。

## 每种文件卷大小允许多少个文件共享？每种卷大小允许的最大文件共享数是多少？
{: #maxfilevolume}
{: faq}

|卷大小|索引节点和文件共享|
|-----|-----|
|20 GB - 39 GB|622,484|
|40 GB - 79 GB|1,245,084|
|80 GB - 99 GB|2,490,263|
|100 GB - 249 GB|3,112,863|
|250 GB - 499 GB|7,782,300|
|500 GB - 999 GB|15,564,695|
|1 TB|31,876,593|
|2 TB|63,753,186|
|3 TB|95,629,970|
| 4 TB |127,506,359|
{: row-headers}
{: class="comparison-table"}
{: caption="表比较" caption-side="top"}
{: summary="Table 1 shows the maximum number of inodes that are allowed based on the volume size. Volume sizes are in the left column. The number of inodes and file shares are on the right."}

## 度量 IOPS
{: faq}

IOPS 根据 16 KB 块的负载概要文件来度量，其中随机 50% 读操作和 50% 写操作。不同于此概要文件的工作负载可能会遇到性能不佳问题。

## 使用更小的块大小来度量性能时，会发生什么情况？
{: faq}

即便使用更小的块大小，也仍然可以获取最大 IOPS。但是，在这种情况下，吞吐量会降低。例如，下面是具有 6000 IOPS 的卷针对各种不同块大小的吞吐量：

- 16 KB * 6000 IOPS == 约 93.75 MB/秒
- 8 KB * 6000 IOPS == 约 46.88 MB/秒
- 4 KB * 6000 IOPS == 约 23.44 MB/秒


## 是按实例还是按卷强制执行分配的 IOPS？
{: faq}

IOPS 会在卷级别强制执行。换句话说，连接到一个具有 6000 IOPS 的卷的两个主机会共享 6000 IOPS。

## 卷需要预热才能达到所需吞吐量吗？
{: faq}

不需要预热。在供应卷之后，您可以立即观察到指定的吞吐量。

## 如果使用更快的以太网连接，可以实现更大的吞吐量吗？
{: faq}

吞吐量限制是在每个卷的级别设置的。即使使用更快的以太网连接也无法提高限制。但是，使用较慢的以太网连接时，带宽可能是潜在瓶颈。

## 防火墙和安全组会影响性能吗？
{: #isolatedstoragetraffic}
{: faq}

最好是在绕过防火墙的 VLAN 上运行存储流量。通过软件防火墙运行存储流量会延长等待时间，并对存储器性能产生负面影响。

## 预计 {{site.data.keyword.filestorage_short}} 中的性能等待时间是多少？   
{: faq}

存储器中的目标等待时间小于 1 毫秒。存储器会连接到共享网络上的计算实例，所以确切的性能等待时间将取决于运行期间的网络流量。

## 删除 {{site.data.keyword.filestorage_short}} 卷时，数据会发生什么情况？
{: faq}

{{site.data.keyword.filestorage_full}} 会在物理存储器上为客户提供文件共享，并且会在重复使用物理存储器之前擦除其上的数据。如果客户对合规性有特殊要求（如 NIST 800-88《存储介质清理指南》），那么在删除存储器之前，需要执行数据清理过程。

## 支持哪些 NFS 版本？
{: faq}

{{site.data.keyword.cloud}} 环境支持 NFS V3 和 NFS V4.1。

首选版本是 NFS V3，因为它是无状态协议，在网络事件发生时更具弹性。

NFS V3 本机支持 `no_root_squash`，通过后者，root 用户客户机可以保留对 NFS 共享的 root 用户许可权。您可以在 NFS V4.1 中启用此功能，方法是通过编辑域信息并运行 `rpcidmapd` 或类似的服务。
有关更多信息，请参阅[为 NFS 实现 no_root_squash](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux#norootsquash)。

对于 vSphere 解决方案，NFS V3 支持的功能多于 V4.1。此类功能包括 Storage DRS 和 Site Recovery Manager。

## 可以在 VMware 部署中启用 VAAI 和硬件加速吗？
{: #isVAAIsupported}
{: faq}

不能。目前不支持 vStorage for API Array Integration 和硬件加速。

## 使驱动器从云数据中心退役时会发生什么情况？
{: faq}

使驱动器退役后，IBM 会在处置前先销毁驱动器。驱动器将变为无法使用。写入该驱动器的任何数据都将变得无法访问。

## 受控故障转移与即时故障转移有何区别？
{: faq}

受控故障转移在执行最后一次同步后中断镜像。即时故障转移会立即中断镜像，并激活副本卷。
