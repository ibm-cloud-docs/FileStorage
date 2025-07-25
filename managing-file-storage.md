---

copyright:
  years: 2014, 2025
lastupdated: "2025-06-26"

keywords: File Storage for Classic, NFS, authorizing hosts, revoke access, grant access, view authorizations

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}


# Managing {{site.data.keyword.filestorage_short}}
{: #managingstorage}

You can manage your {{site.data.keyword.filestorage_full}} volumes through the {{site.data.keyword.cloud}} console and from the CLI.
{: shortdesc}

## Viewing the list of {{site.data.keyword.filestorage_short}} volumes in the console
{: #managestorage-view-ui}
{: ui}

You can view your volumes from the Resources list or by going to the list of {{site.data.keyword.filestorage_short}} volumes.

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**.

## Viewing the list of {{site.data.keyword.filestorage_short}} volumes from the CLI
{: #managestorage-view-cli}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Viewing the list of volumes from the IBMCLOUD CLI
{: #managestorage-view-iccli}
{: cli}

To get the list of your {{site.data.keyword.filestorage_short}} from the IBMCLOUD CLI, use the `ibmcloud sl file volume-list` command. The following example lists all endurance volumes on current account that are located in dal13, and sorts them by capacity.

```sh
ibmcloud sl file volume-list -d dal13 -t endurance --sortby capacity_gb
id          username          datacenter  storage_type            capacity_gb   bytes_used   IOPs   ip_addr   lunId active_transactions   rep_partner_count   notes
20973781    IBM02SEL1575811-1 dal13      endurance_file_storage   100           -            4      -         3 -                     0                   -
22030583    IBM02SEL1575811-3 dal13      endurance_file_storage   20            -            4      -         0 -                     0                   -
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file volume-list](/docs/cli?topic=cli-sl-file-storage-service&interface=cli#sl_file_volume_list){: external}.

### Viewing the list of volumes from the SL CLI
{: #managestorage-view-slcli}
{: cli}

To get the list of your {{site.data.keyword.filestorage_short}} from the SL CLI, use the `slcli file volume-list` command.

```sh
$ slcli file volume-list --help
Usage: slcli file volume-list [OPTIONS]

        List file storage.

Example::
        slcli file volume-list -d dal10 --storage-type endurance --sortby capacity_gb

┌────┬────────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ -u │ --username     │ Volume username                                                                                                                                                              │
│ -d │ --datacenter   │ Datacenter shortname                                                                                                                                                         │
│ -o │ --order        │ Filter by ID of the order that purchased the block storage                                                                                                                   │
│    │ --storage-type │ Type of storage volume Choices: performance, endurance                                                                                                                       │
│    │ --sortby       │ Column to sort by                                                                                                                                                            │
│    │ --columns      │ Columns to display. Options: id, username, datacenter, storage_type, capacity_gb, bytes_used, ip_addr, active_transactions, mount_addr, rep_partner_count, created_by, notes │
│ -h │ --help         │ Show this message and exit.                                                                                                                                                  │
└────┴────────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```
{: screen}

## Updating the notes of a volume in the console
{: #update-volume-notes-UI}
{: ui}

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Infrastructure** ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**.
3. Locate the volume that you want to update. Click the volume name to view the Volume details page.
4. Click the **Edit icon** ![Edit icon](../icons/edit-tagging.svg "Edit") next to **Notes** and enter your text.

## Updating the notes of a volume from the CLI
{: #update-volume-notes-CLI}
{: cli}

You can use the `ibmcloud sl call-api` command to add and modify notes for your volumes.

```sh
$ ibmcloud sl call-api --help
NAME:
  call-api - Call arbitrary API endpoints

USAGE:
  ibmcloud sl call-api SERVICE METHOD [OPTIONS]

OPTIONS:
  --filter value      Object filters
  -h, --help          Usage information.
  --init value        Init parameter
  --limit value       Result limit
  --mask value        Object mask: use to limit fields returned
  --offset value      Result offset
  --output value      Specify output format, only JSON is supported now.
  --parameters value  Append parameters to web call
```
{: screen}

Specify the volume ID in the `--init` option, and use the `--parameters` option to set the new note. See the following example:

```sh
ibmcloud sl call-api SoftLayer_Network_Storage editObject --init 562193766 --parameters '[{"notes":"Testing."}]'
```
{: screen}

For more information, see [ibmcloud sl call-api](/docs/cli?topic=cli-sl-all-api).

## Authorizing hosts to access {{site.data.keyword.filestorage_short}}
{: #managestorage-authhost}
{: help}
{: support}

“Authorized” hosts are hosts that are granted access to a particular volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the username and password.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{: important}

You can authorize up to 64 servers to access the file share. This limit includes all subnet, host, and IP authorizations combined. For more information about increasing this limit, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs#authlimit).

After the host is authorized, you can mount the file share and assign owners to your new folder structure and files. In Linux, you can refine access control by using the `chown` and `chmod` commands to assign read, write, and execute permissions to individual users and groups. For more information, see [Mounting {{site.data.keyword.filestorage_short}} on Red Hat Linux&reg;](/docs/FileStorage?topic=FileStorage-mountingLinux) and [Mounting {{site.data.keyword.filestorage_short}} on Ubuntu](/docs/FileStorage?topic=FileStorage-mountingUbuntu).

### Authorizing hosts to access {{site.data.keyword.filestorage_short}} in the console
{: #authhostUI}
{: help}
{: support}
{: ui}

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and click your **Volume Name**.
3. Scroll to the **Authorized Hosts** section of the page.
4. Click **Authorize Host** on the right.
5. Filter the available host list by selecting the device type, subnet, or IP address.

   When the list is filtered by subnet, the subnets that are displayed are subscribed subnets in the same data center as the storage volume.
   {: note}

6. Select one or more hosts from the list and click **Save**.

### Authorizing hosts to access {{site.data.keyword.filestorage_short}} from the CLI
{: #authhostCLI}
{: help}
{: support}
{: cli}

#### Authorizing hosts from the IBMCLOUD CLI
{: #authhostICCLI}
{: help}
{: support}
{: cli}

Use the `ibmcloud sl file access-authorize` command to authorize a host to access the file share. The following example authorizes the virtual server instance `87654321` to mount the file share `12345678`.

```sh
ibmcloud sl file access-authorize 12345678 --virtual-id 87654321
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file access-authorize](/docs/cli?topic=cli-sl-file-storage-service#sl_file_access_authorize){: external}.

#### Authorizing hosts from the SLCLI
{: #authhostSLCLI}
{: help}
{: support}
{: cli}

To authorize a host to access the volume, you can use the `slcli file access-authorize` command.
```sh
$ slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  -s, --subnet-id TEXT      An ID of one subnet to authorize.
  --help                    Show this message and exit.
```

### Authorizing the host with Terraform
{: #authhostterraform}
{: terraform}

To authorize a Compute host to access the share, use the `ibm_storage_file` resource and specify the `allowed_virtual_guest_ids` for virtual servers, or `allowed_hardware_ids` for bare metal servers. Specify `allowed_ip_addresses` to define which IP addresses have access to the storage. 

The following example defines that the Virtual Server with the ID `28961689` can access the volume from the `10.146.139.64/26` subnet, and `10.146.139.84` address.

```terraform
resource "ibm_storage_file" "fs_endurance" {
  type       = "Endurance"
  datacenter = "dal09"
  capacity   = 20
  iops       = 0.25

  allowed_virtual_guest_ids = ["28961689"]
  allowed_subnets           = ["10.146.139.64/26"]
  allowed_ip_addresses      = ["10.146.139.84"]
  snapshot_capacity         = 10
  hourly_billing            = true
}
```
{: codeblock}

After your storage resource is created, you can access the `hostname` and `volumename` attributes, which you can use to determine the mount target later. For example, a File Storage resource with the `hostname` argument set to `nfsdal0901a.service.softlayer.com` and the `volumename` argument set to `IBM01SV278685_7` has the mount point `nfsdal0901a.service.softlayer.com:-IBM01SV278685_7`.

For more information about the arguments and attributes, see [ibm_storage_file](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_file){: external}.

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.filestorage_short}} volume in the console
{: #viewhostUI}
{: help}
{: support}
{: ui}

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
1. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and click your **Volume Name**.
1. Click **Authorized Hosts** to display the Compute instances that have access to your file share.
1. Click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **View host details**. A side panel is displayed with details like device name, IP address, username, and password. The Access Control List section is also displayed. In the Access Control List section, you can add or remove subnets.
 
The Target address is listed on the **Storage Detail** page. For NFS, the Target address is described as a DNS name, and for iSCSI, it's the IP address of the Discover Target Portal.
{: tip}

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.filestorage_short}} volume from the CLI
{: #viewhostCLI}
{: help}
{: support}
{: cli}

### Viewing the list of authorized hosts from the IBMCLOUD CLI
{: #viewhostICCLI}

You can use the `ibmcloud sl file access-list` command to list the hosts that are authorized to access the file share. The following example lists the hosts that can mount the file share `12345678` and sorts them by their IDs.

```sh
ibmcloud sl file access-list 12345678 --sortby id
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file access-list](/docs/cli?topic=cli-sl-file-storage-service#sl_file_access_list){: external}.

### Viewing the list of authorized hosts from the SLCLI
{: #viewhostSLCLI}

To display the list of authorized hosts, you can use the following command.
```sh
$ slcli file access-list --help
Usage: slcli file access-list [OPTIONS] VOLUME_ID

Options:
 --sortby TEXT   Column to sort by
 --columns TEXT  Columns to display. Options: id, name, type,
                 private_ip_address, source_subnet, host_iqn, username,
                 password, allowed_host_id
 -h, --help      Show this message and exit.
```
{: screen}

## Viewing the {{site.data.keyword.filestorage_short}} volumes to which a host is authorized in the console
{: #viewvolUI}
{: help}
{: support}
{: ui}

You can view the volumes, which a host has access to, including information that is needed to make a connection – Volume Name, Storage Type, Target Address, capacity, and location.

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}.
2. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
3. Click **Devices** > **Device List**.
4. Click the appropriate device.
5. Select the Storage tab.

You are presented with a list of storage volumes that this particular host has access to, all are grouped by storage type (block, file, other). From the respective **Action** menus, you can authorize more storage or remove access.

## Revoking a host's access to {{site.data.keyword.filestorage_short}} in the console
{: #revokehostaccessinUI}
{: ui}

If you want to stop the access from a host to a particular storage volume, you can revoke the access. When access is revoked, the host connection is dropped from the volume. The operating system and applications can't communicate with the volume anymore.

To avoid host side issues, unmount the storage volume from your operating system before you revoke access to avoid missing drives or data corruption.
{: important}

You can revoke access from either Storage from the Device List or the Storage views.

### Revoking access from the Device List in the console
{: #revokeauthDeviceUI}
{: help}
{: support}
{: ui}

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}.
2. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
3. Click **Devices** > **Device List**.
4. Double-click the appropriate device.
5. Select the **Storage** tab.
6. You are presented with a list of storage volumes that this particular host has access to, all grouped by storage type (block, file, other). Select the respective **Action** menu next to the volume that you want to revoke access from, and click **Revoke Access**.
7. Confirm if you want to revoke the access for a volume because the action can't be undone. Click **Yes** to revoke volume access, or **No** to cancel the action.

If you want to disconnect multiple volumes from a specific host, you need to repeat the Revoke Access action for each volume.
{: tip}

### Revoking access from the Storage View in the console
{: #revokeauthStorageUI}
{: help}
{: support}
{: ui}

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}.
2. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
3. Click **Storage** > **{{site.data.keyword.filestorage_short}}**, and select the **Volume** from which you want to revoke access.
4. Click **Authorized Hosts** to display the Compute instances that have access to your File share.
5. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the host whose access is to be revoked, and select **Revoke Access**.
6. Confirm if you want to revoke the access for a volume because the action cannot be undone. Click **Yes** to revoke volume access, or **No** to cancel the action.

If you want to disconnect multiple hosts from a specific volume, you need to repeat the Revoke Access action for each host.
{: tip}

## Revoking a host's access to {{site.data.keyword.filestorage_short}} from the CLI
{: #revokeauthcli}
{: help}
{: support}
{: cli}

### Revoking host authorization from the IBMCLOUD CLI
{: #revokeauthiccli}

You can use the `ibmcloud sl file access-revoke` command to remove authorization from a host. The following command disallows the virtual server instance `87654321` to mount the file share `12345678`.

```sh
ibmcloud sl file access-revoke 12345678 --virtual-id 87654321
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file access-revoke](/docs/cli?topic=cli-sl-file-storage-service#sl_file_access_revoke){: external}.

### Revoking host authorization from the SLCLI
{: #revokeauthslcli}

You can use the following command in SLCLI.
```sh
$ slcli file access-revoke --help
Usage: slcli file access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of one virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of one IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  -s, --subnet-id TEXT      An ID of one subnet to revoke authorization.
  --help                    Show this message and exit.
```
{: screen}

If you want to disconnect multiple hosts from a specific volume, you need to repeat the Revoke Access action for each host.
{: tip}

## Deleting a storage volume in the console
{: #cancelvolUI}
{: help}
{: support}
{: ui}

If you no longer need a specific volume, you can delete that file share. To cancel a storage volume, you need to revoke access from any hosts first.

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") for the volume to be canceled, and select **Delete {{site.data.keyword.filestorage_short}}**.
4. Confirm if you want to delete the volume immediately or on the anniversary date of when the volume was provisioned.

   If you select the option to cancel the volume on its anniversary date, you can void the cancellation request before its anniversary date.
   {: tip}

5. Click **Continue** or **Close**.
6. Click the acknowledgment checkbox, and click **Confirm**.

When the volume is canceled, the request is followed by a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs).
{: note}

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to delete the original volume.

## Deleting a storage volume from the CLI
{: #cancelvolCLI}
{: help}
{: support}
{: cli}

When the volume is canceled, the request is followed by a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs).

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to delete the original volume.

### Deleting a storage volume from the IBMCLOUD CLI
{: #cancelvolICCLI}

You can use the `ibmcloud sl file volume-cancel` command to cancel the file share. The following example cancels the file share `12345678` with immediate effect.

```sh
ibmcloud sl file volume-cancel 12345678 --immediate -f
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file volume-cancel](/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_cancel){: external}.

### Deleting a storage volume from the SLCLI
{: #cancelvolSLCLI}

To delete a storage volume, you can use the following command.
```sh
$ slcli file volume-cancel --help
Usage: slcli file volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation.
  --immediate    Cancels the File Storage volume immediately instead of on the
                 billing anniversary.
  -h, --help     Show this message and exit.
```
{: screen}

## Deleting a storage volume with Terraform
{: #cancelvolTerraform}
{: help}
{: support}
{: terraform}

Use the `terraform destroy` command to conveniently remove a remote object such as a single file share. The following example cancels the file share with the ID `ibm_file_share.example.id`.

```terraform
terraform destroy --target ibm_file_share.example.id
```
{: pre}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

When the volume is canceled, the request is followed by a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQs](/docs/FileStorage?topic=FileStorage-file-storage-faqs).
{: note}

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to delete the original volume.
