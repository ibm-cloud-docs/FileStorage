---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migration de {{site.data.keyword.filestorage_short}} vers {{site.data.keyword.filestorage_short}} amélioré

La fonctionnalité {{site.data.keyword.filestorage_full}} amélioré est désormais disponible dans des centres de données sélectionnés. Pour afficher la liste des centres de données mis à niveau et des fonctionnalités disponibles, telles que les taux IPOS ajustables et les volumes pouvant être développés, cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html). Pour plus d'informations sur le stockage chiffré géré par le fournisseur, reportez-vous à l'article [Chiffrement au repos de {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html). 

Le chemin de migration préféré consiste à se connecter simultanément aux deux LUN et à transférer les données directement d'un numéro d'unité logique à un autre. Les spécificités dépendent de votre système d'exploitation et de la modification attendue ou non des données lors de la copie. 

Nous supposons que votre numéro d'unité logique non chiffré est déjà connecté à l'hôte. Si ce n'est pas le cas, suivez les instructions ci-dessous qui correspondent le mieux à votre système d'exploitation pour effectuer cette tâche :

- [Montage de {{site.data.keyword.filestorage_short}} sur Linux](accessing-file-storage-linux.html)
- [Mountage NFS/{{site.data.keyword.filestorage_short}} dans CentOS](mounting-nsf-file-storage.html)
- [Mountage de {{site.data.keyword.filestorage_short}} sur CoreOS](mounting-storage-coreos.html)

**REMARQUE :** tous les volumes {{site.data.keyword.filestorage_short}} améliorés ont un point de montage différent de celui des volumes non chiffrés. Pour vérifier que vous utilisez le bon point de montage pour les volumes {{site.data.keyword.filestorage_short}} chiffrés et non chiffrés, vous pouvez afficher les informations relatives au point de montage sur la page **Détails du volume** de l'interface utilisateur. Vous pouvez également accéder au point de montage correct via un appel d'API : `SoftLayer_Network_Storage::getNetworkMountAddress()`. 


## Création d'un nouveau stockage {{site.data.keyword.filestorage_short}}

**IMPORTANT** : lorsque vous passez une commande via l'API, spécifiez le package "Storage as a Service" pour être certain d'obtenir les fonctionnalités mises à jour avec votre nouveau stockage. 

Les instructions ci-après s'appliquent lors de la commande d'un volume/partage de fichiers amélioré via l'interface utilisateur. Votre nouveau volume doit avoir une taille identique ou supérieure à celle du volume d'origine afin de faciliter la migration.

### Commande d'un volume de stockage Endurance

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}** dans l'angle supérieur droit.  
3. Sélectionnez **Endurance** dans la liste **Sélectionner le type de stockage**.
4. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage sera ajouté au même emplacement que celui du stockage d'origine. 
5. Sélectionnez votre option de facturation. Vous avez le choix entre une facturation au mois et une facturation à l'heure. 
6. Cliquez sur **Endurance** et sélectionnez le niveau IOPS. 
6. Sélectionnez la **Taille de stockage utilisable** dans la liste. Votre nouveau volume doit avoir une taille identique ou supérieure à celle du volume d'origine. 
7. Choisissez la **taille de l'espace d'instantané** (en plus de votre espace utilisable) dans la liste déroulante.
8. Cliquez sur **Continuer**. Les prix mensuels et calculés au prorata s'affichent pour vous permettre de vérifier une dernière fois les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier la commande.
9. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.
 
### Commande d'un volume de stockage Performance chiffré

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** >** Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}** dans l'angle supérieur droit.  
3. Sélectionnez **Performance** dans la liste Sélectionner le type de stockage.
4. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
    -  Vérifiez que le nouveau stockage sera ajouté au même emplacement que celui du stockage d'origine. 
5. Sélectionnez vos options de facturation. Vous avez le choix entre une facturation à l'heure et une facturation au mois. 
6. Sélectionnez le bouton radio en regard de la **Taille de stockage** appropriée.
6. Entrez le nombre d'IOPS dans la zone **Spécifier les IOPS**.
7. Cliquez sur **Continuer**. Les prix mensuels et calculés au prorata s'affichent pour vous permettre de vérifier une dernière fois les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier la commande.
8. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.

