---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Commande et gestion d'{{site.data.keyword.filestorage_full_notm}}

Vous pouvez mettre à disposition deux types différents de stockage en fonction de vos besoins et de vos préférences. Les deux options de volume {{site.data.keyword.filestorage_short}} sont les suivantes : 

- **Endurance** : met à disposition des niveaux Endurance offrant des niveaux de performance prédéfinis et des fonctionnalités telles que les instantanés et la réplication.
- **Performance** : créez un environnement Performance haute puissance dans lequel vous pouvez allouer les opérations d'entrée-sortie par seconde (IOPS) souhaitées. 

## Mise à disposition de {{site.data.keyword.filestorage_short}}

### Commande de l'option Endurance pour {{site.data.keyword.filestorage_short}}

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** OU, dans le Catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}** dans l'angle supérieur droit de l'écran.  
3. Sélectionnez **Endurance** dans la liste déroulante **Sélectionner le type de stockage**.
4. Cliquez sur la liste déroulante **Emplacement** et sélectionnez votre centre de données. 
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui des hôtes précédemment commandés. 
   - Si vous avez sélectionné un centre de données avec des fonctionnalités améliorées (indiqués par une * dans la liste déroulante), vous avez le choix entre une facturation mensuelle ou horaire.  
     1. Avec la facturation **horaire**, le calcul du nombre d'heures d'existence du volume de fichier sur le compte s'effectue lors de la suppression du volume ou à la fin du cycle de facturation, à la première occurrence de l'un des deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage mis à disposition dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Avec la facturation **mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail utilisant des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).**REMARQUE** : La facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont pas été mis à jour avec les fonctionnalités améliorées. 
5. Sélectionnez le niveau de stockage pour vos applications :
    - L'option **0,25 IOPS par Go** est adaptée aux charges de travail peu exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise. 
    - L'option **2 IOPS par Go** est adaptée à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données utilisées par des applications Web ou d'images de disques de machine virtuelle pour un hyperviseur.
    - L'option **4 IOPS par Go** est adaptée aux charges de travail plus exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
    - L'option **10 IOPS par Go** est adaptée aux charges de travail les plus intensives telles que celle créées par les bases de données NoSQL et le traitement de données pour Analytics. Ce niveau est disponible dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html) pour un stockage mis à disposition à hauteur de 4 To. 
6. Sélectionnez la **Taille de stockage utilisable** dans la liste déroulante. 
7. Choisissez la **taille de l'espace d'instantané** (en plus de votre espace utilisable) dans la liste déroulante. 
8. Cliquez sur **Continuer**. Les prix mensuels et calculés au prorata s'affichent pour vous permettre de vérifier une dernière fois les détails de la commande. 
9. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.
10. Votre nouvelle allocation de stockage devait être disponible en quelques minutes.

**Remarque** : Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Si vous souhaitez augmenter le nombre de volumes, contactez votre ingénieur commercial. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](managing-storage-limits.html).

Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](File-Storage-FAQ.html).

### Commande de l'option Performance pour {{site.data.keyword.filestorage_short}}

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.filestorage_short}}** OU, dans le Catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** >** Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}** dans l'angle supérieur droit de l'écran.  
3. Sélectionnez **Performance** dans la liste déroulante Sélectionner le type de stockage. 
4. Cliquez sur la liste déroulante **Emplacement** et sélectionnez votre centre de données. 
    -  Vérifiez que le nouveau stockage est ajouté au même emplacement que celui des hôtes précédemment commandés. 
    -  Si vous avez sélectionné un centre de données avec des fonctionnalités améliorées (indiqués par une * dans la liste déroulante), vous avez le choix entre une facturation mensuelle ou horaire.  
       1.  Avec la facturation **horaire**, le calcul du nombre d'heures d'existence du volume de fichier sur le compte s'effectue lors de la suppression du volume ou à la fin du cycle de facturation, à la première occurrence de l'un des deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage mis à disposition dans des centres de données sélectionnés.  
       2. Avec la facturation **mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail utilisant des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).
