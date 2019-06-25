---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: File Storage, file storage, NFS, provisioning, setup, configuration, mounting storage

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# Tutoriel d'initiation
{: #getting-started}

{{site.data.keyword.filestorage_full}} est un système de stockage de fichiers {{site.data.keyword.filestorage_short}} NAS basé sur NFS, permanent, rapide et flexible. Cet environnement NAS vous permet d'avoir un contrôle total des fonctions et des performances de vos partages de fichiers. Les partages {{site.data.keyword.filestorage_short}} peuvent être connectés à un maximum de 64 unités autorisées via des connexions TCP/IP routées pour la résilience.
{:shortdesc}

## Avant de commencer
{: #prereqs}

Les volumes {{site.data.keyword.filestorage_short}} peuvent être mis à disposition à partir de 20 Go et jusqu'à 12 To avec deux options : <br/>
- Effectuez la mise à disposition avec des niveaux **Endurance** offrant des niveaux de performance prédéfinis et d'autres fonctionnalités telles que les instantanés et la réplication.
- Créez un environnement de **Performance** haute puissance avec des opérations d'entrée-sortie par seconde (IOPS) allouées.

Pour plus d'information sur l'offre {{site.data.keyword.filestorage_short}}, voir [En savoir plus sur {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-about)

## Remarques sur la mise à disposition

### Taille de bloc

La valeur IOPS pour les niveaux Endurance et Performance se fonde sur une taille de bloc de 16 Ko avec une charge de travail aléatoire/séquentielle de 50/50 et de lecture/écriture de 50/50. Un bloc de 16 Ko équivaut à une écriture sur le volume.
{:important}

La taille de bloc utilisée par votre application a une incidence directe sur les performances de stockage. Si la taille de bloc employée par votre application est inférieure à 16 Ko, la limite des opérations d'entrée-sortie par seconde est atteinte avant la limite de débit. A l'inverse, si la taille de bloc qui est utilisée par votre application est supérieure à 16 Ko, la limite de débit est atteinte avant la limite des opérations d'entrée-sortie par seconde.

| Taille de bloc (ko) | IOPS | Débit (Mo/s) |
|-----|-----|-----|
| 4 | 1 000 | 16 |
| 8 | 1 000 | 16 |
| 16 | 1 000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Le tableau 1 présente des exemples de l'impact de la taille de bloc et des opérations d'entrée-sortie par seconde sur le débit.<br/Taille E-S moyennes x IOPS = Débit en Mo/s." caption-side="top"}

### Hôtes autorisés

Un autre facteur à prendre en compte est le nombre d'hôtes qui utilisent votre volume. Si un seul hôte accède au volume, il peut s'avérer difficile de réaliser le nombre maximal d'IOPS disponible, surtout avec des nombres d'IOPS extrêmes (10 000). Si votre charge de travail requiert un débit élevé, il est préférable de configurer au moins deux serveurs pour accéder à votre volume afin d'éviter un goulot d'étranglement dû à un seul serveur.

### Connexion réseau

La vitesse de votre connexion Ethernet doit être supérieure au débit maximal attendu de votre volume. En règle générale, vous ne devriez pas saturer votre connexion Ethernet au-delà de 70 % de la bande passante disponible. Par exemple, si vous disposez de 6 000 IOPS et que vous utilisez une taille de bloc de 16 Ko, le volume peut traiter un débit d'environ 94 Mbit/s. Si vous disposez d'une connexion Ethernet de 1 Gbit/s
vers votre numéro d'unité logique, vous rencontrez un goulot d'étranglement lorsque vos serveurs tentent d'utiliser le débit maximal disponible. Cela est dû au fait que 70 % de la limite théorique d'une connexion Ethernet de 1 Gbit/s (125 Mo par seconde) n'autorisent que 88 Mo par seconde.

Pour atteindre le nombre maximal d'IOPS, vous devez mettre en place les ressources réseau adéquates. Vous devez également tenir compte de l'utilisation du réseau privé en dehors du stockage, ainsi que des réglages côté hôte et spécifiques aux applications (pile IP ou [nombre de lignes de file d'attente](/docs/infrastructure/FileStorage?topic=FileStorage-hostqueuesettings), etc.).

Le trafic de stockage doit être isolé des autres types de trafic et il ne doit pas être dirigé via des pare-feu et des routeurs. La conservation du trafic de stockage sur un réseau local virtuel (VLAN) dédié permet également d'éviter une non concordance MTU lorsque des trames jumbo sont activées. Pour plus d'informations, voir [Jumbo Frames in IBM Cloud ](/docs/FileStorage?topic=FileStorage-jumboframes).

Le trafic de stockage est inclus dans l'utilisation réseau totale des serveurs virtuels publics. Pour plus d'informations sur les limites que peut imposer le service, voir la [documentation sur les serveurs virtuels](/docs/vsi?topic=virtual-servers-about-public-virtual-servers).

### Version NFS

NFS version 3 et NFS version 4.1 sont pris en charge dans l'environnement {{site.data.keyword.cloud}}. Toutefois, NFS version 3 est recommandé car NFS v4.1 est un protocole avec état (et non sans état comme NFS version 3) et des anomalies peuvent survenir lors des événements de réseau. NFS v4.1 doit mettre au repos toutes les opérations, puis effectuer la réclamation de verrou. Sur un serveur de fichiers relativement occupé, l'augmentation du temps d'attente peut entraîner une interruption. L'absence de fonctions de multi-accès et d'établissement de liaison dans NFS version 4.1 peut également allonger la reprise des opérations NFS.

## Soumission de votre commande

Lorsque vous êtes prêt à soumettre votre commande, vous pouvez la placer via la [Console](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole), l'interface [SLCLI](/docs/infrastructure/FileStorage?topic=FileStorage-orderingSLCLI) ou l'interface CLI IBMCLOUD. Pour plus d'informations sur la mise à disposition de File Storage pour des déploiements VMware, voir le [guide d'architecture](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide).

## Connexion de votre nouveau stockage
{: #mountingstorage}

Lorsque votre demande de mise à disposition est terminée, autorisez vos hôtes à accéder au nouveau stockage et configurez votre connexion. Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Accès à {{site.data.keyword.filestorage_short}} sur Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montage de {{site.data.keyword.filestorage_short}} dans CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montage de {{site.data.keyword.filestorage_short}} sur CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec cPanel](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec Plesk](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [Montage de volume {{site.data.keyword.filestorage_short}} sur des hôtes ESXi](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Gestion de votre nouveau stockage

Via le portail ou l'interface SLCLI, vous pouvez gérer différents aspects de {{site.data.keyword.filestorage_short}}, tels que les autorisations et annulations d'hôtes. Pour plus d'informations, voir [Gestion de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-managingstorage).
