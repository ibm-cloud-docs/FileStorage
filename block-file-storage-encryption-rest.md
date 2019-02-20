---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Provider-managed encryption-at-rest
{: #encryption}

{{site.data.keyword.BluSoftlayer_full}} takes the need for security seriously, and understands the importance of being able to encrypt data to keep it safe. With provider-managed encryption, {{site.data.keyword.filestorage_full}} that is provisioned with either Endurance or Performance options, is encrypted by default at no additional cost and no impact on performance.

The provider-managed encryption-at-rest feature uses the following industry standard protocols:

* Industry-Standard AES-256 encryption
* Keys are managed in-house with industry standard Key Management Interoperability Protocol (KMIP)
* Storage is validated against the following standards:
    - Federal Information Processing Standard (FIPS) Publication 140-2,
    - Federal Information Security Management Act (FISMA),
    - Health Insurance Portability and Accountability Act (HIPAA),
    - Payment Card Industry (PCI),
    - Basel II,
    - California Security Breach Information Act (SB 1386), and
    - EU Data Protection Directive (95/46/EC) compliance.

## Securing your snapshots or replicated storage  

All snapshots and replicas of encrypted file storage are also encrypted by default. This feature can’t be turned off on a volume basis.

## Provisioning storage with encryption

The provider-managed encryption-at-rest feature is available in select data centers. All storage that is ordered in these data centers is automatically provisioned with encryption for data-at-rest. Click [here](/docs/infrastructure/FileStorage?topic=FileStorage-news) to see the current list of data centers where {{site.data.keyword.filestorage_short}} encryption is available.

When you order {{site.data.keyword.filestorage_short}}, select a data center that is marked with an asterisk (`*`). You can see a lock icon to the right of the LUN/Volume Name field that indicates that the volume is encrypted. See Figure 1.

![The lock icon indicates that the LUN is encrypted](/images/encryptedstorage.png)
<caption>Figure 1. Example of the lock icon that indicates that the volume is encrypted.</caption>

Any non-encrypted storage that was provisioned before a data center upgrade is **not** automatically encrypted. If you own non-encrypted storage in an upgraded data center and you want to have it encrypted, you need to create a new volume, and move your data. For more information, see [File Storage Migration in Upgraded Data Centers](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage)
{:important}
