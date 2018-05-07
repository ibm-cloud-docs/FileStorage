---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migration de {{site.data.keyword.filestorage_short}} vers {{site.data.keyword.filestorage_short}} chiffré

{{site.data.keyword.filestorage_full}} chiffré pour un stockage Endurance ou Performance a été lancé dans des centres de données sélectionnés. Vous trouverez ci-dessous des informations sur la façon de migrer votre {{site.data.keyword.filestorage_short}} non chiffré en chiffré. Pour plus d'informations sur le stockage chiffré géré par le fournisseur, reportez-vous à l'article [Chiffrement au repos de {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html). Pour afficher la liste des centres de données mis à niveau et des fonctionnalités disponibles, cliquez [ici](new-ibm-block-and-file-storage-location-and-features).

Le chemin de migration préféré consiste à se connecter simultanément aux deux volumes et à transférer les données directement d'un volume de fichier à un autre. Les spécificités dépendent de votre système d'exploitation et de la modification attendue ou non des données lors de la copie.

Les scénarios les plus courants sont indiqués ici pour vous aider. Nous supposons que votre volume de fichier non chiffré est déjà connecté à l'hôte. Si ce n'est pas le cas, suivez les instructions ci-dessous qui correspondent le mieux au système d'exploitation que vous exécutez pour effectuer cette tâche. 

**REMARQUE :** Tous les volumes {{site.data.keyword.filestorage_short}} chiffrés ont un point de montage différent de celui des volumes non chiffrés.  Pour vérifier que vous utilisez le bon point de montage pour les volumes {{site.data.keyword.filestorage_short}} chiffrés et non chiffrés, vous pouvez afficher les informations sur le point de montage sur la page **Détails du volume** de l'interface utilisateur ou accéder au point de montage correct via un appel d'API : SoftLayer_Network_Storage::getNetworkMountAddress().

[Accès à {{site.data.keyword.filestorage_short}} sur Linux](accessing-file-storage-linux.html)

## Création d'un volume de fichier chiffré

Les étapes suivantes permettent de créer un volume chiffré de taille identique ou supérieure pour faciliter le processus de migration.

### Commande d'un volume de stockage Endurance chiffré

1. Cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** sur la page d'accueil du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OU cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}** dans le catalogue {{site.data.keyword.BluSoftlayer_full}}.

2. Cliquez sur le lien **Commander {{site.data.keyword.filestorage_short}}** sur la page {{site.data.keyword.filestorage_short}}.

3. Sélectionnez **Endurance**.

4. Sélectionnez le centre de données dans lequel se trouve le volume d'origine. Notez que le chiffrement est disponible uniquement dans les centres de données marqués d'un astérisque.

5. Entrez le niveau d'**IOPS** souhaité.

6. Sélectionnez la quantité souhaitée d'espace de stockage en Go. Pour les téraoctets, 1 To est égal à 1 000 Go et 12 To sont égaux à 12 000 Go.

7. Entrez la quantité souhaitée d'espace de stockage en Go pour les instantanés.

8. Sélectionnez le système d'exploitation **VMware** dans la liste déroulante.

9. Soumettez la commande.
 
### Commande d'un volume de stockage Performance chiffré

1. Cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** sur la page d'accueil du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OU cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}** dans le catalogue {{site.data.keyword.BluSoftlayer_full}}.

2. Cliquez sur le lien **Commander {{site.data.keyword.filestorage_short}}**.

3. Sélectionnez **Performance**.

4. Sélectionnez le centre de données dans lequel se trouve le volume d'origine. Notez que le chiffrement est disponible uniquement dans les centres de données marqués d'un astérisque (`*`).

5. Sélectionnez la quantité souhaitée d'espace de stockage en Go (taille identique ou supérieure à celle du volume d'origine).

6. Entrez le nombre souhaité d'IOPS par intervalles de 100.

7. Sélectionnez le système d'exploitation VMware dans la liste déroulante.

