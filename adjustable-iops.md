---

copyright:
  years: 2014, 2023
lastupdated: "2023-10-05"

keywords: File Storage, adjusting IOPS, increase IOPS, decrease IOPS, modify IOPS

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Adjusting IOPS
{: #adjustingIOPS}

With this feature, you can adjust the IOPS of your existing {{site.data.keyword.filestorage_short}} immediately. You don't need to create a duplicate or manually copy data to new storage. The adjustment does not cause any kind of outage or lack of access.
{: shortdesc}

Billing for the storage is updated to add the prorated difference of the new price to the current billing cycle. The full new amount is billed in the next billing cycle.

## Advantages of adjustable IOPS
{: #advantagesadjustableIOPS}

- Cost management – Some of our clients might need high IOPS just during peak usage times. For example, a large retail store has peak usage during the holidays and might need higher IOPS on the storage then than in the middle of the summer. With this feature, you can manage your costs and pay for higher IOPS only when you need it.

## Limitations
{: #limitsofadjustIOPS}

You can’t switch between Endurance and Performance when you adjust your IOPS. You can specify a new IOPS value for your storage based on the following criteria:

| Volume size (GB) | IOPS range |
|-------------|-----------------|
| 10 - 39     | 100 - 1,000 |
| 40 - 79     | 100 - 2,000 |
| 80 - 99     | 100 - 4,000 |
| 100 - 499   | 100 - 6,000 |
| 500 - 999   | 100 - 10,000|
| 1,000 - 1,999 | 100 - 20,000|
| 2,000 - 2,999 | 200 - 40,000|
| 3,000 - 3,999 | 200 - 48,000|
| 4,000 - 7,999 | 300 - 48,000|
| 8,000 - 9,999 | 500 - 48,000 |
| 10,000 - 12,000 | 1,000 - 48,000 |
{: caption="Table 1. Available IOPS based on volume size." caption-side="bottom"}

## Effect of IOPS adjustment on replication
{: #IOPSchangereplica}

If the volume has replication in place, the replica is automatically updated to match the IOPS selection of the primary.

## Adjusting the IOPS on your Storage in the UI
{: #adjustingstepsUI}
{: ui}

1. Go to your list of {{site.data.keyword.filestorage_short}}. From the {{site.data.keyword.cloud}} console, click **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") > **Storage** > **{{site.data.keyword.filestorage_short}}**.
2. Select the volume from the list and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Modify File Share**.
3. In the **Adjust Storage IOPS** section, make a new selection:
    - For Endurance (Tiered IOPS), select a different IOPS tier for your storage. You can increase the IOPS tier at any time. However, decreasing is available only once a month.
    - For Performance (Allocated IOPS), specify a new IOPS option for your storage by entering a value in the range 100 - 48,000 IOPS.
4. Review your selection and the new pricing. Click **Modify**.
5. Your new storage allocation is going to be available in a few minutes.

## Adjusting the IOPS on your Storage from the CLI
{: #adjustingstepsCLI}
{: cli}

You can update your IOPS from the CLI by using the following command.

```python
# slcli file volume-modify --help
Usage: slcli file volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of file volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
  -h, --help                    Show this message and exit.
```

## Adjusting the IOPS on your Storage with the API
{: #adjustingstepsAPI}
{: api}

You can adjust the IOPS by using an API call to the SOAP web service. The following sample API calls can be called from the scripting language of your choice.

For more information about the SLAPI, see the [SLDN](http://sldn.softlayer.com/reference/softlayerapi){: external}.
{: tip}

* Adjust IOPS on Performance storage volume.

   ```python
   <?xml version="1.0" encoding="UTF-8"?>
   <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://api.service.softlayer.com/soap/v3.1/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
   <SOAP-ENV:Header>
     <ns1:authenticate>
     </ns1:authenticate>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
     <ns1:placeOrder>
       <orderData xsi:type="ns1:SoftLayer_Container_Product_Order_Network_Storage_AsAService_Upgrade">
         <volume xsi:type="ns1:SoftLayer_Network_Storage">
             <id xsi:type="xsd:int">XXXXXXXXX</id><!-- where XXXXXXXXX is the Volume Id -->
         </volume>
         <iops xsi:type="xsd:int">2007</iops> <!-- This is the upgraded amount -->
         <packageId xsi:type="xsd:int">759</packageId>
         <prices SOAP-ENC:arrayType="ns1:SoftLayer_Product_Item_Price[3]" xsi:type="SOAP-ENC:Array">
             <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                 <id xsi:type="xsd:int">189433</id> <!-- Top level price -->
             </item>
             <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                 <id xsi:type="xsd:int">190233</id> <!-- 2000 - 2999 GBs storage price-->
             </item>
             <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                 <id xsi:type="xsd:int">190293</id> <!-- 200 - 40000 IOPS price-->
             </item>
         </prices>
       </orderData>
     </ns1:placeOrder>
   </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```
   {: codeblock}

* Adjust IOPS on Endurance storage volume.

   ```python
   <?xml version="1.0" encoding="UTF-8"?>
   <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://api.service.softlayer.com/soap/v3.1/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
     <SOAP-ENV:Header>
      <ns1:authenticate>
      </ns1:authenticate>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
      <ns1:placeOrder>
        <orderData xsi:type="ns1:SoftLayer_Container_Product_Order_Network_Storage_AsAService_Upgrade">
          <volume xsi:type="ns1:SoftLayer_Network_Storage">
              <id xsi:type="xsd:int">XXXXXXXX</id> <!--Where Volume XXXXXXXX is the VolumeID -->
          </volume>
          <packageId xsi:type="xsd:int">759</packageId>
          <volumeSize xsi:type="xsd:int">24</volumeSize>
          <prices SOAP-ENC:arrayType="ns1:SoftLayer_Product_Item_Price[3]" xsi:type="SOAP-ENC:Array">
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">189433</id> <!-- Top level price -->
              </item>
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">193373</id> <!-- New Performance tier price -->
              </item>
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">193433</id> <!-- Storage space price for the new tier -->
              </item>
          </prices>
        </orderData>
      </ns1:placeOrder>
    </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```
   {: codeblock}

## Adjusting the IOPS on your Storage with Terraform
{: #adjustingstepsTerraform}
{: terraform}

You can adjust the IOPS by using the `ibm_storage_file` resource, and specifying a different number in the `iops` argument. The following example increases the performance tier of an Endurance share to the 4 IOPS/GB tier.

```terraform
resource "ibm_storage_file" "fs_endurance" {
  type       = "Endurance"
  datacenter = "dal09"
  capacity   = 20
  iops       = 4
}
```
{: codeblock}

The following example changes the performance level of a Performance share to 150 IOPS.

```terraform
resource "ibm_storage_file" "fs_performance" {
  type       = "Performance"
  datacenter = "dal09"
  capacity   = 20
  iops       = 150
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_storage_file](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_file){: external}.

