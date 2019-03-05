---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, authorizing hosts, rewoke access, grant access, view authorizations

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Managing {{site.data.keyword.filestorage_short}}
{: #managingstorage}

You can manage your {{site.data.keyword.filestorage_full}} volumes through {{site.data.keyword.slportal}}.

## Authorizing hosts to access {{site.data.keyword.filestorage_short}}

“Authorized” hosts are hosts that were given access to a particular volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the user name and password.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{:important}

1. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and click your **Volume Name**.
2. Scroll to the **Authorized Hosts** section of the page.
3. Click **Authorize Host** on the right. Select the hosts that can access that particular volume.

Alternatively, you can use the following command in SLCLI.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to
                            authorize
  --help                    Show this message and exit.
```

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.filestorage_short}} volume

1. Click **Storage > {{site.data.keyword.filestorage_short}}**, and click your **Volume Name**.
2. Scroll down the page to the **Authorized Hosts** section.

There you can see the list of hosts, which are currently authorized to access the volume.

Alternatively, you can use the following command in SLCLI.
```
# slcli file access-list --help
Usage: slcli file access-list [OPTIONS] VOLUME_ID

Options:
 --sortby TEXT   Column to sort by
 --columns TEXT  Columns to display. Options: id, name, type,
                 private_ip_address, source_subnet, host_iqn, username,
                 password, allowed_host_id
 -h, --help      Show this message and exit.
```


## Viewing the {{site.data.keyword.filestorage_short}} volumes to which a host is authorized

You can view the volumes to which a host has access to, including information that is needed to make a connection – Volume Name, Storage Type, Target Address, capacity, and location.

1. Click **Devices** > **Device List** from the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}
2. Click the appropriate device.
2. Select the Storage tab.

You are presented with a list of storage volumes that this particular host has access to, all are grouped by storage type (block, file, other). From the respective **Action** menus, you can authorize more storage or remove access.


## Mounting and unmounting {{site.data.keyword.filestorage_short}}

You can use the mount point information that is provided in the **Volume Details** view to mount {{site.data.keyword.filestorage_short}} from a host. See [Accessing {{site.data.keyword.filestorage_short}} on Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)


## Revoking a host's access to {{site.data.keyword.filestorage_short}}

If you want to stop the access from a host to a particular storage volume, you can revoke the access. When access is revoked, the host connection is dropped from the volume. The operating system and applications cann't communicate with the volume anymore.

To avoid host side issues, unmount the storage volume from your operating system before you revoke access to avoid missing drives or data corruption.
{:important}

You can revoke access from either Storage from the Device List or the Storage views.

### Revoking access from the Device List

1. Click **Devices** > **Device List** from the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}
2. Double-click the appropriate device.
3. Select the **Storage** tab.
4. You are presented with a list of storage volumes that this particular host has access to, all grouped by storage type (block, file, other). Select the respective **Action** menu next to the volume that you want to revoke access from and click **Revoke Access**.
5. Confirm if you want to revoke the access for a volume because the action can't be undone. Click **Yes** to revoke volume access, or **No** to cancel the action.

If you want to disconnect multiple volumes from a specific host, you need to repeat the Revoke Access action for each volume.
{:tip}


### Revoking access from the Storage View

1. Click **Storage, {{site.data.keyword.filestorage_short}}**, and select the **Volume** from which you want to revoke access.
2. Scroll to the **Authorized Hosts** section of the page.
3. Click **Actions** next to the host whose access is to be revoked, and select **Revoke Access**.
4. Confirm if you want to revoke the access for a volume because the action cannot be undone. Click **Yes** to revoke volume access, or **No** to cancel the action.

If you want to disconnect multiple hosts from a specific volume, you need to repeat the Revoke Access action for each host.
{:tip}

### Revoking access through the SLCLI.
Alternatively, you can use the following command in SLCLI.
```
# slcli file access-revoke --help
Usage: slcli file access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to revoke
                            authorization
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to revoke
                            authorization
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to revoke authorization
  --ip-address TEXT         An IP address to revoke authorization
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to revoke
                            authorization
  --help                    Show this message and exit.
```

## Canceling a storage volume

If you no longer need a specific volume, you can cancel that storage. To cancel a storage volume, you need to revoke access from any hosts first.

1. Click **Storage**>**{{site.data.keyword.filestorage_short}}**.
2. Click **Actions** for the volume to be canceled, and select **Cancel {{site.data.keyword.filestorage_short}}**.
3. Confirm if want to cancel the volume immediately or on the yearly anniversary date of when the volume was provisioned.

   If you select the option to cancel the volume on its anniversary date, you can void the cancellation request before its anniversary date.
   {:tip}
4. Click **Continue** or **Close**.
5. Click the acknowledgment check box, and click **Confirm**.

Alternatively, you can use the following command in SLCLI.
```
# slcli file volume-cancel --help
Usage: slcli file volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the file storage volume immediately instead of on the
                 billing anniversary
  -h, --help     Show this message and exit.
```
