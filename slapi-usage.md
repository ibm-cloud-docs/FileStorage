---

copyright:
  years: 2026
lastupdated: "2026-01-28"

keywords: File Storage for Classic, NSF, SLAPI, Python

subcollection: FileStorage

---
{{site.data.keyword.attribute-definition-list}}

# Setting up your API environment
{: #setupAPI}

You can use the SLAPI to accomplish tasks that are normally handled through the [{{site.data.keyword.cloud}} console](/cloud-storage/file){: external}. For example, you can place orders for new volumes, snapshot space, and replication, update authorizations, cancel volumes, query capacity usage.
{: shortdesc}

Before you begin, review the [Introduction to the SL API](https://sldn.softlayer.com/article/getting-started/){: external} in the [API Reference](https://sldn.softlayer.com/reference/softlayerapi/){: external}. For more information about the {{site.data.keyword.filestorage_short}} service and available data types, see the [`SoftLayer_Network_Storage`](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Storage/){: external} topic.

## Setting up your environment with the SLAPI for Python client
{: #setup-api-environment-import-manager}
{: api}

You can pass your username and SL API Key every time you make an API request. You can also set your credentials as the environmental variables `SL_USERNAME` and `SL_API_KEY`, and create an environment that is only accessible from the {{site.data.keyword.cloud}} private network, and a timeout of 4 minutes. See the following example.

```sh
$ export SL_USERNAME=YOUR_USERNAME
$ export SL_API_KEY=YOUR_API_KEY
$ python
>>> import SoftLayer
>>> client = SoftLayer.create_client_from_env(username='YOUR_USERNAME',api_key='YOUR_API_KEY', endpoint_url=SoftLayer.API_PRIVATE_ENDPOINT, timeout=240)
```
{: codeblock}

For day-to-day operation, you might find the managers in the API Python client to be the most convenient means for interacting with the API. Managers abstract many of the complexities of using the API into classes that provide a simpler interface to various services. To interact with {{site.data.keyword.filestorage_short}} services, import the `FileStorageManager`.

```python
from SoftLayer import BlockStorageManager, FileStorageManager, Client
```
{: pre}

For more information, see [SL API Client for Python](https://softlayer-python.readthedocs.io/en/latest/api/client/){: external} and [FileStorageManager](https://softlayer-python.readthedocs.io/en/latest/api/managers/SoftLayer.managers.FileStorageManager/#SoftLayer.managers.FileStorageManager.list_file_volumes){: external} topics.
