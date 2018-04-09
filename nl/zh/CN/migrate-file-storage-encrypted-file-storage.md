---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# 将 {{site.data.keyword.filestorage_short}} 迁移到加密 {{site.data.keyword.filestorage_short}}

精选数据中心内已经推出针对耐久性或性能的加密 {{site.data.keyword.filestorage_full}}。下面您将了解有关如何将 {{site.data.keyword.filestorage_short}} 从未加密迁移到加密的信息。有关提供者管理的加密存储器的更多信息，请阅读 [{{site.data.keyword.filestorage_short}} 静态加密](block-file-storage-encryption-rest.html)文章。要查看已升级的数据中心和可用功能的列表，请单击[此处](new-ibm-block-and-file-storage-location-and-features)。

首选迁移路径是同时连接两个卷，并将数据从一个文件卷直接传输到另一个文件卷。具体操作取决于您的操作系统以及在复制操作期间数据是否会更改。

为方便起见，概述的是较为常见的场景。假定您已将非加密文件卷连接到主机。如果尚未连接，请按照下面最适合您所运行操作系统的指示信息来完成此任务。 

**注**：所有加密 {{site.data.keyword.filestorage_short}} 卷的安装点与非加密卷不同。要确保对加密和非加密 {{site.data.keyword.filestorage_short}} 卷使用正确的安装点，可以在 UI 中的**卷详细信息**页面中查看安装点信息，还可以通过以下 API 调用来访问正确的安装点：SoftLayer_Network_Storage::getNetworkMountAddress()。

[在 Linux 上访问 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html)

## 创建加密文件卷

使用以下步骤创建大小相同或更大的加密卷，以帮助执行迁移过程。

### 订购加密耐久性存储卷

1. 单击 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 主页中的**存储** > **{{site.data.keyword.filestorage_short}}**，或者单击 {{site.data.keyword.BluSoftlayer_full}}“目录”中的**基础架构** > **存储** > **{{site.data.keyword.filestorage_short}}**。

2. 单击 {{site.data.keyword.filestorage_short}} 页面上的**订购 {{site.data.keyword.filestorage_short}}** 链接。

3. 选择**耐久性**。

4. 选择原始卷所在的数据中心。请注意，加密仅在带有星号的数据中心内可用。

5. 输入所需的 **IOPS 层**。

6. 选择所需的存储空间量（以 GB 为单位）。对于 TB，1 TB 等于 1,000 GB，12 TB 等于 12,000 GB。

7. 输入用于快照的所需存储空间量（以 GB 为单位）。

8. 从下拉列表中选择 **VMware 操作系统**。

9. 提交订单。
 
### 订购加密性能存储卷

1. 单击 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 主页中的**存储** > **{{site.data.keyword.filestorage_short}}**，或者单击 {{site.data.keyword.BluSoftlayer_full}}“目录”中的**基础架构** > **存储** > **{{site.data.keyword.filestorage_short}}**。

2. 单击**订购 {{site.data.keyword.filestorage_short}}**。

3. 选择**性能**。

4. 选择原始卷所在的数据中心。请注意，加密仅在带有星号 (`*`) 的数据中心内可用。

5. 选择所需的存储空间量（以 GB 为单位），大小等于或大于原始卷。

6. 输入所需的 IOPS，以 100 为区间。

7. 从下拉列表中选择 VMware 操作系统。

8. 提交订单。

存储器将在不到一分钟的时间内进行供应，并且将在客户门户网站的 {{site.data.keyword.filestorage_short}} 页面上显示。

 
## 将新卷连接到主机

“已授权”主机是已向其授予某个卷的访问权的主机。如果没有对主机授权，那么您将无法从系统访问或使用该存储器。

1. 单击加密**卷名**。

2. 滚动到页面的**已授权主机**部分。

3. 单击页面右侧的**授权主机**链接。选择可以访问该卷的主机。

一旦授权后，请将卷连接到主机。

 
## 快照和复制

是否已为原始卷建立快照和复制？如果是，那么需要使用与原始卷相同的设置，为新的加密卷设置复制和快照空间以及创建快照安排。 

请注意，如果目标数据中心尚未升级用于加密，那么在该数据中心升级之前，您将无法建立对新卷的复制。

 
## 迁移数据

您的主机应该同时连接到原始和加密 {{site.data.keyword.filestorage_short}} 卷。否则：

• 确保已经执行上面的步骤并正确参考了文档。

• 开具支持凭单，以获取有关将两个卷连接到主机的进一步帮助。

### 数据注意事项

此时，请考虑原始 {{site.data.keyword.filestorage_short}} 卷上有什么类型的数据，以及如何以最佳方式将其复制到加密卷。如果您有备份、静态内容以及在复制期间不会更改的内容，那么没有任何重大注意事项。

如果是在 {{site.data.keyword.filestorage_short}} 上运行数据库或虚拟机，请确保原始卷上的数据在复制期间不会发生变更，以免发生损坏。如果您担心任何带宽问题，那么应该在非高峰时段执行迁移。如果需要有关这些注意事项的帮助，请立即开具支持凭单。

### Microsoft Windows

要将数据从原始 {{site.data.keyword.filestorage_short}} 卷复制到加密卷，请设置新存储器的格式并使用 Windows 资源管理器复制文件。

### Linux

您可以考虑使用 rsync 来复制数据。下面是示例命令：

`[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage` 

建议将上面的命令与 `--dry-run` 标志一起使用，以确保路径正确排列。如果此过程中断，您可能需要删除正在复制的最后一个目标文件，以确保该文件从头开始复制到新位置。

完成不带 `--dry-run` 标志的此命令后，应该立即将数据复制到加密 {{site.data.keyword.filestorage_short}} 卷。您应该向上滚动并再次运行该命令，以确保没有漏掉任何内容。您还可能需要手动复查这两个位置，以查找是否有任何可能漏掉的内容。

迁移完成后，您将能够将生产移至加密卷，然后从配置中拆离原始卷并将其删除。请注意，删除操作还将除去目标站点上与原始卷关联的任何快照或副本。
