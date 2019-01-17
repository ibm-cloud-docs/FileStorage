---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Commande de {{site.data.keyword.filestorage_short}} via l'interface de ligne de commande SL

Vous pouvez utiliser l'interface de ligne de commande SL pour commander des produits qui se commandent normalement via le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}. Dans l'API SL, une commande peut se composer de plusieurs conteneurs de commandes. L'interface de ligne de commande pour les commandes fonctionne avec un seul conteneur de commandes.

Pour plus d'informations sur la manière d'installer et d'utiliser l'interface de ligne de commande SL, voir [Client API Python](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Recherche des offres {{site.data.keyword.filestorage_short}} disponibles

Lorsque vous passez une commande, vous devez commencer par rechercher est un package. Les packages sont répartis entre les différents produits de niveau supérieur que vous pouvez commander dans {{site.data.keyword.BluSoftlayer_full}}. Entre autres exemples de packages, citons CLOUD_SERVER pour instances de serveur virtuel, BARE_METAL_SERVER pour serveurs bare metal et STORAGE_AS_A_SERVICE_STAAS pour {{site.data.keyword.filestorage_short}} et {{site.data.keyword.blockstorageshort}}.

Dans un package, certains éléments sont subdivisés en catégories. Certains packages disposent de préréglages pour votre commodité et d'autres nécessitent que des éléments soient spécifiés individuellement. Si une catégorie de package est requise, un élément de cette catégorie doit être sélectionné pour commander le package. Selon la catégorie, certains éléments de la catégorie s'excluent mutuellement.

A chaque commande doit être associé un emplacement (centre de données). Lorsque vous commandez {{site.data.keyword.filestorage_short}}, veillez à ce que sa mise disposition s'effectue dans le même emplacement que vos instances de traitement.
{:important}

Vous pouvez utiliser la commande `slcli order package-list` pour rechercher le package que vous voulez commander. Une option `–keyword` est fournie pour effectuer une recherche et un filtrage simples. Cette option facilite la recherche du package dont vous avez besoin. Recherchez `Storage-as-a-Service Package 759`.

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

Vous pouvez également utiliser la commande `slcli file volume-order`.

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

Pour plus d'informations sur les commandes {{site.data.keyword.filestorage_short}} via l'API, voir [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}.
Pour pouvoir accéder à toutes les nouvelles fonctions, commandez `Storage-as-a-Service Package 759`.
{:tip}


## Passation de la commande

L'exemple suivant montre comment commander un volume {{site.data.keyword.filestorage_short}} de 10 Go avec 100 E-S/s par Go.

```
# slcli file volume-order --storage-type performance --size 20 --location dal10 --iops 100
Order #32076317 placed successfully!
> Storage as a Service
> File Storage
> 20 GBs
> 100 IOPS
```

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Pour plus d'informations sur l'augmentation des limites, voir [Gestion des limites de stockage](managing-storage-limits.html).
{:important}

## Autorisation des hôtes pour l'accès au nouveau stockage

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

Pour plus d'informations sur l'autorisation des hôtes à accéder à {{site.data.keyword.filestorage_short}} via l'API, voir [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file.html#SoftLayer.managers.file.FileStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

Pour plus d'informations sur la limite des autorisations simultanées, voir, voir la [Foire aux questions](faqs.html).
{:important}

## Connexion de votre nouveau stockage

Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Montage de {{site.data.keyword.filestorage_short}} sur Linux](accessing-file-storage-linux.html)
- [Montage de {{site.data.keyword.filestorage_short}} dans CentOS](mounting-nsf-file-storage.html)
- [Montage de {{site.data.keyword.filestorage_short}} sur Container Linux](mounting-storage-coreos.html)
- [Configuring {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec cPanel](configure-backup-cpanel.html)
- [Configuring {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec Plesk](configure-backup-plesk.html)
- [Montage de volume {{site.data.keyword.filestorage_short}} sur des hôtes ESXi](architecture-guide-file-storage-vmware.html)
