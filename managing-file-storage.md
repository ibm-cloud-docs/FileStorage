---

copyright:
  years: 2014, 2021
lastupdated: "2021-05-03"

keywords: File Storage, file storage, NFS, authorizing hosts, revoke access, grant access, view authorizations

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}


# Managing {{site.data.keyword.filestorage_short}}
{: #managingstorage}

You can manage your {{site.data.keyword.filestorage_full}} volumes through the {{site.data.keyword.cloud}} console and from the CLI.
{: shortdesc}

## Authorizing hosts to access {{site.data.keyword.filestorage_short}} in the UI
{: #authhostUI}
{: help}
{: support}
{: ui}

“Authorized” hosts are hosts that were given access to a particular volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the user name and password.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{: important}

1. Go to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}. From the menu, select **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and click your **Volume Name**.
3. Scroll to the **Authorized Hosts** section of the page.
4. Click **Authorize Host** on the right.
5. Filter the available host list by selecting the device type, subnet, or IP address.

   When the list is filtered by subnet, the subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {: note}

6. Select one or more hosts from the list and click **Save**.

## Authorizing hosts to access {{site.data.keyword.filestorage_short}} from the SLCLI
{: #authhostCLI}
{: help}
{: support}
{: cli}

To authorize a host to access the volume, you can use the following command.
```python
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  -s, --subnet-id TEXT      An ID of one subnet to authorize.
  --help                    Show this message and exit.
```

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.filestorage_short}} volume in the UI
{: #viewhostUI}
{: help}
{: support}
{: ui}

1. Go to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}. From the menu, select **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and click your **Volume Name**.
3. Click **Authorized Hosts** to display the compute instances that have access to your file share.

There you can see the list of hosts, which are currently authorized to access the volume.

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.filestorage_short}} volume from the SLCLI
{: #viewhostCLI}
{: help}
{: support}
{: cli}

To display the lost of authorized hosts, you can use the following command.
```python
# slcli file access-list --help
Usage: slcli file access-list [OPTIONS] VOLUME_ID

Options:
 --sortby TEXT   Column to sort by
 --columns TEXT  Columns to display. Options: id, name, type,
                 private_ip_address, source_subnet, host_iqn, username,
                 password, allowed_host_id
 -h, --help      Show this message and exit.
```


## Viewing the {{site.data.keyword.filestorage_short}} volumes to which a host is authorized in the UI
{: #viewvolUI}
{: help}
{: support}
{: ui}

You can view the volumes to which a host has access to, including information that is needed to make a connection – Volume Name, Storage Type, Target Address, capacity, and location.

1. Go to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}.
2. From the menu, select **Classic Infrastructure**.
3. Click **Devices** > **Device List**.
4. Click the appropriate device.
5. Select the Storage tab.

You are presented with a list of storage volumes that this particular host has access to, all are grouped by storage type (block, file, other). From the respective **Action** menus, you can authorize more storage or remove access.

## Revoking a host's access to {{site.data.keyword.filestorage_short}} in the UI
{: #revokehostaccessinUI}
{: ui}

If you want to stop the access from a host to a particular storage volume, you can revoke the access. When access is revoked, the host connection is dropped from the volume. The operating system and applications can't communicate with the volume anymore.

To avoid host side issues, unmount the storage volume from your operating system before you revoke access to avoid missing drives or data corruption.
{: important}

You can revoke access from either Storage from the Device List or the Storage views.

### Revoking access from the Device List in the UI
{: #revokeauthDeviceUI}
{: help}
{: support}
{: ui}

1. Go to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}.
2. From the menu, select **Classic Infrastructure**.
3. Click **Devices** > **Device List**.
4. Double-click the appropriate device.
5. Select the **Storage** tab.
6. You are presented with a list of storage volumes that this particular host has access to, all grouped by storage type (block, file, other). Select the respective **Action** menu next to the volume that you want to revoke access from, and click **Revoke Access**.
7. Confirm if you want to revoke the access for a volume because the action can't be undone. Click **Yes** to revoke volume access, or **No** to cancel the action.

If you want to disconnect multiple volumes from a specific host, you need to repeat the Revoke Access action for each volume.
{: tip}


### Revoking access from the Storage View in the UI
{: #revokeauthStorageUI}
{: help}
{: support}
{: ui}

1. Go to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}.
2. From the menu, select **Classic Infrastructure**.
3. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and select the **Volume** from which you want to revoke access.
4. Click **Authorized Hosts** to display the compute instances that have access to your File share.
5. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the host whose access is to be revoked, and select **Revoke Access**.
6. Confirm if you want to revoke the access for a volume because the action cannot be undone. Click **Yes** to revoke volume access, or **No** to cancel the action.

If you want to disconnect multiple hosts from a specific volume, you need to repeat the Revoke Access action for each host.
{: tip}

## Revoking a host's access to {{site.data.keyword.filestorage_short}} from the SLCLI
{: #revokeauthslcli}
{: help}
{: support}
{: cli}

You can use the following command in SLCLI.
```python
# slcli file access-revoke --help
Usage: slcli file access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of one virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of one IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  -s, --subnet-id TEXT      An ID of one subnet to revoke authorization.
  --help                    Show this message and exit.
```

If you want to disconnect multiple hosts from a specific volume, you need to repeat the Revoke Access action for each host.
{: tip}

## Deleting a storage volume in the UI
{: #cancelvolUI}
{: help}
{: support}
{: ui}

If you no longer need a specific volume, you can delete that file share. To cancel a storage volume, you need to revoke access from any hosts first.

1. Go to the [{{site.data.keyword.cloud}} console](https://{DomainName}/){: external}. From the menu, select **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") for the volume to be canceled, and select **Delete {{site.data.keyword.filestorage_short}}**.
4. Confirm if want to delete the volume immediately or on the anniversary date of when the volume was provisioned.

   If you select the option to cancel the volume on its anniversary date, you can void the cancellation request before its anniversary date.
   {: tip}

5. Click **Continue** or **Close**.
6. Click the acknowledgment check box, and click **Confirm**.

When the volume is canceled, there's a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs).
{: note}

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to delete the original volume.


## Deleting a storage volume from the SLCLI
{: #cancelvolCLI}
{: help}
{: support}
{: cli}

To delete a storage volume, you can use the following command.
```python
# slcli file volume-cancel --help
Usage: slcli file volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation.
  --immediate    Cancels the file storage volume immediately instead of on the
                 billing anniversary.
  -h, --help     Show this message and exit.
```

When the volume is canceled, there's a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs).
{: note}

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to delete the original volume.
