---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-11"

---
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}

# 常见问题

## 如何判断哪些 {{site.data.keyword.filestorage_short}} 卷已加密？
{: faq}

查看客户门户网站中 {{site.data.keyword.filestorage_short}} 的列表。您将在卷名右侧看到“锁定”图标，指示已对这些卷进行加密。

## 如果数据中心内有在升级前供应的非加密 {{site.data.keyword.filestorage_short}}，那么在数据中心升级进行加密后我能对此 {{site.data.keyword.filestorage_short}} 进行加密吗？
{: faq}

无法对在数据中心升级之前供应的 {{site.data.keyword.filestorage_short}} 加密。在已升级的数据中心内供应的新 {{site.data.keyword.filestorage_short}} 会自动加密。此过程是自动的，不是可以选择也可以留空的供应设置。通过创建新卷，然后使用基于主机的迁移将数据复制到新的加密卷，可以对非加密存储器上的数据进行加密。有关更多信息，请参阅[迁移文件存储器](migrate-file-storage-encrypted-file-storage.html)。

## 怎样知道是在已升级的数据中心内供应 {{site.data.keyword.filestorage_short}}？
{: faq}

在 {{site.data.keyword.filestorage_short}} 订单表单中，所有已升级的数据中心都会标有星号 (`*`)。在订购过程中，会向您指示您正在供应具有加密功能的存储器。供应存储器后，在存储器列表中会看到相应图标，指示该卷已加密。

所有加密卷和文件共享仅在已升级的数据中心内供应。您可以在[此处](new-ibm-block-and-file-storage-location-and-features.html)找到已升级的数据中心和可用功能的完整列表。

## 为什么一些数据中心内供应耐久性 10 IOPS 层的 {{site.data.keyword.filestorage_short}}，而其他数据中心内不供应？
{: faq}

{{site.data.keyword.filestorage_short}} 耐久性类型 10 IOPS/GB 层仅在精选数据中心内提供，很快会增加新的数据中心。您可以在[此处](new-ibm-block-and-file-storage-location-and-features.html)找到已升级的数据中心和可用功能的完整列表。

## 如何找到 {{site.data.keyword.filestorage_short}} 的正确安装点？
{: faq}

增强型数据中心内供应的所有加密 {{site.data.keyword.filestorage_short}} 卷的安装点与非加密卷不同。要确保使用的是正确的安装点，可以在 UI 中的**卷详细信息**页面中查看安装点信息。还可以通过 API 调用来访问正确的安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

## 可以供应多少个卷？
{: faq}

缺省情况下，总共可以供应 250 个块存储卷和文件存储卷。要增大限制，请联系销售代表。有关更多信息，请参阅[管理存储限制](managing-storage-limits.html)。

## 一个供应的 {{site.data.keyword.filestorage_short}} 卷可以由多少个实例共享使用？
{: faq}

每个文件卷的缺省授权数限制为 64。要增大此限制，请联系销售代表。

## 一个主机可连接多少个 {{site.data.keyword.filestorage_short}} 卷？
{: faq}

这取决于主机操作系统的处理能力，而不受 {{site.data.keyword.BluSoftlayer_full}} 的限制。有关可安装的文件共享数的限制，请参阅操作系统文档。

## 每种文件卷大小允许多少个文件共享？每种卷大小允许的最大文件共享数是多少？
{: faq}

<table>
  <caption>表 1 显示了根据卷大小允许的最大索引节点数。左列是卷大小。右列是索引节点数和文件共享数。</caption>
  <thead>
    <tr>
      <th>卷大小</th>
      <th>索引节点和文件共享</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB - 39 GB</td>
      <td>622,484</td>
    </tr>
    <tr>
      <td>40 GB - 79 GB</td>
      <td>1,245,084</td>
    </tr>          
    <tr>
      <td>80 GB - 99 GB</td>
      <td>2,490,263</td>
    </tr>          
    <tr>
      <td>100 GB - 249 GB</td>
      <td>3,112,863</td>
    </tr>          
    <tr>
      <td>250 GB - 499 GB</td>
      <td>7,782,300</td>
    </tr>          
    <tr>
      <td>500 GB - 999 GB</td>
      <td>15,564,695</td>
    </tr>
    <tr>
      <td>1 TB</td>
      <td>31,876,593</td>
    </tr>
    <tr>
      <td>2 TB</td>
      <td>63,753,186</td>
    </tr>
    <tr>
      <td>3 TB</td>
      <td>95,629,970</td>
    </tr>
    <tr>
      <td>4 TB - 12 TB</td>
      <td>127,506,359</td>
    </tr>
   </tbody>
</table>

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
{: faq}

最好是在绕过防火墙的 VLAN 上运行存储流量。通过软件防火墙运行存储流量会延长等待时间，并对存储器性能产生负面影响。

## 预计 {{site.data.keyword.filestorage_short}} 中的性能等待时间是多少？   
{: faq}

存储器中的目标等待时间小于 1 毫秒。存储器会连接到共享网络上的计算实例，所以确切的性能等待时间将取决于运行期间的网络流量。

## 删除 {{site.data.keyword.filestorage_short}} 卷时，数据会发生什么情况？
{: faq}

{{site.data.keyword.filestorage_full}} 会在物理存储器上为客户提供文件共享，并且会在重复使用物理存储器之前擦除其上的数据。如果客户对合规性有特殊要求（如 NIST 800-88《存储介质清理指南》），那么在删除存储器之前，需要执行数据清理过程。

## 使驱动器从云数据中心退役时会发生什么情况？
{: faq}

使驱动器退役后，IBM 会在处置前先销毁驱动器。驱动器将变为无法使用。写入该驱动器的任何数据都将变得无法访问。
