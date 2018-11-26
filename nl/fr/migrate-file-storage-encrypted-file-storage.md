---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migration de {{site.data.keyword.filestorage_short}} vers {{site.data.keyword.filestorage_short}} amélioré

La fonctionnalité {{site.data.keyword.filestorage_full}} amélioré est désormais disponible dans des centres de données sélectionnés. Pour afficher la liste des centres de données mis à niveau et des fonctionnalités disponibles, telles que les taux IPOS ajustables et les volumes pouvant être développés, cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html). Pour plus d'informations sur le stockage chiffré géré par un fournisseur, voir [Chiffrement au repos pour {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html).

Le chemin de migration préféré consiste à se connecter simultanément aux deux volumes et à transférer les données directement d'un numéro d'unité logique à un autre. Les spécificités dépendent de votre système d'exploitation et de la modification attendue ou non des données lors de la copie.

Nous supposons que votre numéro d'unité logique non chiffré est déjà connecté à l'hôte. Si tel n'est pas le cas, suivez les instructions ci-dessous qui correspondent le mieux à votre système d'exploitation pour effectuer cette tâche.

- [Montage de {{site.data.keyword.filestorage_short}} sur Linux](accessing-file-storage-linux.html)
- [Montage de NFS/{{site.data.keyword.filestorage_short}} dans CentOS](mounting-nsf-file-storage.html)
- [Montage de {{site.data.keyword.filestorage_short}} sur CoreOS](mounting-storage-coreos.html)

Tous les volumes {{site.data.keyword.filestorage_short}} améliorés ont un point de montage différent de celui des volumes non chiffrés. Pour vérifier que vous utilisez le bon point de montage pour les volumes {{site.data.keyword.filestorage_short}} chiffrés et non chiffrés, vous pouvez afficher les informations relatives au point de montage sur la page **Détails du volume** du portail {{site.data.keyword.slportal}}. Vous pouvez également accéder au point de montage correct via un appel d'API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Création d'un nouveau {{site.data.keyword.filestorage_short}}

Lorsque vous passez une commande via l'API, spécifiez le package "Storage as a Service" pour être certain d'obtenir les fonctionnalités mises à jour avec votre nouveau stockage.
{:important}

Les instructions ci-après s'appliquent lors de la commande d'un volume/partage de fichiers amélioré via le catalogue {{site.data.keyword.BluSoftlayer_full}} du portail {{site.data.keyword.slportal}}. Votre nouveau volume doit avoir une taille identique ou supérieure à celle du volume d'origine afin de faciliter la migration.

### Commande d'un nouveau volume de stockage Endurance

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}**.
3. Sélectionnez **Endurance** dans la liste **Sélectionner le type de stockage**.
4. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du stockage d'origine.
5. Sélectionnez votre option de facturation. Vous avez le choix entre une facturation au mois et une facturation à l'heure.
6. Cliquez sur **Endurance** et sélectionnez le niveau IOPS.
6. Sélectionnez la **Taille de stockage utilisable** dans la liste. Votre nouveau volume doit avoir une taille identique ou supérieure à celle du volume d'origine.
7. Choisissez la **taille de l'espace d'image instantanée** (en plus de votre espace utilisable) dans la liste.
8. Cliquez sur **Continuer**. Les prix mensuels et calculés au prorata s'affichent pour vous permettre de vérifier une dernière fois les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier la commande.
9. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.

### Commande d'un volume de stockage Performance chiffré

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** >** Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}**.
3. Sélectionnez **Performance** dans la liste **Sélectionner le type de stockage**.
4. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
    -  Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du stockage d'origine.
5. Sélectionnez vos options de facturation. Vous avez le choix entre une facturation à l'heure et une facturation au mois.
6. Sélectionnez le bouton radio en regard de la **Taille de stockage** appropriée.
6. Entrez le nombre d'IOPS dans la zone **Spécifier les IOPS**.
7. Cliquez sur **Continuer**. Les prix mensuels et calculés au prorata s'affichent pour vous permettre de vérifier une dernière fois les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier la commande.
8. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.

Le stockage est mis à disposition en moins d'une minute et est visible sur la page {{site.data.keyword.filestorage_short}} du portail {{site.data.keyword.slportal}}.


## Autorisation des hôtes pour l'accès au nouveau {{site.data.keyword.filestorage_short}}

Les hôtes "autorisés" sont des hôtes auxquels des droits d'accès à un volume ont été accordés. Sans autorisation d'hôte, vous ne pouvez pas accéder au stockage ni l'utiliser depuis votre système.

1. Cliquez sur le nom de votre nouveau volume.
2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés**.
3. Cliquez sur le lien **Hôte autorisé** sur le côté droit. Sélectionnez les hôtes qui peuvent accéder à ce volume.

Une fois l'autorisation accordée à l'hôte, connectez le volume à votre hôte.


## Configuration des instantanés et de la réplication

Si des instantanés et la réplication ont été établis pour votre volume d'origine, vous devez les configurer pour le nouveau volume. Configurez la réplication, ainsi que l'espace d'image instantanée et créez des plannings d'instantané avec les mêmes paramètres que ceux du volume d'origine.

Si le chiffrement n'est pas activé pour le centre de données cible, vous ne pouvez pas établir de réplication pour le nouveau volume tant que ce centre de données n'a pas été mis à niveau.{:important}


## Migration de vos données

1. Connectez-vous à votre volume d'origine et à votre nouveau volume {{site.data.keyword.filestorage_short}}.
  - Si vous avez besoin d'aide pour savoir comment connecter les deux partages de fichiers à votre hôte, ouvrez un ticket de demande de service.

2. Identifiez le type de données figurant sur votre volume {{site.data.keyword.filestorage_short}} d'origine et pensez à la meilleure façon de les copier sur votre nouveau partage de fichiers.
  - Si vous disposez de sauvegardes, d'un contenu statique ou d'autres contenus qui ne sont pas susceptibles d'être modifiés au cours de la copie, le processus s'avère assez simple.
  - Si vous exécutez une base de données ou une machine virtuelle sur votre service {{site.data.keyword.filestorage_short}}, vérifiez que les données ne sont pas modifiées lors de la copie pour éviter leur altération. Si vous rencontrez des problèmes liés à la bande passante, procédez à la migration pendant les périodes creuses. Si vous avez besoin d'aide quant à ces différents points, ouvrez un ticket de demande de service.

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
