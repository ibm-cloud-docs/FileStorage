---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Création d'un volume {{site.data.keyword.filestorage_short}} dupliqué

Vous avez la possibilité de créer un doublon d'un volume {{site.data.keyword.filestorage_full}} existant. Le volume dupliqué hérite par défaut des options de capacité et de performance du volume d'origine et possède une copie des données jusqu'au point de cohérence d'un instantané.   

Etant donné que le volume dupliqué est basé sur les données de l'instantané d'un point de cohérence, vous devez disposer d'un espace d'image instantanée sur le volume d'origine avant de créer un doublon.  Pour en savoir plus sur les instantanés et sur la commande d'espace d'image instantanée, reportez-vous à l'article [Instantanés](snapshots.html).  

Vous pouvez créer des doublons à partir de volumes principaux et de volumes de réplique, le nouveau doublon étant créé dans le même centre de données que celui du volume d'origine.  Par exemple, si vous créez un doublon à partir d'un volume de réplique, le nouveau volume est créé dans le même centre de données que le volume de réplique.    

Les volumes dupliqués sont accessibles par un hôte en lecture/écriture dès la mise à disposition du stockage. Les instantanés et la réplication ne sont pas autorisés tant que la copie des données depuis le volume d'origine vers le doublon n'est pas terminée. 

Une fois la copie des données achevée, le doublon peut être géré et utilisé comme un volume entièrement indépendant du volume d'origine. 

Cette fonctionnalité est disponible uniquement pour le stockage mis à disposition avec chiffrement. Pour connaître la liste des centres de données disponibles, cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html). 

Voici quelques exemples d'utilisation courante d'un volume dupliqué :
  - **Test de reprise après incident** : créez un doublon de votre volume de réplique pour vérifier que les données sont intactes et qu'elles peuvent être utilisées dans le cas d'un sinistre sans interruption de la réplication. 
  - **Copie finale** : utilisez un volume de stockage comme copie finale à partir de laquelle vous pouvez créer plusieurs instances en vue d'utilisations différentes. 
  - **Actualisation des données** : créez une copie de vos données de production à monter sur votre environnement de non-production en vue de leur test. 
  - **Restauration à partir d'un instantané** : restaurez des données sur le volume d'origine avec une date ou des fichiers spécifiques d'un instantané sans écraser le volume d'origine entier avec une fonction de restauration d'instantané. 
  - **Développement/Test** : créez jusqu'à 4 doublons simultanés d'un volume en même temps pour créer des volumes comportant des données dupliquées à des fins de développement et de test. 
  - **Redimensionnement de stockage** : créez un nouveau volume avec une nouvelle taille et/ou un nouveau nombre d'IOPS sans avoir à effectuer une migration de vos données basée sur l'hôte.  
	

Il existe plusieurs manières de créer un volume dupliqué via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} : 

## Création d'un doublon à partir d'un volume spécifique dans la liste de stockage

Accédez à votre liste de volumes {{site.data.keyword.filestorage_short}} :

Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.filestorage_short}}** OU, dans le Catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.filestorage_short}}**. 

1.	Sélectionnez un volume dans la liste et cliquez sur **Actions** > **Doublon du numéro d'unité logique/volume**. 
2.	Choisissez une option d'instantané : 
    -	Si vous effectuez votre commande à partir d'un volume qui n'est pas un volume de réplique :
      -	Sélectionnez **Créer à partir d'un nouvel instantané** pour créer un nouvel instantané qui sera utilisé pour le volume dupliqué. Choisissez cette option s'il n'existe aucun instantané pour votre volume ou si vous souhaitez créer un doublon à ce point de cohérence. 
                      OU 

      -	Sélectionnez **Créer à partir du dernier instantané** pour créer un doublon à partir du dernier instantané, quel qu'il soit, existant pour ce volume. 
    -	Si vous effectuez votre commande à partir d'un volume de réplique, l'unique option d'instantané consiste à utiliser le dernier instantané disponible. 
