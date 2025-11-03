---

copyright:
  years: 2014, 2025
lastupdated: "2025-11-03"

keywords: File Storage for Classic, NFS, security, encryption

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Securing your data in {{site.data.keyword.filestorage_short}}
{: #mng-data}

{{site.data.keyword.cloud}} prioritizes security and understands the critical need to encrypt data for protection. For {{site.data.keyword.filestorage_full}} that is provisioned with either Endurance or Performance options, provider-managed encryption is enabled by default, at no extra cost and with no performance impact.
{: shortdesc}

The provider-managed encryption-at-rest feature uses the following industry standard protocols:

* AES-256 encryption, an industry-standard encryption method.
* Key Management Interoperability Protocol (KMIP) is used for in-house key management.
* Compliance validations include US Federal Information Processing Standard (FIPS) Publication 140-2, Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA). Storage is also validated for Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386), and the EU General Data Protection Regulation (GDPR).

## Securing your snapshots or replicated storage
{: #securesnapshot}

All snapshots and replicas of encrypted File Storage are also encrypted by default. This feature can’t be turned off on a volume basis. All cluster-to-cluster traffic is encrypted with TLS.

## Provisioning storage with encryption
{: #encryptvolume}

The provider-managed encryption-at-rest feature is available in all [data centers](/docs/overview?topic=overview-locations#data-centers). All storage that is ordered in these data centers is automatically provisioned with encryption for data-at-rest.

Any nonencrypted storage that was provisioned before a data center upgrade is **not** automatically encrypted. If you own nonencrypted storage in an upgraded data center and you want to have it encrypted, you need to create a volume and move your data. For more information, see [File Storage Migration in Upgraded Data Centers](/docs/FileStorage?topic=FileStorage-migratestorage).
{: important}

## Deleting {{site.data.keyword.filestorage_short}} instances
{: #delfilevol}

If you no longer need a specific volume, you can delete it. {{site.data.keyword.filestorage_short}} presents file shares to customers on physical storage that is wiped before any reuse. For more information, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#deleted).

Customers with specific compliance needs, such as NIST 800-88 Guidelines for Media Sanitization, can complete the data sanitization procedure before they delete their storage.

To delete a storage volume, you must revoke access from any hosts first. Active replicas and dependent duplicates can also block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to cancel the original volume.
{: important}

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") for the volume to be canceled, and select **Delete {{site.data.keyword.filestorage_short}}**.
4. Confirm if you want to cancel the volume immediately or on the anniversary date of when the volume was provisioned.

   If you select the option to cancel the volume on its anniversary date, you can void the cancellation request before its anniversary date.
   {: tip}

5. Click **Continue**.
6. Click the acknowledgment checkbox, and click **Confirm**.

When the volume is deleted, a 24-hour reclaim wait period begins. You can still see the volume in the console during those 24 hours. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. However, billing for the volume stops immediately. For more information, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs).

After the Storage LUN is reclaimed, the disk is wiped, and data can't be restored.
When drives are decommissioned in a data center, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.
