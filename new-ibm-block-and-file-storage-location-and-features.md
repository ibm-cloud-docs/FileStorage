---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# New Locations and Features of {{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} is introducing a new version of {{site.data.keyword.filestorage_full}}! The new storage is available in select data centers, and is backed by flash storage at higher IOPS levels with disk level encryption for data-at-rest. All storage provisioned in the select data centers are automatically provisioned with the new version of {{site.data.keyword.filestorage_short}}.

**Note:** The NFS mount point for new volumes has changed. See **New Mount Point for encrypted {{site.data.keyword.filestorage_short}} Volumes** section for details.

The new {{site.data.keyword.filestorage_short}} is available in following regions/data centers with additional data center availability added later!

<table style="width:100%;">
	<caption>Data Center Availability</caption>
	<tbody>
		<tr>
			<td><strong>US 2</strong></td>
			<td><strong>EU</strong></td>
			<td><strong>Australia</strong></td>
			<td><strong>Canada</strong></td>
			<td><strong>Latin America</strong></td>
			<td><strong>Asia Pacific</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br /><br /></p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				FRA04<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
				SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


The new storage has the following features and capabilities:

- [Provider-managed encryption for data-at-rest](block-file-storage-encryption-rest.html). All {{site.data.keyword.filestorage_short}} volumes are automatically provisioned as encrypted at no additional charge.
- 10 IOPS per GB tier option. A new tier has been added to the Endurance type {{site.data.keyword.filestorage_short}} to support the most demanding workloads.
- All flash-backed storage. {{site.data.keyword.filestorage_short}} is provisioned with either Endurance or Performance options at 2 IOPS per GB or higher are backed by all-flash storage.
- Snapshot and Replication support for {{site.data.keyword.filestorage_short}} provisioned with either Endurance or Performance options.
- Hourly Billing option added for storage that is planned to be used for less than a full month. 
- Up to 48,000 IOPS for {{site.data.keyword.filestorage_short}} provisioned with the Performance type.
- IOPS rates are adjustable to improve performance in case of seasonal load changes. Read more about this feature [here](adjustable-iops.html).
- Create a new clone of your data with the [{{site.data.keyword.filestorage_short}} Volume Duplication feature](how-to-create-duplicate-volume.html).
- Storage is expandable in GB increments up to 12 TB immediately, without the need to create a duplicate or manually migrate data to a larger volume. Read more about this feature [here](expandable_file_storage.html).

## New Mount Point for Encrypted {{site.data.keyword.filestorage_short}} Volumes

All encrypted {{site.data.keyword.filestorage_short}} volumes provisioned in these data centers have a different mount point than non-encrypted volumes.  To ensure you're using the correct mount point for both your encrypted and non-encrypted file storage volumes, you can view the mount point information in the **Volume Details** page in the UI and access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Check back here to see when additional data centers have been upgraded and for new features and capabilities that are being added for {{site.data.keyword.filestorage_short}}.
