---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ordering and Managing {{site.data.keyword.filestorage_full_notm}}

There are two different types of storage you can provision based on your needs and preferences. The two options of {{site.data.keyword.filestorage_short}} volumes are:

Endurance: provision Endurance tiers featuring pre-defined performance levels and features like snapshots and replication.
Performance: build a high powered Performance environment where you can allocate the desired input/output operations per second (IOPS).

## Provisioning {{site.data.keyword.filestorage_short}}

### How to order Endurance for {{site.data.keyword.filestorage_short}}

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} Catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Click **Order {{site.data.keyword.filestorage_short}}** in the top right corner of the screen. 
3. Select **Endurance** from the **Select Storage Type** drop-down list.
4. Click the **Location** drop-down list and select your data center.
   - Ensure that the new Storage will be added in the same location as the previously ordered host(s).
   - If you selected a data center with improved capabilities (denoted with an * in the drop-down), you will have the option to choose between Monthly or Hourly Billing. 
     1. With **hourly** billing, the calculation of the number of hours the file volume existed on the account is performed at the time the volume is deleted or at the end of the billing cycle, which ever comes first.  Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There is no refund If a file volume is deleted before the end of the billing cycle.  Monthly billing is a good choice for storage used in production workloads that use data that needs to be stored and accessed for long periods of time (a month or longer).
     **NOTE**: Monthly billing type is used by default for storage provisioned in data centers that are not updated with improved capabilities.
5. Select the storage level for your applications:
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a given time. Example applications include storing mailboxes or departmental level file shares.
    - **2 IOPS per GB** is designed for most general purpose usage. Example applications include hosting small databases backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a given time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL Databases, and data processing for Analytics.  This tier is available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html) for storage provisioned up to 4TB in size.
6. Select the **Usable Storage Size** from the drop-down list.
7. Choose the **Snapshot Space Size** (in addition to your usable space) from the drop-down list.
8. Click** Continue**. You’re shown the monthly and prorated charges with a final chance to review order details.
9. Click the **I have read the Master Service Agreement** checkbox and click **Place Order**.
10. Your new storage allocation should be available in a few minutes.

**Note**: By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes please contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).

For the limit on simultaneous authorizations please see our [FAQs](File-Storage-FAQ.html)

### How to order Performance for {{site.data.keyword.filestorage_short}}

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage**, **{{site.data.keyword.filestorage_short}}** OR from {{site.data.keyword.BluSoftlayer_full}} Catalog click **Infrastructure** >** Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Click **Order {{site.data.keyword.filestorage_short}}** in the top right corner of the screen. 
3. Select **Performance** from the Select Storage Type drop-down list.
4. Click the **Location** drop-down list and select your data center.
    -  Ensure that the new Storage will be added in the same location as the previously ordered host(s).
    -  If you selected a data center with improved capabilities (denoted with an * in the drop-down), you will have the option to choose between Monthly or Hourly Billing. 
       1.  With **hourly** billing, the calculation of the number of hours the file volume existed on the account is performed at the time the volume is deleted or at the end of the billing cycle, which ever comes first.  Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage provisioned in these select data centers. 
       2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There is no refund If a file volume is deleted before the end of the billing cycle.  Monthly billing is a good choice for storage used in production workloads that use data that needs to be stored and accessed for long periods of time (a month or longer).
       **NOTE**: Monthly billing type is used by default for storage provisioned in data centers that are not updated with improved capabilities.  
5. Select the radio button next to the appropriate **Storage Size**.
6. Enter the IOPS in the **Specify IOPS** field.
7. Click **Continue**. You are shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
8. Click the **I have read the Master Service Agreement** checkbox and click **Place Order**.
9. Your new storage allocation should be available in a few minutes.

**Note**: By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes please contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).

For the limit on simultaneous authorizations please see our [FAQs](File-Storage-FAQ.html)

## Managing {{site.data.keyword.filestorage_short}}

### How to authorize hosts to access {{site.data.keyword.filestorage_short}}

“Authorized” hosts are hosts that have been given access rights to a particular volume. Without host authorization you won’t be able to access or use the storage from your system. Authorizing a host to access your volume generates the Username, Password. 

**Note**: You can only authorize and connect hosts that reside in the same datacenter as your storage. If you have multiple accounts, you cannot authorize a host from one account to access your storage on another. 

1. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and click on your **Volume Name**.
2. Scroll to the **Authorized Hosts** section of the page.
3. Click the **Authorize Host** link on the right side of the page. Select the hosts that can access that particular volume.

 

### How to view the list of hosts authorized to a {{site.data.keyword.filestorage_short}} volume

Use the following steps to view the list of hosts authorized to a volume.

1. Click **Storage > {{site.data.keyword.filestorage_short}}**, and click on your **Volume Name**.
2. Scroll down to the bottom of the page to the **Authorized Hosts** section.

Here you will see the list of hosts which are currently authorized to access the volume.


### How to view the {{site.data.keyword.filestorage_short}} volumes to which a host is authorized

You can view the volumes to which a host has access to, including information needed to make a connection – Volume Name, Storage Type, Target Address, capacity and location:

1. Click **Devices** > **Device List** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} and click the appropriate device.
2. Select the Storage tab.

You will then be presented with a list of storage volumes that this particular host has access to, all grouped by storage type (block, file, other). From the respective **Action** menus you can authorize additional storage or remove access.

 

### How to mount and unmount {{site.data.keyword.filestorage_short}}

You can use the mount point information provided in the **Volume Details** view to mount {{site.data.keyword.filestorage_short}} from a host.

Refer to the following article with details for mounting and unmounting {{site.data.keyword.filestorage_short}} from a host: [Accessing {{site.data.keyword.filestorage_short}} on Linux](accessing-file-storage-linux.html)

 

### How to revoke a host's access to {{site.data.keyword.filestorage_short}}

If you want to stop the access from a host to a particular storage volume you can revoke the access. Upon revoking access, the host connection will be dropped from the volume and neither operating system nor applications will be able to communicate with the volume. 

**Note:** To avoid host side issues, unmount the storage volume from your operating system before revoking access to avoid missing drives or data corruption.

You can revoke access from either Storage from the Device List or the Storage views.

#### How to Revoke access from the Device List:

1. Click **Devices** > **Device List** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} and double-click the appropriate device.
2. Select the **Storage** tab.
3. You will then be presented with a list of storage volumes that this particular host has access to, all grouped by storage type (block, file, other). Select the respective **Action** menu next to the volume that you want to revoke access from and click **Revoke Access**.
4. You will be asked if you want to revoke the access for a volume because the action cannot be undone. Click **Yes** to revoke volume access or **No** to cancel the action.

**Note:** If you want to disconnect multiple volumes from a specific host, you will need to repeat the Revoke Access action for each volume.

 

#### How to Revoke access from the Storage View:
1. Click **Storage, {{site.data.keyword.filestorage_short}}**, and select the **Volume** from which you want to revoke access.
2. Scroll down to the **Authorized Hosts** section of the page.
3. Click the **Actions** drop-down arrow next to the host whose access is to be revoked, and select **Revoke Access**.
4. You will be asked if you want to revoke the access for a volume because the action cannot be undone. Click **Yes** to revoke volume access or **No** to cancel the action.

**Note:** If you want to disconnect multiple hosts from a specific volume, you will need to repeat the Revoke Access action for each host.

 

### How to Cancel a storage volume

If you no longer need a specific volume, you can cancel it. In order to cancel a storage volume, it's first necessary to revoke access from any hosts.

1. Click **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Click the **Actions** drop-down arrow for the volume to be cancelled and select **Cancel {{site.data.keyword.filestorage_short}}**.
3. You will be asked to confirm if want to cancel the volume immediately or on the anniversary date of when the volume was provisioned. Click **Continue** or **Close**. 
**Note**: If you select the option to cancel the volume on its anniversary date, you can void the cancellation request prior to its anniversary date.
4. Click the acknowledgment checkbox and click **Confirm**.

 

### How to See a provisioned {{site.data.keyword.filestorage_short}} volume’s details

You can view a summary the key information for the selected storage volume including additional snapshot and replication capabilities that have been added to the storage.

1. Click **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Click the appropriate **Volume Name** from the list.

### How to Identify my {{site.data.keyword.filestorage_short}} volumes on my invoice

All volumes will appear on your invoice as a line item; Endurance will appear as “Endurance Storage Service” and Performance will appear as "Performance Storage service". The rate will vary based on your storage level. You can expand on Endurance or Performance to see that it is a {{site.data.keyword.filestorage_short}} volume.
