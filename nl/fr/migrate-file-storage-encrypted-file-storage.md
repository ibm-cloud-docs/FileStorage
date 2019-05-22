---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, upgrade, migrate to new

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migration de {{site.data.keyword.filestorage_short}} vers {{site.data.keyword.filestorage_short}} amélioré
{: #migratestorage}

La fonctionnalité {{site.data.keyword.filestorage_full}} amélioré est désormais disponible dans des centres de données sélectionnés. Pour afficher la liste des centres de données mis à niveau et des fonctions disponibles telles que les débits d'IOPS ajustables et les volumes extensibles, cliquez [ici](/docs/infrastructure/FileStorage?topic=FileStorage-news). Pour en savoir plus sur le chiffrement géré par les fournisseurs, voir [Chiffrement au repos dans {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-encryption).

Le chemin de migration recommandé consiste à se connecter simultanément aux deux volumes et à transférer les données via NFS directement d'un numéro d'unité logique à un autre. Les spécificités dépendent de votre système d'exploitation et de la modification attendue ou non des données lors de la copie.

Nous supposons que votre numéro d'unité logique non chiffré est déjà connecté à l'hôte. Si tel n'est pas le cas, suivez les instructions ci-dessous qui correspondent le mieux à votre système d'exploitation pour effectuer cette tâche.

- [Montage de {{site.data.keyword.filestorage_short}} sur Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montage de {{site.data.keyword.filestorage_short}} dans CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montage de {{site.data.keyword.filestorage_short}} sur CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

Tous les volumes {{site.data.keyword.filestorage_short}} améliorés mis à disposition dans ces centres de données ont un point de montage différent de celui des volumes non chiffrés. Pour vérifier que vous utilisez le bon point de montage pour les deux types de volume de stockage, vous pouvez afficher les informations sur le point de montage sur la page **Détails du volume** de la console. Vous pouvez également accéder au point de montage correct via un appel d'API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Création d'un {{site.data.keyword.filestorage_short}}

Lorsque vous passez une commande via l'API, spécifiez le package "Storage as a Service" pour être certain d'obtenir les fonctionnalités mises à jour avec votre nouveau stockage.
{:important}

Vous pouvez commander un numéro d'unité logique étendu via le catalogue {{site.data.keyword.BluSoftlayer_full}} et le portail {{site.data.keyword.slportal}}. Votre nouveau volume doit avoir une taille identique ou supérieure à celle du partage de fichiers d'origine afin de faciliter la migration.

- [Commande de {{site.data.keyword.filestorage_short}} avec des niveaux d'IOPS prédéfinis (Endurance)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [Commande de {{site.data.keyword.filestorage_short}} avec un nombre d'IOPS personnalisé (Performance)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

Votre nouveau stockage est disponible pour montage en quelques minutes. Il figure dans la Liste de ressources et dans la liste {{site.data.keyword.blockstorageshort}}.


## Autorisation des hôtes pour l'accès au nouveau {{site.data.keyword.filestorage_short}}

Les hôtes "autorisés" sont des hôtes auxquels des droits d'accès à un volume ont été accordés. Sans autorisation d'hôte, vous ne pouvez pas accéder au stockage ni l'utiliser depuis votre système.

1. Cliquez sur le nom de votre nouveau volume.
2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés**.
3. Cliquez sur le lien **Hôte autorisé** sur le côté droit. Sélectionnez les hôtes qui peuvent accéder à ce volume.

Une fois l'autorisation accordée à l'hôte, connectez le volume à votre hôte.


## Configuration des instantanés et de la réplication

Si des instantanés et la réplication ont été établis pour votre volume d'origine, vous devez les configurer pour le nouveau volume. Configurez la réplication, ainsi que l'espace d'instantané et créez des plannings d'instantané avec les mêmes paramètres que ceux du volume d'origine.

Si le chiffrement n'est pas activé pour le centre de données cible, vous ne pouvez pas établir de réplication pour le nouveau volume tant que ce centre de données n'a pas été mis à niveau.
{:important}


## Migration de vos données

1. Connectez-vous à votre volume d'origine et à votre nouveau volume {{site.data.keyword.filestorage_short}}.
  - Si vous avez besoin d'aide pour savoir comment connecter les deux partages de fichiers à votre hôte, ouvrez un ticket de demande de service.

2. Identifiez le type de données figurant sur votre volume {{site.data.keyword.filestorage_short}} d'origine et pensez à la meilleure façon de les copier sur votre nouveau partage de fichiers.
  - Si vous disposez de sauvegardes, d'un contenu statique ou d'autres contenus qui ne sont pas susceptibles d'être modifiés au cours de la copie, vous n'avez pas à vous inquiéter.
  - Si vous exécutez une base de données ou une machine virtuelle sur votre service {{site.data.keyword.filestorage_short}}, vérifiez que les données ne sont pas modifiées lors de la copie pour éviter leur altération.
  - Si vous rencontrez des problèmes liés à la bande passante, procédez à la migration pendant les périodes creuses.
  - Si vous avez besoin d'aide quant à ces différents points, ouvrez un ticket de demande de service.

3. Copiez vos données.
   - **Microsoft Windows**
     - Pour copier des données depuis votre numéro d'unité logique {{site.data.keyword.filestorage_short}} d'origine vers le nouveau numéro d'unité logique chiffré, formatez le nouveau stockage et copiez les fichiers à l'aide de l'Explorateur Windows.
   - **Linux**
     - Vous pouvez utiliser `rsync` pour copier les données.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   Il est recommandé d'utiliser une fois la commande précédente avec l'indicateur `--dry-run` pour faire en sorte que les chemins s'alignent correctement. Si ce processus est interrompu, vous pouvez supprimer le fichier cible le plus récemment copié et veiller à ce qu'il soit bien copié depuis le début vers le nouvel emplacement.

   Une fois cette commande terminée sans l'indicateur `--dry-run`, vos données sont copiées sur le nouveau volume {{site.data.keyword.filestorage_short}}. Exécutez la commande une nouvelle fois pour être sûr qu'il ne manque aucun élément. Vous pouvez également vérifier manuellement les deux emplacements et rechercher d'éventuels éléments manquants.

   Une fois la migration terminée, vous pouvez déplacer la production vers le nouveau numéro d'unité logique. Ensuite, vous pouvez détacher et supprimer votre volume d'origine de votre configuration. Cette suppression entraîne également la suppression de tous les instantanés ou répliques du site cible qui étaient associés au volume d'origine.
