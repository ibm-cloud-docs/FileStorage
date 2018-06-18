---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Commande de {{site.data.keyword.filestorage_short}} 

Vous pouvez mettre à disposition {{site.data.keyword.filestorage_full}} de deux manières différentes en fonction de vos besoins et de vos préférences. Les deux options possibles sont les suivantes :

- **Endurance** : met à disposition des niveaux Endurance offrant des niveaux de performance prédéfinis et des fonctionnalités telles que les instantanés et la réplication.
- **Performance** : créez un environnement Performance haute puissance dans lequel vous pouvez allouer les opérations d'entrée-sortie par seconde (IOPS) souhaitées.

## Commande de l'option Endurance pour {{site.data.keyword.filestorage_short}}

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}** dans l'angle supérieur droit.  
3. Sélectionnez **Endurance** dans la liste **Sélectionner le type de stockage**.
4. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage sera ajouté au même emplacement que celui des hôtes précédemment commandés.
   - Si vous avez sélectionné un centre de données avec des fonctionnalités améliorées (indiqué par un astérisque), vous avez le choix entre une facturation mensuelle ou horaire. 1. Avec la facturation **horaire**, le calcul du nombre d'heures d'existence du volume de fichier sur le compte s'effectue lors de la suppression du volume ou à la fin du cycle de facturation, à la première occurrence de l'un des deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage mis à disposition dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Avec la facturation **mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail utilisant des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).
     **REMARQUE** : la facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont pas été mis à jour avec les fonctionnalités améliorées.
5. Sélectionnez le niveau de stockage pour vos applications :
    - L'option **0,25 IOPS par Go** est adaptée aux charges de travail peu exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.
    - L'option **2 IOPS par Go** est adaptée à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données utilisées par des applications Web ou d'images de disques de machine virtuelle pour un hyperviseur.
    - L'option **4 IOPS par Go** est adaptée aux charges de travail plus exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
    - L'option **10 IOPS par Go** est adaptée aux charges de travail les plus intensives, telles que celles créées par les bases de données NoSQL et le traitement de données pour Analytics. Ce niveau est disponible dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html) pour un stockage mis à disposition à hauteur de 4 To.
6. Sélectionnez la **Taille de stockage utilisable** dans la liste. 
7. Choisissez la **taille de l'espace d'image instantanée** (en plus de votre espace utilisable) dans la liste. 
8. Cliquez sur **Continuer**. Les prix mensuels et calculés au prorata s'affichent pour vous permettre de vérifier une dernière fois les détails de la commande.
9. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.
10. Votre nouvelle allocation de stockage devait être disponible en quelques minutes.

**Remarque** : Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.filestorage_short}}. Contactez votre ingénieur commercial si vous souhaitez augmenter le nombre de vos volumes. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](managing-storage-limits.html).

Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](File-Storage-FAQ.html).

## Commande de l'option Performance pour {{site.data.keyword.filestorage_short}}

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** >** Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Commander {{site.data.keyword.filestorage_short}}** dans l'angle supérieur droit.  
3. Sélectionnez **Performance** dans la liste Sélectionner le type de stockage.
4. Cliquez sur la liste **Emplacement** et sélectionnez votre centre de données.
    - Vérifiez que le nouveau stockage sera ajouté au même emplacement que celui des hôtes précédemment commandés.
    - Si vous avez sélectionné un centre de données avec des fonctionnalités améliorées (indiqué par un astérisque), vous avez le choix entre une facturation mensuelle ou horaire.  
       1. Avec la facturation **horaire**, le nombre d'heures d'existence du volume de fichier sur le compte est calculé lors de la suppression du volume ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage mis à disposition dans des centres de données sélectionnés. 2. Avec la facturation **mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail utilisant des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).
       **REMARQUE** : la facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont pas été mis à jour avec les fonctionnalités améliorées.  
5. Sélectionnez la **taille de stockage** appropriée.
6. Entrez le nombre d'IOPS dans la zone **Spécifier les IOPS**.
7. Cliquez sur **Continuer**. Les prix mensuels et calculés au prorata s'affichent pour vous permettre de vérifier une dernière fois les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier la commande.
8. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.
9. Votre nouvelle allocation de stockage devait être disponible en quelques minutes.

**Remarque** : Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.filestorage_short}}. Contactez votre ingénieur commercial si vous souhaitez augmenter le nombre de vos volumes. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](managing-storage-limits.html).

Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](File-Storage-FAQ.html).

## Comment identifier mes volumes {{site.data.keyword.filestorage_short}} sur ma facture ?

Tous les volumes apparaissent sur votre facture sous forme de lignes d'article. Les volumes de type Endurance apparaissent en tant que service Stockage Endurance, et les volumes de type Performance en tant que service Stockage Performance. Le tarif varie en fonction du niveau de stockage. Vous pouvez développer Endurance ou Performance pour voir qu'il s'agit d'un volume {{site.data.keyword.filestorage_short}}.

