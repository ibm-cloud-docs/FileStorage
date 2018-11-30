---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}


# 管理 {{site.data.keyword.filestorage_short}}

您可以通过 {{site.data.keyword.slportal}} 管理 {{site.data.keyword.filestorage_full}} 卷。

## 授权主机访问 {{site.data.keyword.filestorage_short}}

“已授权”主机是已向其授予特定卷访问权的主机。如果没有对主机授权，那么您将无法从系统访问或使用该存储器。授权主机来访问卷会生成用户名和密码。

您可以授权和连接与存储器位于同一数据中心的主机。您可以有多个帐户，但不能授权一个帐户中的主机来访问其他帐户上的存储器。
{:important}

1. 单击**存储** > **{{site.data.keyword.filestorage_short}}**，然后单击**卷名**。
2. 滚动到页面的**已授权主机**部分。
3. 单击右侧的**授权主机**。选择可以访问该特定卷的主机。


## 查看授权访问 {{site.data.keyword.filestorage_short}} 卷的主机的列表

1. 单击**存储 > {{site.data.keyword.filestorage_short}}**，然后单击**卷名**。
2. 向下滚动到页面的**已授权主机**部分。

在此，您将看到当前已授权访问该卷的主机的列表。


## 查看授权主机访问的 {{site.data.keyword.filestorage_short}} 卷

可以查看主机有权访问的卷，包括建立连接所需的信息 - 卷名、存储类型、目标地址、容量和位置。

1. 在 [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} 中，单击**设备** > **设备列表**。
2. 单击相应的设备。
2. 选择“存储”选项卡。

这将向您显示此特定主机有权访问的存储卷的列表，全部按存储类型（块、文件或其他）进行分组。通过相应的**操作**菜单，可以授权更多存储器访问权或除去访问权。


## 安装和卸装 {{site.data.keyword.filestorage_short}}

可以使用**卷详细信息**视图中提供的安装点信息，从主机安装 {{site.data.keyword.filestorage_short}}。请参阅[在 Linux 上访问 {{site.data.keyword.filestorage_short}}](accessing-file-storage-linux.html)


## 撤销主机对 {{site.data.keyword.filestorage_short}} 的访问权

如果要停止从主机访问特定存储卷，可以撤销相应的访问权。撤销访问权时，将从卷中删除主机连接。操作系统和应用程序无法再与该卷通信。

为了避免主机端问题，请在撤销访问权之前，先从操作系统中卸装存储卷，以避免发生驱动器缺失或数据损坏的情况。
{:important}

可以在“设备列表”或“存储”视图中撤销对任何存储器的访问权。

### 通过设备列表撤销访问权

1. 在 [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} 中，单击**设备** > **设备列表**。
2. 双击相应的设备。
3. 选择**存储**选项卡。
4. 这将向您显示此特定主机有权访问的存储卷的列表，全部按存储类型（块、文件或其他）进行分组。选择要从中撤销访问权的卷旁边的相应**操作**菜单，然后单击**撤销访问权**。
5. 确认是否要撤销对卷的访问权，因为该操作无法撤销。单击**是**以撤销卷访问权，或单击**否**以取消该操作。

如果要断开一个特定主机与多个卷的连接，需要对每个卷重复“撤销访问权”操作。
{:tip}


### 通过存储视图撤销访问权

1. 单击**存储 > {{site.data.keyword.filestorage_short}}**，然后选择要从中撤销访问权的**卷**。
2. 滚动到页面的**已授权主机**部分。
3. 单击要撤销其访问权的主机旁边的**操作**，然后选择**撤销访问权**。
4. 确认是否要撤销对卷的访问权，因为该操作无法撤销。单击**是**以撤销卷访问权，或单击**否**以取消该操作。

如果要断开一个特定卷与多个主机的连接，需要对每个主机重复“撤销访问权”操作。
{:tip}


## 取消存储卷

如果不再需要特定卷，可以将其取消。要取消存储卷，首先需要撤销所有主机对该卷的访问权。

1. 单击**存储** > **{{site.data.keyword.filestorage_short}}**。
2. 单击要取消的卷的**操作**，然后选择**取消 {{site.data.keyword.filestorage_short}}**。
3. 确认是要立即取消卷，还是在供应卷的周年日期取消。

   如果选择在卷的周年日期取消卷的选项，那么可以在卷的周年日期之前使取消请求失效。
   {:tip}
4. 单击**继续**或**关闭**。

5. 单击“确认”复选框，然后单击**确认**。
