---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-15"

---
{:new_window: target="_blank"}

# New Locations and Features of {{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} is introducing a new version of {{site.data.keyword.filestorage_full}}! The new storage is available in select data centers, and is backed by flash storage at higher IOPS levels with disk-level encryption for data-at-rest. All storage that is ordered in the select data centers is automatically created with the new version of {{site.data.keyword.filestorage_short}}.

**Note:** The NFS mount point for new volumes changed. See **New mount point for enhanced {{site.data.keyword.filestorage_short}} volumes** section for details.

The new {{site.data.keyword.filestorage_short}} is available in following regions/data centers with more data center availability added later!

<table role="presentation">
  <tr>
    <td><strong>US 2</strong></td>
    <td><strong>EU</strong></td>
    <td><strong>Australia</strong></td>
    <td><strong>Canada</strong></td>
    <td><strong>Latin America</strong></td>
    <td><strong>Asia Pacific</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*Table 1 shows our Data Center Availability. Each region has its own column. Some cities, such as Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London and Sydney have multiple data centers.*

The new storage has the following features and capabilities:

- [Provider-managed encryption for data-at-rest](block-file-storage-encryption-rest.html). <br/> All {{site.data.keyword.filestorage_short}} volumes are automatically provisioned as encrypted at no additional charge.
- 10 IOPS per GB tier option. <br/> A new tier was added to the Endurance type {{site.data.keyword.filestorage_short}} to support the most demanding workloads.
- All flash-backed storage. <br/> {{site.data.keyword.filestorage_short}} that is provisioned with either Endurance or Performance options at 2 IOPS per GB or higher is backed by all-flash storage.
- Snapshot and Replication support.
- Hourly Billing option added for storage that is planned to be used for less than a full month.
- Up to 48,000 IOPS for {{site.data.keyword.filestorage_short}} provisioned with the Performance type.
- IOPS rates are adjustable to improve performance of seasonal load changes. Read more about this feature [here](adjustable-iops.html).
- Create a clone of your data with the [{{site.data.keyword.filestorage_short}} Volume Duplication feature](how-to-create-duplicate-volume.html).
- Storage is expandable in GB increments up to 12 TB immediately, without the need to create a duplicate or manually moving data to a larger volume. Read more about this feature [here](expandable_file_storage.html).

## New Mount Point for enhanced {{site.data.keyword.filestorage_short}} volumes

All enhanced {{site.data.keyword.filestorage_short}} volumes that are provisioned in these data centers have a different mount point than non-encrypted volumes. To ensure you're using the correct mount point for both your storage volumes, you can view the mount point information in the **Volume Details** page in the UI. You can also access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Check back here to see when more data centers are upgraded and for new features and capabilities that are being added for {{site.data.keyword.filestorage_short}}.
