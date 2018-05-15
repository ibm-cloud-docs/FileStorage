---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Managing {{site.data.keyword.filestorage_short}}

You can manager your {{site.data.keyword.filestorage_full}} volumes through {{site.data.keyword.slportal}}. This article provides instructions for the most common tasks.

## How to authorize hosts to access {{site.data.keyword.filestorage_short}}

“Authorized” hosts are hosts that have been given access rights to a particular volume. Without host authorization you won’t be able to access or use the storage from your system. Authorizing a host to access your volume generates theuser name and password. 

**Note**: You can only authorize and connect hosts that reside in the same datacenter as your storage. If you have multiple accounts, you can't authorize a host from one account to access your storage on another. 

1. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and click your **Volume Name**.
2. Scroll to the **Authorized Hosts** section of the page.
3. Click **Authorize Host** on the right side of the page. Select the hosts that can access that particular volume.

 

## How to view the list of hosts authorized to a {{site.data.keyword.filestorage_short}} volume

Use the following steps to view the list of hosts authorized to a volume.

1. Click **Storage > {{site.data.keyword.filestorage_short}}**, and click your **Volume Name**.
2. Scroll down to the bottom of the page to the **Authorized Hosts** section.

Here you'll see the list of hosts which are currently authorized to access the volume.


## How to view the {{site.data.keyword.filestorage_short}} volumes to which a host is authorized

You can view the volumes to which a host has access to, including information needed to make a connection – Volume Name, Storage Type, Target Address, capacity and location:

1. Click **Devices** > **Device List** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} and click the appropriate device.
2. Select the Storage tab.

You will then be presented with a list of storage volumes that this particular host has access to, all grouped by storage type (block, file, other). From the respective **Action** menus, you can authorize more storage or remove access.

 

## How to mount and unmount {{site.data.keyword.filestorage_short}}

You can use the mount point information provided in the **Volume Details** view to mount {{site.data.keyword.filestorage_short}} from a host.

Refer to the following article with details for mounting and unmounting {{site.data.keyword.filestorage_short}} from a host: [Accessing {{site.data.keyword.filestorage_short}} on Linux](accessing-file-storage-linux.html)

 

## How to revoke a host's access to {{site.data.keyword.filestorage_short}}

If you want to stop the access from a host to a particular storage volume, you can revoke the access. Upon revoking access, the host connection will be dropped from the volume and neither operating system nor applications will be able to communicate with the volume. 

**Note:** To avoid host side issues, unmount the storage volume from your operating system before revoking access to avoid missing drives or data corruption.

You can revoke access from either Storage from the Device List or the Storage views.

### How to Revoke access from the Device List:

1. Click **Devices** > **Device List** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} and double-click the appropriate device.
2. Select the **Storage** tab.
3. You are presented with a list of storage volumes that this particular host has access to, all grouped by storage type (block, file, other). Select the respective **Action** menu next to the volume that you want to revoke access from and click **Revoke Access**.
4. Confirm if you want to revoke the access for a volume because the action can't be undone. Click **Yes** to revoke volume access or **No** to cancel the action.

**Note:** If you want to disconnect multiple volumes from a specific host, you will need to repeat the Revoke Access action for each volume.

 

### How to Revoke access from the Storage View:
1. Click **Storage, {{site.data.keyword.filestorage_short}}**, and select the **Volume** from which you want to revoke access.
2. Scroll down to the **Authorized Hosts** section of the page.
3. Click **Actions** next to the host whose access is to be revoked, and select **Revoke Access**.
4. Confirm if you want to revoke the access for a volume because the action cannot be undone. Click **Yes** to revoke volume access or **No** to cancel the action.

**Note:** If you want to disconnect multiple hosts from a specific volume, you need to repeat the Revoke Access action for each host.

 

## How to Cancel a storage volume

If you no longer need a specific volume, you can cancel it. In order to cancel a storage volume, it's first necessary to revoke access from any hosts.

1. Click **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Click **Actions** for the volume to be canceled and select **Cancel {{site.data.keyword.filestorage_short}}**.
3. Confirm if want to cancel the volume immediately or on the yearly anniversary date of when the volume was provisioned. Click **Continue** or **Close**. 
**Note**: If you select the option to cancel the volume on its anniversary date, you can void the cancellation request before its anniversary date.
4. Click the acknowledgment check box and click **Confirm**.

 

## How to see a provisioned {{site.data.keyword.filestorage_short}} volume’s details

You can view a summary the key information for the selected storage volume including more snapshot and replication capabilities that have been added to the storage.

1. Click **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Click the appropriate **Volume Name** from the list.
