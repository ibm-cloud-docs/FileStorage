---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, SLCLI, provisioning, API

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Pedindo o {{site.data.keyword.filestorage_short}} por meio do SLCLI
{: #orderingSLCLI}

É possível usar o SLCLI para fazer pedidos de produtos que normalmente são pedidos por meio do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}. Na API do SL, um pedido pode consistir em múltiplos contêineres de pedido. A CLI de pedido funciona com apenas um contêiner de pedido.

Para obter mais informações sobre como instalar e usar o SLCLI, consulte [Cliente da API de Python ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-python.readthedocs.io/en/latest/cli/){:new_window}.
{:tip}

## Procurando ofertas disponíveis do {{site.data.keyword.filestorage_short}}

O primeiro componente a ser procurado quando você faz um pedido é um pacote. Os pacotes são divididos entre os diferentes produtos de nível superior que estão disponíveis para pedido no {{site.data.keyword.BluSoftlayer_full}}. Alguns pacotes de exemplo são CLOUD_SERVER para VSIs, BARE_METAL_SERVER para servidores bare metal e STORAGE_AS_A_SERVICE_STAAS para o {{site.data.keyword.filestorage_short}} e o {{site.data.keyword.blockstorageshort}}.

Dentro de um pacote, alguns itens são subdivididos em categorias. Alguns pacotes têm pré-configurações para a sua conveniência e outros requerem que os itens sejam especificados individualmente. Se a categoria de um pacote for necessária, um item dessa categoria deverá ser escolhido para pedir o pacote. Dependendo da categoria, alguns itens dentro dela podem ser mutuamente exclusivos.

Cada pedido deve ter uma localização associada (data center). Ao pedir o {{site.data.keyword.filestorage_short}}, certifique-se de que ele seja fornecido na mesma localização que as suas instâncias de cálculo.
{:important}

É possível usar o comando `slcli order package-list` para localizar o pacote que você deseja pedir. Uma opção `-keyword` é fornecida para executar procura e filtragem simples. Essa opção facilita localizar o pacote necessário. Procure `Storage-as-a-Service Package 759`.

```
$ slcli order package-list --help
Uso: slcli order package-list [OPTIONS]

  Liste os pacotes que podem ser pedidos por meio da API placeOrder.

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

Também é possível usar o comando `slcli file volume-order`.

```
# slcli file volume-order --help
Usage: slcli file volume-order [OPTIONS]

  Pedir um volume de armazenamento de arquivo.

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

Para obter mais informações sobre como pedir o {{site.data.keyword.filestorage_short}} por meio da API, consulte [order_file_volume ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}.
Para poder acessar todos os novos recursos, peça o `Storage-as-a-Service Package 759`.
{:tip}


## Fazendo o pedido

O exemplo a seguir mostra como pedir um volume {{site.data.keyword.filestorage_short}} de 10 GB com 100 IOPS por GB.

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

Por padrão, é possível provisionar um total combinado de 250 volumes
{{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Para obter mais informações sobre o aumento dos limites, consulte [Gerenciando os limites de armazenamento](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).
{:important}

## Autorizando o acesso dos hosts ao novo armazenamento

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
  --help                    Show this message and exit.
```

Para obter mais informações sobre como autorizar hosts para acessar o {{site.data.keyword.filestorage_short}} por meio da API, consulte [authorize_host_to_volume![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

Para obter mais informações sobre o limite de autorizações simultâneas, consulte as [Perguntas mais frequentes](/docs/infrastructure/FileStorage?topic=FileStorage-faqs).
{:important}

## Conectando seu novo armazenamento
{: #mountingvolumesCLI}

Dependendo do sistema operacional de seu host, siga o link apropriado.
- [Montando o {{site.data.keyword.filestorage_short}} no Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montando o {{site.data.keyword.filestorage_short}} no CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montando o {{site.data.keyword.filestorage_short}} no Container Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com o cPanel](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com o Plesk](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [Montando o volume do {{site.data.keyword.filestorage_short}} em hosts ESXi](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)