8. Soumettez la commande.

Le stockage est mis à disposition en moins d'une minute et est visible sur la page {{site.data.keyword.filestorage_short}} du portail client.

 
## Connexion d'un nouveau volume à l'hôte

Les hôtes autorisés sont des hôtes auxquels des droits d'accès à un volume ont été accordés. Sans autorisation d'hôte, vous ne pouvez pas accéder au stockage, ni l'utiliser depuis votre système.

1. Cliquez sur le **Nom du volume** chiffré.

2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés** de la page.

3. Cliquez sur le lien **Hôte autorisé** sur la droite de la page. Sélectionnez les hôtes qui peuvent accéder à ce volume.

Une fois l'autorisation accordée, connectez le volume à l'hôte.

 
## Instantanés et réplication

Des instantanés et la réplication ont-ils été établis pour le volume d'origine ? Si oui, vous devez configurer la réplication ainsi que l'espace d'image instantanée et créer des plannings d'instantané pour le nouveau volume chiffré avec les mêmes paramètres que ceux du volume d'origine. 

Notez que si le centre de données cible n'a pas été mis à niveau pour le chiffrement, vous ne pouvez pas établir de réplication pour le nouveau volume tant que la mise à niveau n'a pas eu lieu.

 
## Migration des données

Votre hôte devrait maintenant être connecté aux volumes {{site.data.keyword.filestorage_short}} d'origine et chiffré. Si tel n'est pas le cas :

• Vérifiez que vous avez correctement suivi les étapes ci-dessus et les documents référencés.

• Ouvrez un ticket de demande de service pour obtenir de l'aide lors de la connexion des deux volumes à l'hôte.

### Remarques sur les données

A ce stade, identifiez le type de données figurant sur votre volume {{site.data.keyword.filestorage_short}} d'origine et pensez à la meilleure façon de les copier sur le volume chiffré. Si vous disposez de sauvegardes, de contenu statique ou d'autres contenus qui ne sont pas susceptibles d'être modifiés au cours de la copie, le processus s'avère assez simple.

Si vous exécutez une base de données ou une machine virtuelle sur votre volume {{site.data.keyword.filestorage_short}}, veillez à ce que les données du volume d'origine ne soient pas modifiées au cours de la copie afin d'éviter toute altération. Si vous rencontrez des problèmes liés à la bande passante, il est conseillé de procéder à la migration pendant les périodes creuses. Si vous avez besoin d'aide quant à ces différents points, n'hésitez pas à ouvrir un ticket de demande de service.

### Microsoft Windows

Pour copier des données depuis votre volume {{site.data.keyword.filestorage_short}} d'origine vers le volume chiffré, formattez le nouveau stockage et copiez les fichiers à l'aide de l'Explorateur Windows.

### Linux

Vous pouvez envisager d'utiliser rsync pour copier les données. Voici un exemple de commande :

`[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage` 

Il est recommandé d'utiliser la commande ci-dessus une fois avec l'indicateur `--dry-run` pour faire en sorte que les chemins s'alignent correctement. Si ce processus est interrompu, vous pouvez supprimer le fichier cible le plus récemment copié et veiller à ce qu'il soit bien copié depuis le début vers le nouvel emplacement.

Une fois cette commande terminée sans l'indicateur `--dry-run`, vos données devraient être copiées sur le volume {{site.data.keyword.filestorage_short}} chiffré. Faites défiler l'écran vers le haut et exécutez la commande une nouvelle fois pour être sûr qu'il ne manque aucun élément. Vous pouvez également vérifier manuellement les deux emplacements et rechercher d'éventuels éléments manquants.

Une fois la migration terminée, vous pouvez déplacer la production vers le volume chiffré, puis déconnecter et supprimer le volume d'origine de votre configuration. Notez que la suppression entraîne également la suppression de tous les instantanés ou répliques du site cible qui était associé au volume d'origine.
