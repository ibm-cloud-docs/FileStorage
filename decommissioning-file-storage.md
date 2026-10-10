---

copyright:
  years: 2025, 2026
lastupdated: "2026-10-10"

keywords: File Storage for Classic, NFS, delete volume, cancel volume, decommission, pre-deletion checklist, accidental deletion

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}


# Decommissioning and deleting {{site.data.keyword.filestorage_short}} volumes
{: #decommissioning-file-storage}

Before you cancel a {{site.data.keyword.filestorage_short}} volume, complete the pre-deletion checklist to confirm the volume is unused and avoid accidental data loss.
{: shortdesc}

Cancellation is permanent. After the 24-hour reclaim period expires, the data is destroyed and cannot be recovered. Take each step in the checklist before you proceed to the deletion procedures.
{: important}

## Pre-deletion checklist
{: #file-storage-deletion-checklist}

Work through each step in order before you cancel the volume.

### Step 1: Confirm ownership and purpose
{: #checklist-confirm-ownership}

Check the **Notes** field on the volume details page to identify the original purpose or owner. Contact the team that ordered the volume to confirm it is no longer needed.

- For more information about how to check notes in the console, see [Updating the notes of a volume in the console](/docs/FileStorage?topic=FileStorage-managingstorage#update-volume-notes-UI).{: ui}
- For more information about how to check notes from the CLI, see [Updating the notes of a volume from the CLI](/docs/FileStorage?topic=FileStorage-managingstorage#update-volume-notes-CLI).{: cli}
- For more information about how to check notes with the API, see [Updating the notes of a volume with the API](/docs/FileStorage?topic=FileStorage-managingstorage#update-volume-notes-API).{: api}

### Step 2: Understand what "authorized hosts" means
{: #checklist-authorized-hosts-meaning}

An authorized host has permission to mount the volume. An empty authorized-host list does not mean that the volume is unused or safe to delete. A volume can be temporarily unmounted for maintenance. The list can be filtered by the user's Classic infrastructure device permissions. Hosts that the user does not have permission to view are hidden from the results.

Do not treat an empty authorized-host list as confirmation that a volume is safe to delete.
{: important}

### Step 3: Verify the authorized-host list under the correct user
{: #checklist-verify-access-list}

The `ibmcloud sl file access-list` command and the SoftLayer API (`SoftLayer_Network_Storage::getObject`) return only hosts that the calling user has Classic infrastructure device permission to see. The relevant object masks are `allowedVirtualGuests`, `allowedHardware`, `allowedSubnets`, and `allowedIpAddresses`. A user without device permissions for a server receives HTTP 200 with an empty list rather than an error or warning. The volume might still be authorized to that server and in active use.

Always run the authorized-host check as the account owner, or as a user who has Classic infrastructure device permissions for all devices in the account. For information about assigning those permissions, see [Managing classic infrastructure access](/docs/iam?topic=iam-mngclassicinfra).

- In the console, see [Viewing the list of hosts that are authorized to access a volume in the console](/docs/FileStorage?topic=FileStorage-managingstorage#viewhostUI).{: ui}
- In the CLI, see [Viewing the list of authorized hosts from the CLI](/docs/FileStorage?topic=FileStorage-managingstorage#viewhostCLI).{: cli}

### Step 4: Check actual data usage on the host
{: #checklist-check-data-usage}

Unlike Block Storage, {{site.data.keyword.filestorage_short}} volumes do report the bytes used. Check the `bytes_used` column in the `ibmcloud sl file volume-list` output to see whether the volume stores data. You can also query usage directly with the API:

```sh
curl -g -u $SL_USER:$SL_APIKEY -X GET \
'https://api.softlayer.com/rest/v3.1/SoftLayer_Network_Storage/{volume_ID}/collectBytesUsed'
```
{: pre}

From any NFS client that mounted the volume, verify usage with:

```sh
df -h /mount/point
```
{: pre}

Confirm with the application team that no workload depends on the volume before you proceed.

### Step 5: Unmount the NFS share from every host
{: #checklist-unmount}

Unmount the volume from every host before you revoke access. Revoking access while the share is mounted can cause stale NFS handles and data corruption on the host.

- On Red Hat Linux, see [unmounting the file system](/docs/FileStorage?topic=FileStorage-mountingLinux#umountRHEL).
- On Ubuntu, see [unmounting the file system](/docs/FileStorage?topic=FileStorage-mountingUbuntu#umountUbuntu).

### Step 6: Revoke all host authorizations
{: #checklist-revoke-access}

After unmounting, revoke access for every authorized host and subnet.

- In the console, see [Revoking a host's access in the console](/docs/FileStorage?topic=FileStorage-managingstorage#revokehostaccessinUI).{: ui}
- In the CLI, see [Revoking a host's access from the CLI](/docs/FileStorage?topic=FileStorage-managingstorage#revokeauthcli).{: cli}

### Step 7: Cancel active replication and remove dependent duplicates
{: #checklist-cancel-replication}

Active replicas and dependent duplicate volumes block reclamation of the parent volume. Cancel them before you attempt to delete the parent.

- Cancel replication: see [Canceling replication](/docs/FileStorage?topic=FileStorage-replication#cancelreplicaUI).
- Cancel dependent duplicates: see [Canceling a storage volume with a dependent duplicate](/docs/FileStorage?topic=FileStorage-duplicatevolume#cancelvolwithdependent).

### Step 8: Understand the reclaim timeline
{: #checklist-reclaim-timeline}

When you cancel a volume, a 24-hour reclaim wait period begins. During this period, the volume remains visible in the console but is inactive. The billing stops immediately. After the reclaim period expires, the data is permanently destroyed and the volume is removed from the console.

To stop a pending cancellation before the reclaim period expires, open a [Support case](/unifiedsupport/cases/add){: external}.

## Deleting a storage volume in the console
{: #cancelvolUI}
{: help}
{: support}
{: ui}

If you no longer need a specific volume, you can delete that file share.

1. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.filestorage_short}}**.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") for the volume to be canceled, and select **Delete {{site.data.keyword.filestorage_short}}**.
4. Confirm whether you want to delete the volume immediately or on the anniversary date of when the volume was provisioned.

   If you select the option to cancel the volume on its anniversary date, you can void the cancellation request before its anniversary date.
   {: tip}

5. Click **Continue** or **close**.
6. Click the acknowledgment checkbox, and click **Confirm**.

## Deleting a storage volume from the CLI
{: #cancelvolCLI}
{: help}
{: support}
{: cli}

### Deleting a storage volume from the IBM Cloud CLI
{: #cancelvolICCLI}

You can use the `ibmcloud sl file volume-cancel` command to cancel the file share. The following example cancels the file share `12345678` with immediate effect.

```sh
ibmcloud sl file volume-cancel 12345678 --immediate -f
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl file volume-cancel](/docs/cli?topic=cli-sl-file-storage-service#sl_file_volume_cancel){: external}.

### Deleting a storage volume from the SLCLI
{: #cancelvolSLCLI}

Use the following command to delete a storage volume.
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

## Deleting a storage volume with the API
{: #cancelvolAPI}
{: help}
{: support}
{: api}

Use the [`cancel_volume` method](https://softlayer-python.readthedocs.io/en/latest/api/managers/SoftLayer.managers.FileStorageManager/#SoftLayer.managers.FileStorageManager.cancel_volume){: external} in the API Python Client. Specify the `volume_id` and whether you want to cancel the volume immediately or on the anniversary date (`immediate=False` or `immediate=True`). Optionally, you can also provide a reason for the cancellation (`reason='No longer needed'`).

## Deleting a storage volume with Terraform
{: #cancelvolTerraform}
{: help}
{: support}
{: terraform}

The preferred way to delete a file share that is managed by Terraform is to remove its `ibm_storage_file` resource block from your configuration and run `terraform apply`. Terraform detects the missing resource and destroys it, keeping your configuration and state in sync.

1. Open your Terraform configuration file and delete the `ibm_storage_file` resource block for the volume.
2. Run `terraform apply` to apply the change.

   ```sh
   terraform apply
   ```
   {: pre}

   Terraform displays a plan that shows the resource is marked for destruction. Confirm the plan to proceed.

If you want to destroy a specific file share without editing your configuration, you can use `terraform destroy --target` instead. The following example targets a single resource by its Terraform address.

```sh
terraform destroy --target ibm_storage_file.example
```
{: pre}

Using this method preserves the resource block in your configuration, which creates a drift between your config and your infrastructure state. Remove the resource block from your configuration after you confirm that the volume is deleted.

For more information, see [terraform apply](https://developer.hashicorp.com/terraform/cli/commands/apply){: external} and [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.
