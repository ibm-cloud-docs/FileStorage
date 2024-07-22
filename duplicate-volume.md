---

copyright:
  years: 2017, 2024
lastupdated: "2024-07-22"

keywords: File Storage for Classic, NFS, duplicate volume

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Creating and managing duplicate volumes
{: #duplicatevolume}

You can create a duplicate of an existing {{site.data.keyword.filestorage_full}}. The duplicate volume inherits the capacity and performance options of the original volume by default. However, both attributes can be changed. The duplicate has a copy of the data up to the point-in-time of the snapshot that was used to create it. The duplicate volume can be dependent or independent from the original volume.
{: shortdesc}

If you are a Dedicated account user of {{site.data.keyword.containerlong}}, see your options for duplicating a volume in the [{{site.data.keyword.containerlong_notm}} Documentation](/docs/containers?topic=containers-file_storage#file_backup_restore).
{: tip}

Because the duplicate is based on the data in a point-in-time snapshot, snapshot space is required on the original volume before you can create a duplicate. For more information about snapshots and how to order snapshot space, see the [Snapshot Documentation](/docs/FileStorage?topic=FileStorage-snapshots).
{: important}

## Types of duplicate volumes
{: #duplicatetype}

### Independent duplicate
{: #independent}

Independent duplicates can be created from both **primary** and **replica** volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the duplicate volume is created in the same data center as the replica.

#### Common uses for an independent duplicate volume
{: #independent_usecase}

- **Gold copy**. Use a storage volume as a gold copy that you can create multiple instances from for various uses.
- **Data refreshes**. Create a copy of your production data to mount to your nonproduction environment for testing.
- **Development and Testing**. Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing.

### Dependent duplicate
{: #dependent}

Dependent duplicate volumes are created by using a snapshot from the primary volume. Replica volumes cannot be used to create or update dependent duplicate volumes.

#### Common uses for a dependent duplicate volume
{: #dependent_usecase}

- **Disaster Recovery Testing**. Create a duplicate of your source volume and compare it to the replica. By comparing the duplicate to the replica you can verify that the data that is being replicated is intact and can be used if a disaster occurs, without interrupting the replication.
- **Restore from Snapshot**. Restore data on the original volume with specific files and date from a snapshot without overwriting the entire original volume with the snapshot restore function.
- **Data refreshes**. Create a copy of your production data to mount to your nonproduction environment for testing.
- **Development and Testing**. Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing.

## Creating a duplicate from a specific volume in the console
{: #createdepduplicateUI}
{: ui}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Locate and click the volume name.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Duplicate Share**.
4. Select whether the duplicate is to be dependent or independent.
5. Choose your snapshot option.
    - Create a snapshot for the most recent data now.
    - Use the last snapshot that was taken previously.
6. Location and IOPS Profile remain the same as the original volume.
7. You can choose to provision the duplicate volume with hourly or monthly billing method. The billing type for the original volume is automatically selected, and you can change it if you want to.
8. You can specify a different IOPS or IOPS Tier for the new volume if you want to. The IOPS designation of the original volume is set by default. Available Performance and size combinations are displayed.
    - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection.
    - If your original volume is 2, 4, or 10 IOPS Endurance tier, you can move anywhere between those tiers for the new volume.
9. You can update the size of the new volume so that it's larger than the original. The size of the original volume is set by default.

   {{site.data.keyword.filestorage_short}} can be resized to 10 times the original size of the volume.
   {: tip}

10. You can update the snapshot space for the new volume to add more, less, or no snapshot space. The snapshot space of the original volume is set by default.
11. Check the box to confirm that you read and agreed to the terms, then click **Create** to place your order.

After you click **Create**, the order confirmation window appears. When you close the window, you return to the resources list. You can go back to your list of {{site.data.keyword.filestorage_short}} shares to click the newly provisioned duplicate. The share details section displays information such as Duplicate Type, a link to the parent share's details page and the name of the snapshot that was used to create the duplicate.

## Creating a duplicate from the CLI
{: #createindependentduplicateCLI}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Creating a duplicate from the IBMCLOUD CLI
{: #createindependentduplicateICCLI}

You can use the `ibmcloud sl file volume-duplicate` command to create a duplicate for your file share. The following example creates an independent duplicate of the file share `560382016`.

```sh
ibmcloud sl file volume-duplicate 560382016
This action will incur charges on your account. Continue?> y
OK
Order 110554892 was placed.
 > Storage as a Service
 > File Storage
 > 500 GBs
 > 4 IOPS per GB
 > 500 GB (Snapshot Space)

You may run 'ibmcloud sl file volume-list --order 110554892' to find this file volume after it is ready.
```
{: codeblock}

Your new duplicate is ready within minutes.

```sh
$ ibmcloud sl file volume-list --order 110554892
id          username             datacenter   storage_type             capacity_gb   bytes_used   IOPs   ip_addr                                 lunId   active_transactions   rep_partner_count   notes
560391190   SL02SEV1414935_269   dal09        endurance_file_storage   500           -            -      fsf-dal0902b-fz.service.softlayer.com   -       1                     0                   -
```
{: codeblock}

When you want to create a dependent duplicate of your volume, use the command with the `--dependent-duplicate` option. See the following example.

```sh
$ ibmcloud sl file volume-duplicate 560391190 --dependent-duplicate
This action will incur charges on your account. Continue?> y
OK
Order 110553472 was placed.
 > Storage as a Service
 > File Storage
 > 500 GBs
 > 4 IOPS per GB
 > 500 GB (Snapshot Space)

You may run 'ibmcloud sl file volume-list --order 110553472' to find this file volume after it is ready.
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file volume-duplicate](/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_duplicate){: external}.

### Creating a duplicate from the SLCLI
{: #createindependentduplicateSLCLI}

To create an **independent duplicate** {{site.data.keyword.filestorage_short}} volume, you can use the following command.

```sh
$ slcli file volume-duplicate --help
Usage: slcli file volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate file volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Minimum: [the size
                                  of the origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin file volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```
{: codeblock}

**Dependent duplicate** volumes can be ordered from the CLI, too, with the option `--dependent-duplicate TRUE`.

```python
slcli file volume-duplicate --dependent-duplicate TRUE <primary-vol-id>
```
{: pre}

For more information about available command options, see [`file volume-duplicate`](https://softlayer-python.readthedocs.io/en/latest/cli/file/#file-volume-duplicate){: external}.

## Creating a duplicate share with the API
{: #cloneinAPI}
{: api}

To order an **independent duplicate** {{site.data.keyword.blockstorageshort}} volume with the API, you can make a `POST /SoftLayer_Product_Order/placeOrder` call. The following REST API example creates an independent duplicate for an Endurance (IOPS tiers) volume.

- URL - `https://USERNAME:APIKEY@api.softlayer.com/rest/v3.1/SoftLayer_Product_Order/placeOrder`
- Type - POST
- Request body - 
   ```js
    {
       "parameters":[{
       "complexType": "SoftLayer_Container_Product_Order_Network_Storage_AsAService",
       "packageId": 531,
       "duplicateOriginVolumeId":<PrimaryId>,
       "isDependentDuplicateFlag": 1,
       "prices": [{"id": 12207}, {"id": 10723}, {"id": 10413}, {"id": 15749}, {"id":14043}],
       "quantity": 1,
       "location": 2,
       "volumeSize":23
       }]
   }
   ```
   {: codeblock}

To order a **dependent duplicate** for a Performance (custom IOPS) volume, make a `POST /SoftLayer_Product_Order/placeOrder` call like the following REST API example.

- URL - `https://USERNAME:APIKEY@api.softlayer.com/rest/v3.1/SoftLayer_Product_Order/placeOrder`
- Type - POST
- Request body - 
   ```js
   {
       "parameters":[{
       "complexType": "SoftLayer_Container_Product_Order_Network_Storage_AsAService",
       "packageId": 531,
       "duplicateOriginVolumeId":<PrimaryId>,
       "isDependentDuplicateFlag": 1,
       "prices": [{"id": 15751}, {"id": 19487}, {"id": 18983}, {"id": 15749}, {"id":14043}],
       "quantity": 1,
       "iops":456,
       "location": 2,
       "volumeSize":23
       }]
   }
   ```
   {: codeblock}

For more information about the API and the options, see the [API Reference](https://sldn.softlayer.com/reference/softlayerapi/){: external}.

## Managing your duplicate volume
{: #manageduplicate}

All duplicate volumes can be accessed by a host for read and write operations as soon as the volume is provisioned.

However, snapshots and replication of independent duplicate volumes aren't allowed until the data copy from the original to the duplicate is complete and the duplicate volume is fully independent. Depending on the size of the data, the separation process can take several hours. When it's complete, the duplicate can be managed and used as an independent volume.

While data is being copied from the original volume to the **independent** duplicate, you can see that the status indicator on the details page shows the duplication is in progress. During this time, you can attach to a host, and read and write to the volume, but you can't create snapshot schedules or refresh data from the original file share. When the separation process is complete, the new volume is independent from the original, and can be managed with snapshots and replication as normal. The independent duplicate can be manually refreshed by using a snapshot from the parent volume after the conversion is complete.

**Dependent** duplicates do not go through the separation process and can be refreshed manually at any time. The refresh process can be initiated from the CLI, with the API or in the console. Later, if you want to convert the dependent duplicate into an independent volume, you can initiate that process by using the UI, the API, or the CLI, too.

The dependent duplicate volume locks the original snapshot so the snapshot cannot be deleted while the dependent duplicate exists.
{: note}

## Updating data on the duplicate from the parent volume in the console
{: #refreshindependentvol_ui}
{: ui}

As time passes and the primary volume changes, the duplicate volume can be updated with these changes to reflect the current state through the refresh action. The refresh involves taking a snapshot of the primary volume and then updating the duplicate volume by using the data from that snapshot.

If the duplicate volume is independent, you can stop a running refresh operation and start a new one.
{: note}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Locate the duplicate volume and click its name to view the volume details.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Restore parent snapshot**.
4. From the list of snapshots, select the parent snapshot that holds the data that you want to restore to the duplicate volume. If the duplicate volume that you're refreshing is an independent volume, you can stop a running operation and force a new restore to start. If you want to force a current refresh process to stop, check the box before you proceed.

   Restoring data from a snapshot results in the loss of any data that was created or modified since the selected snapshot was taken. During the refresh transaction, the duplicate volume is disabled and must be remounted after the refresh is completed.
   {: attention}

5. Click **Yes** to start the refresh. The refresh can take a while to complete. The status bar shows the percentage of data that is copied to the volume. To see updated status, refresh the page in the browser.

## Converting a dependent volume to an independent duplicate in the console
{: #convertdependentvol_ui}
{: ui}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") menu, click **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Locate the duplicate volume and click its name to view the volume details.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Convert Dependent Duplicate**.
4. Check the box to confirm that you want to proceed with the conversion.
5. Click **Yes**.

The conversion process can take some time to complete. The bigger the volume is, the longer it takes to convert it. You can view the status of the process on the volume details page under the **Duplicate conversion status** header.

## Updating data on the duplicate from the parent volume from the CLI
{: #refreshindependentvol}
{: cli}

As time passes and the primary volume changes, the duplicate volume can be updated with these changes to reflect the current state through the refresh action. The refresh involves [taking a snapshot](/docs/FileStorage?topic=FileStorage-managingSnapshots&interface=cli#takemanualsnapshotCLI) of the primary volume and then updating the duplicate volume by using that snapshot.

A refresh incurs no downtime on the primary volume. However, during the refresh transaction, the duplicate volume is unavailable and must be remounted after the refresh is completed.
{: important}

The refresh process can be time-consuming. If you find that you have new data that you want to copy to the independent duplicate volume, you can issue the `file volume-refresh` command with the `--force-refresh` option to stop all ongoing and pending refresh transactions, and initiate a new refresh. 

The force refresh process works only on independent volumes.
{: note}

### Refreshing data on a dependent duplicate from the IBMCLOUD CLI
{: #refreshindependentvolICCLI}

You can use the `ibmcloud sl file volume-refresh` command to update the data on your duplicate volume with the data from a snapshot of the parent volume. The following example shows how to manually create a snapshot of the parent volume `560391190`, then use the new snapshot `560391944` to refresh the data on the file share `560391814`.

```sh
$ ibmcloud sl file snapshot-create 560391190
OK
New snapshot 560391944 was created.

$ ibmcloud sl file volume-refresh 560391814 560391944
OK
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file volume-refresh](/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_refresh){: external}.

### Refreshing data on a dependent duplicate from the SLCLI
{: #refreshindependentvolSLCLI}

Refreshes can be initiated by using the following command.

```sh
slcli file volume-refresh <duplicate-vol-id> <primary-snapshot-id>
```
{: pre}

For more information about available command options, see [`slcli file volume-refresh`](https://softlayer-python.readthedocs.io/en/latest/cli/file/#file-volume-refresh){: external}.

## Converting a dependent volume to an independent duplicate from the CLI
{: #convertdependentvol}
{: cli}

If you want to use the dependent volume as a stand-alone volume in the future, you can convert it to a normal, independent {{site.data.keyword.filestorage_short}} volume from the CLI. 

### Converting a dependent volume from the IBMCLOUD CLI
{: #convertdependentvolICCLI}

Use the `ibmcloud sl file volume-convert` command to convert a dependent duplicate to an independent file share. The following example shows how to convert the file share `560391814` to an independent volume.

```sh
$ ibmcloud sl file volume-convert 560391814
OK
```
{: codeblock}

The conversion process can take some time to complete. The bigger the volume is, the longer it takes to convert it. Use the following command to check on the progress.

```sh
ibmcloud sl file volume-convert-status 560391814
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file volume-convert](/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_convert){: external}.

### Converting a dependent volume from the SLCLI
{: #convertdependentvolSLCLI}

Use the following command.

```sh
slcli file volume-convert <dependent-vol-id>
```
{: pre}

The conversion process can take some time to complete. The bigger the volume is, the longer it takes to convert it. Use the following command to check on the progress.

```zsh
slcli file duplicate-convert-status <dependent-vol-id>
```
{: pre}

The following example shows the output that you can expect.
```zsh
slcli file duplicate-convert-status 370597202
Username            Active Conversion Start Timestamp   Completed Percentage
SL02SEVC307608_74   2022-06-13 14:59:17                 90
```
{: screen}

For more information about available command options, see [`file duplicate-convert-status`](https://softlayer-python.readthedocs.io/en/latest/cli/file/#file-duplicate-convert-status){: external}.

## Updating data on the duplicate from the parent volume with the API
{: #refreshindependentvol_api}
{: api}

As time passes and the primary volume changes, the duplicate volume can be updated with these changes to reflect the current state through the refresh action. The refresh involves taking a snapshot of the primary volume and then updating the duplicate volume by using the data from that snapshot. 

A refresh incurs no downtime on the primary volume. However, during the refresh transaction, the duplicate volume is disabled and must be remounted after the refresh is completed.
{: important}

The refresh process can be time-consuming. You might find that you have new data that you want to add to the duplicate before the running refresh is finished. If that's the case, you can make a second call to `refreshDuplicate` and specify the second `forceRefresh` parameter as `true` to stop all ongoing and pending refresh transactions, and initiate a new refresh. If the second parameter is set to `false` or it is not specified, the call fails if another refresh is already in progress.

The force refresh process works only on independent volumes.
{: note}

### REST API example
{: #refreshindependentvol_rest}

- URL - `https://USERNAME:APIKEY@api.softlayer.com/rest/v3.1/SoftLayer_Network_Storage/duplicateVolumeId/refreshDuplicate`
- Type - POST
- Request body - 
   ```js
   {
    "parameters": [primaryVolumeSnapshotId, true OR false]
   }
   ```
   {: codeblock}

### SOAP API example
{: #refreshindependentvol_soap}

- URL - `https://api.softlayer.com/soap/v3.1/SoftLayer_Network_Storage`
- Type - POST
- Request body - 
   ```js
   <?xml version="1.0" encoding="UTF-8"?>
   <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://api.service.softlayer.com/soap/v3.1/">
   <SOAP-ENV:Header>
    <ns1:authenticate>
     <username>USERNAME</username>
     <apiKey>APIKEY</apiKey>
    </ns1:authenticate>
    <ns2:SoftLayer_Network_StorageInitParameters>
     <id>duplicate Volume Id</id>
    </ns2:SoftLayer_Network_StorageInitParameters>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
    <ns1:refreshDuplicate>
      <snapshotId xsi:type="int">primary Volume Snapshot Id</snapshotId>
      <forceRefresh xsi:type="boolean">true</forceRefresh> <-- (remove this tag for normal refresh)
    </ns1:refreshDuplicate>
   </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```
   {: codeblock}

For more information about the API and the options, see the [API Reference](https://sldn.softlayer.com/reference/softlayerapi/){: external} and [`SoftLayer_Network_Storage::refreshDuplicate`](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage/refreshDuplicate/){: external}.

## Converting a dependent volume to an independent duplicate with the API
{: #convertdependentvol_api}
{: api}

If you want to use the dependent volume as a stand-alone volume in the future, you can convert it to a normal, independent {{site.data.keyword.blockstoragefull}} volume with the API. See the following example that uses the REST API.

- URL - `https://USERNAME:APIKEY@api.softlayer.com/rest/v3.1/SoftLayer_Network_Storage/<storageId>/convertCloneDependentToIndependent`
- Type - POST
- Request body - blank

For more information about the API and the options, see the [API Reference](https://sldn.softlayer.com/reference/softlayerapi/){: external}.

## Canceling a storage volume with a dependent duplicate
{: #cancelvolwithdependent}

Canceling a parent volume that has active dependent volumes requires canceling the dependent duplicate volumes first.
