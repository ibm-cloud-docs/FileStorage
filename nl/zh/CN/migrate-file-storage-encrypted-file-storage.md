---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 将 {{site.data.keyword.filestorage_short}} 迁移到增强型 {{site.data.keyword.filestorage_short}}

现在，增强型 {{site.data.keyword.filestorage_full}} 在精选数据中心内提供。要查看已升级的数据中心和可用功能（例如，可调整的 IOPS 速率和可扩展卷）的列表，请单击[此处](new-ibm-block-and-file-storage-location-and-features.html)。有关提供者管理的加密存储器的更多信息，请参阅 [{{site.data.keyword.filestorage_short}} 静态加密](block-file-storage-encryption-rest.html)。

首选迁移路径是同时连接两个卷，并将数据从一个 LUN 直接传输到另一个 LUN。具体操作取决于您的操作系统以及在复制操作期间数据是否会更改。

假定您已将非加密 LUN 连接到主机。如果尚未连接，请遵循最适合您操作系统的指示信息来完成此任务。

- [在 Linux 上安装 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html)
- [在 CentOS 中安装 NFS/{{site.data.keyword.filestorage_short}}](mounting-nsf-file-storage.html)
- [在 CoreOS 上安装 {{site.data.keyword.filestorage_short}}](mounting-storage-coreos.html)

所有增强型 {{site.data.keyword.filestorage_short}} 卷的安装点都与非加密卷不同。要确保对加密和非加密 {{site.data.keyword.filestorage_short}} 卷使用正确的安装点，可以在 {{site.data.keyword.slportal}} 中的**卷详细信息**页面中查看安装点信息。还可以通过 API 调用来访问正确的安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。
{:tip}


## 创建新的 {{site.data.keyword.filestorage_short}}

使用 API 下订单时，请指定“存储即服务”包，以确保获取新存储器的更新功能。
{:important}

以下指示信息用于通过 {{site.data.keyword.slportal}}/{{site.data.keyword.BluSoftlayer_full}} 目录订购增强型卷/文件共享。新卷的大小应该等于或大于原始卷，以便于迁移。

### 订购新的耐久性存储卷

1. 在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，单击**存储** > **{{site.data.keyword.filestorage_short}}**，或者在 {{site.data.keyword.BluSoftlayer_full}}“目录”中，单击**基础架构** > **存储** > **{{site.data.keyword.filestorage_short}}**。
2. 单击**订购 {{site.data.keyword.filestorage_short}}**。
3. 从**选择存储器类型**列表中，选择**耐久性**。
4. 单击**位置**，然后选择数据中心。
   - 确保将新存储器添加到原始项所在的位置。
5. 选择您的记帐选项。可以选择按月计费或按小时计费。
6. 单击**耐久性**，然后选择 IOPS 层。
6. 从列表中选择**可用存储器大小**。新卷的大小必须等于或大于原始卷。
7. 从列表中选择**快照空间大小**（除了可用空间外）。
8. 单击**继续**。这将显示每月费用和按比例的费用，此时您还有最后一次机会复查订单详细信息。如果要更改订单，请单击**上一步**。
9. 单击**我已阅读主服务协议**复选框，然后单击**下订单**。

### 订购加密性能存储卷

1. 在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，单击**存储** > **{{site.data.keyword.filestorage_short}}**，或者在 {{site.data.keyword.BluSoftlayer_full}}“目录”中，单击**基础架构** > **存储** > **{{site.data.keyword.filestorage_short}}**。
2. 单击**订购 {{site.data.keyword.filestorage_short}}**。
3. 从**选择存储器类型**列表中，选择**性能**。
4. 单击**位置**，然后选择数据中心。
    -  确保将新存储器添加到原始项所在的位置。
5. 选择您的记帐选项。可以选择“每小时计费”和“每月计费”。
6. 选择相应的**存储器大小**旁边的单选按钮。
6. 在**指定 IOPS** 字段中，输入 IOPS。
7. 单击**继续**。这将显示每月费用和按比例的费用，此时您还有最后一次机会复查订单详细信息。如果要更改订单，请单击**上一步**。
8. 单击**我已阅读主服务协议**复选框，然后单击**下订单**。

存储器将在不到一分钟的时间内进行供应，并且会在 {{site.data.keyword.slportal}} 的 {{site.data.keyword.filestorage_short}} 页面上显示。


## 授权主机访问新的 {{site.data.keyword.filestorage_short}}

“已授权”主机是已向其授予卷访问权的主机。如果没有对主机授权，那么您将无法从系统访问或使用该存储器。

1. 单击新卷的名称。
2. 滚动到**已授权主机**部分。
3. 单击右侧的**授权主机**链接。选择可以访问该卷的主机。

授权主机后，将卷连接到主机。


## 设置快照和复制

如果为原始卷建立了快照和复制，那么需要为新卷设置快照和复制。使用与原始卷相同的设置来配置复制和快照空间，并创建快照安排。

如果目标数据中心不具有加密功能，那么只有升级该数据中心，才能建立对新卷的复制。
{:important}


## 迁移数据

1. 同时连接到原始和新的 {{site.data.keyword.filestorage_short}} 卷。
  - 如果在将这两个文件共享连接到主机时需要帮助，请开具支持凭单。

2. 请考虑原始 {{site.data.keyword.filestorage_short}} 卷上有什么类型的数据，以及如何以最佳方式将其复制到新的文件共享。
  - 如果您有备份、静态内容以及在复制期间不会更改的内容，那么没有任何重大注意事项。
  - 如果是在 {{site.data.keyword.filestorage_short}} 上运行数据库或虚拟机，请确保数据在复制期间不会发生变更，以免发生数据损坏。如果您担心任何带宽问题，请在非高峰时段执行迁移。如果需要有关这些注意事项的帮助，请开具支持凭单。

3. 复制数据。
   - **Microsoft Windows**
     - 要将数据从原始 {{site.data.keyword.filestorage_short}} LUN 复制到新 LUN，请设置新存储器的格式并使用 Windows 资源管理器复制文件。
   - **Linux**
     - 可以使用 `rsync` 来复制数据。
       ```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

   最好将上一个命令与 `--dry-run` 标志一起使用一次，以确保路径正确排列。如果此过程中断，您可以删除正在复制的最后一个目标文件，以确保该文件从头开始复制到新位置。

      完成不带 `--dry-run` 标志的此命令后，数据应该会复制到新的 {{site.data.keyword.filestorage_short}} 卷。再次运行此命令，以确保没有漏掉任何内容。您还可以手动复查这两个位置，以查找是否有任何可能漏掉的内容。

      迁移完成后，您可以将生产移至新的 LUN。然后，可以从配置中拆离原始卷并将其删除。删除操作还将除去目标站点上与原始卷关联的任何快照或副本。
