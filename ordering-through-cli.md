---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ordering {{site.data.keyword.filestorage_short}} through the SL CLI

You can use the SL CLI to place orders for products that are normally ordered through the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}. In the SL API, an order can consist of multiple order containers. The order CLI works with one order container only.

For more information about how to install and use the SL CLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Searching for available {{site.data.keyword.filestorage_short}} offers

The first component to look for when you place an order is a package. Packages are divided among the different top-level products that are available for ordering in the {{site.data.keyword.BluSoftlayer_full}}. Some example packages are CLOUD_SERVER for VSIs, BARE_METAL_SERVER for bare metal servers, and STORAGE_AS_A_SERVICE_STAAS for {{site.data.keyword.filestorage_short}} and {{site.data.keyword.blockstorageshort}}.

Within a package, some items are subdivided into categories. Some packages have presets for your convenience and others require items to be specified individually. If a package's category is required, an item from that category must be chosen to order the package. Depending on the category, some items within the category can be mutually exclusive.

Each order must have an associated location (data center). When you order {{site.data.keyword.filestorage_short}}, make sure that it is provisioned in the same location as your compute instances.
{:important}

You can use the `slcli order package-list` command to find the package you want to order. A `–keyword` option is provided to perform simple searching and filtering. This option makes it easier to find the package you need. Look for `Storage-as-a-Service Package 759`.

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

  List packages that can be ordered via the placeOrder API.

  Example:
      # List out all packages for ordering
      slcli order package-list

  Keywords can also be used for some simple filtering functionality to help
  find a package easier.

  Example:
     # List out all packages with "server" in the name
      slcli order package-list --keyword server

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

You can also use the `slcli file volume-order` command.

```
# slcli file volume-order --help
Usage: slcli file volume-order [OPTIONS]

  Order a file storage volume.

Options:
  --storage-type [performance|endurance]
                                  Type of file storage volume  [required]
  --size INTEGER                  Size of file storage volume in GB
                                  [required]
  --iops INTEGER                  Performance Storage IOPs, between 100 and
                                  6000 in multiples of 100  [required for
                                  storage-type performance]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                  [required for storage-type endurance]
  --location TEXT                 Datacenter short name (e.g.: dal09)
                                  [required]
  --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                  space along with endurance file storage;
                                  specifies the size (in GB) of snapshot space
                                  to order
  --service-offering [storage_as_a_service|enterprise|performance]
                                  The service offering package to use for
                                  placing the order [optional, default is
                                  'storage_as_a_service']
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```

For more information about ordering {{site.data.keyword.filestorage_short}} through the API, see [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}.
To be able to access all the new features, order `Storage-as-a-Service Package 759`.
{:tip}


## Placing the order

The following example shows how to order a 10-GB {{site.data.keyword.filestorage_short}} volume with 100 IOPS per GB.

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes. To increase the number of your volumes, contact your sales representative. For more information about increasing limits, see [Managing Storage limits](managing-storage-limits.html).
{:important}

## Authorizing the hosts to access the new storage

```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

  Authorizes hosts to access a given volume

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to
                            authorize
  --help                    Show this message and exit.```

For more information about authorizing hosts to access the {{site.data.keyword.filestorage_short}} through the API, see [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

For more information about the simultaneous authorizations limit, see the [FAQs](faqs.html).
{:important}

## Connecting your new storage

Depending on your host's operating system, follow the appropriate link.
- [Mounting {{site.data.keyword.filestorage_short}} on Linux](accessing-file-storage-linux.html)
- [Mounting {{site.data.keyword.filestorage_short}} in CentOS](mounting-nsf-file-storage.html)
- [Mounting {{site.data.keyword.filestorage_short}} on Container Linux](mounting-storage-coreos.html)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with cPanel](configure-backup-cpanel.html)
- [Configuring {{site.data.keyword.filestorage_short}} for backup with Plesk](configure-backup-plesk.html)
- [Mounting {{site.data.keyword.filestorage_short}} Volume on ESXi hosts](architecture-guide-file-storage-vmware.html)