Le stockage est mis à disposition en moins d'une minute et est visible sur la page {{site.data.keyword.filestorage_short}} du portail {{site.data.keyword.slportal}}.

 
## Connexion d'un nouveau stockage {{site.data.keyword.filestorage_short}} à l'hôte

Les hôtes "autorisés" sont des hôtes auxquels des droits d'accès à un volume ont été accordés. Sans autorisation d'hôte, vous ne pourrez pas accéder au stockage, ni l'utiliser depuis votre système.

1. Cliquez sur le nom de votre nouveau volume.
2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés** de la page.
3. Cliquez sur le lien **Hôte autorisé** sur le côté droit de la page. Sélectionnez les hôtes qui peuvent accéder à ce volume.

Une fois l'autorisation accordée, connectez le volume à votre hôte.

 
## Instantanés et réplication

Des instantanés et la réplication ont-ils été établis pour le volume d'origine ? Si oui, vous devez configurer la réplication ainsi que l'espace d'instantané et créer des plannings d'instantané pour le nouveau volume chiffré avec les mêmes paramètres que ceux du volume d'origine. 

**Remarque** : si le centre de données cible n'a pas été mis à niveau pour le chiffrement, vous ne pourrez pas établir de réplication pour le nouveau volume tant que la mise à niveau n'aura pas eu lieu. 

 
## Migration des données

Votre hôte devrait maintenant être connecté aux volumes {{site.data.keyword.filestorage_short}} d'origine et chiffré. Si tel n'est pas le cas :

- Vérifiez que vous avez correctement suivi les étapes décrites dans ce document et dans les documents référencés.
- Ouvrez un ticket de demande de service pour obtenir de l'aide lors de la connexion des deux volumes à l'hôte.

### Remarques sur les données

A ce stade, identifiez le type de données figurant sur votre volume {{site.data.keyword.filestorage_short}} d'origine et pensez à la meilleure façon de les copier sur le volume chiffré. Si vous disposez de sauvegardes, de contenu statique ou d'autres contenus qui ne sont pas susceptibles d'être modifiés au cours de la copie, le processus s'avère assez simple.

Si vous exécutez une base de données ou une machine virtuelle sur votre volume {{site.data.keyword.filestorage_short}}, assurez-vous que les données du volume d'origine ne soient pas modifiées au cours de la copie afin d'éviter toute altération. Si vous rencontrez des problèmes liés à la bande passante, il est conseillé de procéder à la migration pendant les périodes creuses. Si vous avez besoin d'aide quant à ces différents points, ouvrez un ticket de demande de service.

### Microsoft Windows

Pour copier des données depuis votre volume {{site.data.keyword.filestorage_short}} d'origine vers le volume chiffré, formattez le nouveau stockage et copiez les fichiers à l'aide de l'Explorateur Windows.

### Linux

Vous pouvez envisager d'utiliser `rsync` pour copier les données. Voici un exemple de commande :

```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

Il est recommandé d'utiliser l'exemple de commande une fois avec l'indicateur `--dry-run` pour faire en sorte que les chemins s'alignent correctement. Si ce processus est interrompu, vous pouvez supprimer le fichier cible le plus récemment copié et veiller à ce qu'il soit bien copié depuis le début vers le nouvel emplacement.

Une fois cette commande terminée sans l'indicateur `--dry-run`, vos données devraient être copiées sur le volume {{site.data.keyword.filestorage_short}} chiffré. Faites défiler l'écran vers le haut et exécutez la commande une nouvelle fois pour être sûr qu'il ne manque aucun élément. Vous pouvez également vérifier manuellement les deux emplacements et rechercher d'éventuels éléments manquants.

Une fois la migration terminée, vous pouvez déplacer la production vers le volume chiffré, puis déconnecter et supprimer le volume d'origine de votre configuration.  

**Remarque** : la suppression entraîne également la suppression de tous les instantanés ou répliques du site cible qui était associé au volume d'origine.
