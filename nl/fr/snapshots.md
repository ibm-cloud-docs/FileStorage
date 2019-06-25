---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, snapshots

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Instantanés
{: #snapshots}

Les instantanés sont une fonction d'{{site.data.keyword.filestorage_full}}. Un instantané représente le contenu d'un volume à un point précis dans le temps. Les instantanés vous permettent de protéger vos données sans impact sur les performances et avec une consommation minimale de l'espace. Ils sont considérés comme votre première ligne de défense dans le cadre de la protection des données. Si un utilisateur modifie ou supprime par erreur des données critiques d'un volume, les données peuvent être facilement et rapidement restaurées à partir d'une copie d'instantané.

{{site.data.keyword.filestorage_short}} vous propose deux façons de prendre des instantanés.

* La première via un planning d'instantané configurable qui crée et supprime automatiquement des copies d'instantané pour chaque volume de stockage. Vous pouvez également créer des plannings d'instantané supplémentaires, supprimer des copies manuellement et gérer les plannings en fonction de vos besoins.
* La seconde méthode consiste à prendre un instantané manuel.

Une copie d'instantané est une image en lecture seule d'un volume {{site.data.keyword.filestorage_short}} qui capture l'état du volume à un point dans le temps. Les copies d'instantané sont efficaces en termes de durée de création et d'espace de stockage. La création d'une copie d'instantané {{site.data.keyword.filestorage_short}} ne prend que quelques secondes, en général moins d'une seconde, quelle que soit la taille du volume ou le niveau d'activité sur le stockage. Une fois une copie d'instantané créée, les modifications apportées aux objets de données sont reflétées dans les mises à jour de la version en cours des objets, comme s'il n'existait pas de copies d'instantané. Pendant ce temps, la copie des données reste stable.

Une copie d'instantané n'entraîne aucune baisse de performances. Les utilisateurs peuvent facilement stocker jusqu'à 50 instantanés planifiés et 50 instantanés manuels par volume {{site.data.keyword.filestorage_short}}, qui sont tous accessibles en tant que versions en ligne et en lecture seule des données.

Les instantanés vous permettent :

- de créer de manière transparente des points de récupération à un point de cohérence,
- de restaurer des volumes ponctuels antérieurs.

Vous devez d'abord acheter une certaine quantité d'espace d'instantané pour votre volume avant de pouvoir prendre des instantanés de celui-ci. Il est possible d'ajouter de l'espace d'instantané lors de la commande initiale ou après via la page des **détails sur le volume**. Les instantanés planifiés et manuels partagent l'espace d'instantané ; veillez à commander l'espace en conséquence. Consultez l'article [Commande d'instantanés](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots) pour plus de détails et conseils.

## Meilleures pratiques concernant les instantanés

La conception des instantanés dépend de l'environnement du client. Prenez en compte les remarques de conception suivantes pour planifier et implémenter les copies d'instantané :
- Vous pouvez créer jusqu'à 50 instantanés grâce à la planification et jusqu'à 50 instantanés manuellement sur chaque volume ou numéro d'unité logique.
- Ne prenez pas trop d'instantanés. Veillez à ce que la fréquence des instantanés planifiés corresponde à vos besoins en termes d'objectif de temps de reprise et d'objectif de point de reprise, ainsi qu'à vos exigences professionnelles liées aux applications et planifiez des instantanés à un rythme horaire, quotidien ou hebdomadaire.
- La fonction de suppression automatique des instantanés permet de contrôler la croissance de la consommation d'espace de stockage.

  Le seuil de suppression automatique est fixé à 95 %.
  {:note}

Les instantanés ne se substituent pas à la réplication de reprise après incident hors site ou à la sauvegarde à long terme.
{:important}

## Sécurité

Tous les instantanés et répliques de données chiffrées {{site.data.keyword.filestorage_short}} sont également chiffrés par défaut. Cette fonction ne peut pas être désactivée par volume. Pour plus d'informations sur le chiffrement au repos géré par le fournisseur, voir [Sécurisation de vos données](/docs/infrastructure/FileStorage?topic=FileStorage-encryption).

## Répercussions des instantanés sur l'espace disque

Les copies d'instantané minimisent la consommation du disque en conservant des blocs individuels plutôt que des fichiers entiers. Les copies d'instantané n'utilisent de l'espace supplémentaire qu'en cas de modification ou de suppression des fichiers situés dans le système de fichiers actif.

Dans le système de fichiers actif, les blocs modifiés sont réécrits à des emplacements différents sur le disque ou retirés sous la forme de blocs de fichier complets. Lorsque des fichiers sont modifiés ou supprimés, les blocs de fichier d'origine sont conservés dans une ou plusieurs copies d'instantané. Ainsi, l'espace disque qui est utilisé par les blocs d'origine est conservé pour refléter le statut du système de fichiers actif avant la modification. La réservation de cet espace s'ajoute à l'espace disque employé par les blocs dans le système de fichiers actif modifié.

| Utilisation d'espace disque |   |
|-----|-----|
| ![Espace qui est utilisé avant la prise d'une copie d'instantané](/images/bfcircle1.png "Avant une copie d'instantané") | Avant la création d'une copie d'instantané, l'espace disque est utilisé uniquement par le système de fichiers actif. |
| ![Espace qui est utilisé lors de la prise d'une copie d'instantané](/images/bfcircle3.png "Après une copie d'instantané") | Après la création d'une copie d'instantané, le système de fichiers actif et la copie d'instantané pointent vers les mêmes blocs disque. La copie d'instantané n'utilise pas d'espace disque supplémentaire.  |
| ![Espace qui est utilisé lorsqu'une modification a lieu après la prise d'une copie d'instantané](/images/bfcircle2.png "Modifications après une copie d'instantané") | Même après la suppression de `myfile.txt` du système de fichiers actif, la copie d'instantané inclut toujours le fichier et fait référence à ses blocs de disque. C'est la raison pour laquelle la suppression des données du système de fichiers actif ne libère pas toujours de l'espace disque. |
{: caption="Le tableau 1 illustre dans quelle mesure les instantanés affectent l'utilisation de l'espace dans le stockage." caption-side="top"}


Pour plus d'informations sur l'utilisation de l'espace d'instantané, voir [Gestion des instantanés](/docs/infrastructure/FileStorage?topic=FileStorage-managingSnapshots).