**REMARQUE** : La facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont pas été mis à jour avec les fonctionnalités améliorées.   
5. Sélectionnez le bouton radio en regard de la **Taille de stockage** appropriée.
6. Entrez le nombre d'IOPS dans la zone **Spécifier les IOPS**. 
7. Cliquez sur **Continuer**. Les prix mensuels et calculés au prorata s'affichent pour vous permettre de vérifier une dernière fois les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier la commande. 
8. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.
9. Votre nouvelle allocation de stockage devait être disponible en quelques minutes.

**Remarque** : Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Si vous souhaitez augmenter le nombre de volumes, contactez votre ingénieur commercial. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](managing-storage-limits.html).

Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](File-Storage-FAQ.html).

## Gestion de {{site.data.keyword.filestorage_short}}

### Autorisation des hôtes pour l'accès à {{site.data.keyword.filestorage_short}}

Les hôtes autorisés sont les hôtes qui ont reçu des droits d'accès sur un volume spécifique. Sans autorisation d'hôte, vous ne pouvez pas accéder au stockage, ni l'utiliser depuis votre système. L'autorisation d'un hôte pour l'accès à votre volume génère un nom d'utilisateur et un mot de passe.  

**Remarque** : Vous ne pouvez autoriser et connecter que des hôtes qui résident dans le même centre de données que votre stockage. Si vous disposez de plusieurs comptes, vous ne pouvez pas autoriser un hôte d'un autre compte à accéder à votre stockage sur un autre compte.  

1. Cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}**, puis sur le **Nom du volume**.
2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés** de la page. 
3. Cliquez sur le lien **Hôte autorisé** sur la droite de la page. Sélectionnez les hôtes qui peuvent accéder à ce volume en particulier. 

 

### Affichage de la liste des hôtes autorisés sur un volume {{site.data.keyword.filestorage_short}} 

Pour afficher la liste des hôtes autorisés sur un volume, procédez comme indiqué ci-après.

1. Cliquez sur **Stockage > {{site.data.keyword.filestorage_short}}**, puis sur le **Nom du volume**.
2. Faites défiler la page vers le bas jusqu'à la section **Hôtes autorisés**.

Cette section affiche la liste des hôtes en cours autorisés à accéder au volume.


### Affichage des volumes {{site.data.keyword.filestorage_short}} sur lesquels un hôte est autorisé 

Vous pouvez afficher les volumes auxquels un hôte peut accéder, y compris les informations nécessaires pour l'établissement d'une connexion (Nom du volume, Type de stockage, Adresse cible, Capacité et Emplacement) :

1. Cliquez sur **Unités** > **Liste des unités** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, puis cliquez sur l'unité appropriée.
2. Sélectionnez l'onglet Stockage.

La liste des volumes de stockage auxquels cet hôte particulier a accès s'affiche (les volumes sont regroupés par type de stockage : bloc, fichier, autre). Les menus **Action** respectifs vous permettent d'autoriser un stockage supplémentaire ou de retirer l'accès. 

 

### Montage et démontage de {{site.data.keyword.filestorage_short}}

Vous pouvez vous reporter aux informations relatives aux points de montage fournies dans la vue ** Détails du volume** pour monter {{site.data.keyword.filestorage_short}} à partir d'un hôte.

Reportez-vous à l'article suivant qui comporte des détails sur le montage et le démontage de {{site.data.keyword.filestorage_short}} à partir d'un hôte : [Accès à {{site.data.keyword.filestorage_short}} sur Linux](accessing-file-storage-linux.html).

 

### Révocation de l'accès d'un hôte à {{site.data.keyword.filestorage_short}}

Si vous souhaitez ne plus autoriser un hôte à accéder à un volume de stockage en particulier, vous pouvez révoquer l'accès. Lors de la révocation de l'accès, la connexion hôte est supprimée du volume et ni le système d'exploitation, ni les applications ne peuvent alors communiquer avec le volume. 