3.	Facturation horaire ou mensuelle : vous pouvez choisir de mettre le nouveau volume dupliqué à disposition avec une facturation horaire ou mensuelle.  Le type de facturation du volume d'origine est automatiquement sélectionné, mais si vous souhaitez choisir un autre type de facturation pour le nouveau stockage dupliqué, vous pouvez indiquer votre choix ici.
4. 	Le Type de stockage (Endurance ou Performance) et l'Emplacement restent identiques à ce qui est indiqué pour le volume d'origine. 
5.	Si vous le souhaitez, vous pouvez indiquer les IOPS ou le niveau d'IOPS du nouveau volume. Les IOPS du volume d'origine sont définies par défaut. 
      -	Si le volume d'origine a un niveau Endurance avec 0,25 IOPS, vous ne pourrez pas effectuer de nouvelle sélection. 
      -	Si le volume d'origine a un niveau Endurance avec 2, 4 ou 10 IOPS, vous pouvez indiquer n'importe lequel de ces niveaux pour le nouveau volume. 
      -	Les combinaisons de performance et de taille disponibles s'affichent. 
6.	Si vous le souhaitez, vous pouvez mettre à jour la taille du nouveau volume de telle sorte qu'il soit plus grand que le volume d'origine.  La taille du volume d'origine est définie par défaut. 
  	-	**Remarque** : Le redimensionnement de {{site.data.keyword.filestorage_short}} est soumis à la limite de 10 fois la taille du volume d'origine. 
7.	Si vous le souhaitez, vous pouvez mettre à jour l'espace d'image instantanée du nouveau volume et ajouter davantage d'espace, le réduire ou n'indiquer aucun espace d'image instantanée. L'espace d'image instantanée du volume d'origine est défini par défaut. 
8.	Cliquez sur **Continuer** pour passer votre commande du doublon. 



## Création d'un doublon à partir d'un instantané spécifique

Accédez à votre liste de volumes {{site.data.keyword.filestorage_short}} :

1.	Cliquez sur un **numéro d'unité logique/volume** dans la liste pour afficher la page des détails. (Il peut s'agir ou non d'un volume de réplique.) 
2.	Faites défiler l'écran et sélectionnez un instantané existant sur la page des détails, puis cliquez sur **Actions > Doublon**.   
3.	Le Type de stockage (Endurance ou Performance) et l'Emplacement restent identiques à ce qui est indiqué pour le volume d'origine. 
4.	Si vous le souhaitez, vous pouvez indiquer les IOPS ou le niveau d'IOPS du nouveau volume. Les IOPS du volume d'origine sont définies par défaut. 
      - Si le volume d'origine a un niveau Endurance avec 0,25 IOPS, vous ne pourrez pas effectuer de nouvelle sélection. 
      - Si le volume d'origine a un niveau Endurance avec 2, 4 ou 10 IOPS, vous pouvez indiquer n'importe lequel de ces niveaux pour le nouveau volume. 
      - Les combinaisons de performance et de taille disponibles s'affichent. 
5.	Si vous le souhaitez, vous pouvez mettre à jour la taille du nouveau volume de telle sorte qu'il soit plus grand que le volume d'origine.  La taille du volume d'origine est définie par défaut. 
      - **Remarque** : Le redimensionnement de {{site.data.keyword.filestorage_short}} est soumis à la limite de 10 fois la taille du volume d'origine. 
6.	Si vous le souhaitez, vous pouvez mettre à jour l'espace d'image instantanée du nouveau volume et ajouter davantage d'espace, le réduire ou n'indiquer aucun espace d'image instantanée. L'espace d'image instantanée du volume d'origine est défini par défaut. 
7.	Cliquez sur **Continuer** pour passer votre commande du doublon. 


## Gestion du volume dupliqué

Pendant que les données sont copiées depuis le volume d'origine vers le doublon, un statut s'affiche sur la page des détails indiquant que la duplication est en cours. Pendant ce temps, vous pouvez établir une connexion vers un hôte et effectuer des opérations de lecture/écriture, mais vous ne pouvez pas créer de planning d'instantané. Une fois le processus de duplication terminé, le nouveau volume est complètement indépendant du volume d'origine ; il peut être géré avec des instantanés et des réplications, etc., comme un volume normal. 