**Remarque :** Pour éviter les problèmes côté hôte, démontez le volume de stockage de votre système d'exploitation avant de révoquer l'accès afin de prévenir tout incident lié à des unités manquantes ou à l'altération des données.

Vous pouvez révoquer l'accès à partir de Stockage dans la Liste des unités ou dans les vues Stockage. 

#### Révocation de l'accès à partir de la Liste des unités : 

1. Cliquez sur **Unités** > **Liste des unités** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, puis cliquez deux fois sur l'unité appropriée.
2. Sélectionnez l'onglet **Stockage**.
3. La liste des volumes de stockage auxquels cet hôte particulier a accès s'affiche (les volumes sont regroupés par type de stockage : bloc, fichier, autre). Sélectionnez le menu **Action** adéquat en regard du volume pour lequel vous souhaitez révoquer l'accès et cliquez sur **Révoquer le droit d'accès**.
4. Il vous est demandé si vous souhaitez révoquer l'accès à un volume car cette action ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès au volume ou sur **Non** pour annuler l'action.

**Remarque :** Si vous souhaitez déconnecter plusieurs volumes d'un hôte spécifique, vous devez répéter l'action Révoquer le droit d'accès pour chaque volume.

 

#### Révocation de l'accès à partir de la vue Stockage : 
1. Cliquez sur **Stockage, {{site.data.keyword.filestorage_short}}** et sélectionnez le **Volume** pour lequel vous souhaitez révoquer l'accès. 
2. Faites défiler la page jusqu'à la section **Hôtes autorisés**. 
3. Cliquez sur la flèche de déroulement **Actions** située en regard de l'hôte dont l'accès doit être révoqué et sélectionnez **Révoquer le droit d'accès**.
4. Il vous est demandé si vous souhaitez révoquer l'accès à un volume car cette action ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès au volume ou sur **Non** pour annuler l'action.

**Remarque :** Si vous souhaitez déconnecter plusieurs hôtes d'un volume spécifique, vous devez répéter l'action Révoquer le droit d'accès pour chaque hôte.

 

### Annulation d'un volume de stockage

Si vous n'avez plus besoin d'un volume spécifique, vous pouvez l'annuler. Pour ce faire, il est d'abord nécessaire de révoquer l'accès à partir de tous les hôtes.

1. Cliquez sur **Stockage**>**{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur la flèche de déroulement **Actions** correspondant au volume à annuler et sélectionnez **Annuler {{site.data.keyword.filestorage_short}}**.
3. Vous êtes invité à confirmer l'annulation du volume de manière immédiate ou à la date anniversaire de la mise à disposition du volume. Cliquez sur **Continuer** ou sur **Fermer**.
**Remarque** : Si vous sélectionnez l'option d'annulation du volume à sa date anniversaire, vous pouvez annuler la demande d'annulation avant la date anniversaire. 
4. Cochez la case d'accusé de réception et cliquez sur **Confirmer**.

 

### Affichage des détails d'un volume {{site.data.keyword.filestorage_short}} mis à disposition 

Vous pouvez afficher un récapitulatif des informations clés sur le volume de stockage sélectionné, ainsi que les fonctionnalités supplémentaires d'instantané et de réplication qui ont été ajoutées au stockage. 

1. Cliquez sur **Stockage**>**{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur le **Nom du volume** approprié dans la liste. 

### Identification de mes volumes {{site.data.keyword.filestorage_short}} sur ma facture 

Tous les volumes apparaissent sur votre facture sous forme d'une ligne article. Les volumes de type Endurance apparaissent en tant que service Stockage Endurance, et les volumes de type Performance en tant que service Stockage Performance. Le tarif varie en fonction du niveau de stockage. Vous pouvez développer Endurance ou Performance pour voir s'il s'agit d'un volume {{site.data.keyword.filestorage_short}}.
