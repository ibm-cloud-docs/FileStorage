---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-07"

---
{:new_window: target="_blank"}

# Commande de {{site.data.keyword.filestorage_short}}

Vous pouvez mettre à disposition {{site.data.keyword.filestorage_short}} et l'ajuster en fonction de vos besoins en termes de capacité et d'IOPS. Profitez pleinement de votre stockage grâce à deux options vous permettant de spécifier les performances.

- Vous pouvez effectuer une sélection parmi les niveaux d'IOPS Endurance qui proposent des niveaux de performances prédéfinis afin de prendre en charge les charges de travail pour lesquelles il n'existe aucune exigence bien définie en matière de performances.
- Vous pouvez ajuster votre stockage en fonction d'exigences de performances très spécifiques en spécifiant le nombre total d'IOPS avec Performance.

## Commande de {{site.data.keyword.filestorage_short}} avec des niveaux d'IOPS prédéfinis (Endurance)

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander {{site.data.keyword.filestorage_short}}**.
3. Sélectionnez l'**emplacement** de votre déploiement (centre de données).
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes de calcul dont vous disposez.
4. Facturation. Si vous avez sélectionné un centre de données avec des fonctionnalités améliorées (indiqué par un astérisque), vous avez le choix entre une facturation mensuelle ou horaire.
     1. Avec la facturation **horaire**, le nombre d'heures d'existence du volume de fichier sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html).
     2. Avec la facturation **mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).
        >**REMARQUE** : la facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont **pas** été mis à jour avec les fonctionnalités améliorées.
5. Entrez votre taille de stockage dans la zone **Nouvelle taille de stockage**.
6. Sélectionnez **Endurance (IOPS hiérarchisées)** dans la section **Options d'IOPS de stockage**.
7. Sélectionnez le niveau d'IOPS requis par votre application.
    - L'option **0,25 IOPS par Go** est adaptée aux charges de travail peu exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.
    - L'option **2 IOPS par Go** est adaptée à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données qui sauvegardent des applications Web ou des images de disques de machine virtuelle pour un hyperviseur.
    - L'option **4 IOPS par Go** est adaptée aux charges de travail plus exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
    - L'option **10 IOPS par Go** est adaptée aux charges de travail les plus intensives, telles que celles créées par les bases de données NoSQL et le traitement de données pour Analytics. Ce niveau est disponible dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html) pour un stockage qui est mis à disposition à hauteur de 4 To.
8. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](ordering-snapshots.html).
9. Cochez les cases **Dispositions**, puis cliquez sur **Passer une commande**.
10. Votre nouvelle allocation de stockage est disponible en quelques minutes.

>**Remarque** : par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](managing-storage-limits.html).<br/><br/>Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](faqs.html).

## Commande de {{site.data.keyword.filestorage_short}} avec un nombre d'IOPS personnalisé (Performance)

1. Sur le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** >** Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander {{site.data.keyword.filestorage_short}}**.
3. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes de calcul dont vous disposez.
4. Facturation. Si vous avez sélectionné un centre de données avec des fonctionnalités améliorées (indiqué par un astérisque), vous avez le choix entre une facturation mensuelle ou horaire.
     1. Avec la facturation **horaire**, le nombre d'heures d'existence du volume de fichier sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html).
     2. Avec la facturation **mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).
        >**REMARQUE** : la facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont **pas** été mis à jour avec les fonctionnalités améliorées.
5. Entrez votre taille de stockage dans la zone **Nouvelle taille de stockage**.
6. Sélectionnez **Performance (IOPS allouées)** dans la section **Options d'IOPS de stockage**.
7. Entrez le nombre d'IOPS dans la zone **IOPS allouées**.
8. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](ordering-snapshots.html).
9. Cochez les cases **Dispositions**, puis cliquez sur **Passer une commande**.
10. Votre nouvelle allocation de stockage est disponible en quelques minutes.

>**Remarque** : par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](managing-storage-limits.html).<br/><br/>Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](faqs.html).


## Connexion de votre nouveau stockage

Lorsque votre demande de mise à disposition est terminée, autorisez vos hôtes à accéder au nouveau stockage et configurez votre connexion. Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Accès à {{site.data.keyword.filestorage_short}} sur Linux](accessing-file-storage-linux.html)
- [Montage de NFS/{{site.data.keyword.filestorage_short}} dans CentOS](mounting-nsf-file-storage.html)
- [Montage de {{site.data.keyword.filestorage_short}} sur CoreOS](mounting-storage-coreos.html)
- [Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec cPanel](configure-backup-cpanel.html)
- [Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec Plesk](configure-backup-plesk.html)
- [Montage de volume {{site.data.keyword.filestorage_short}} sur des hôtes ESXi](architecture-guide-file-storage-vmware.html)


## Identification des volumes {{site.data.keyword.filestorage_short}} sur la facture

Tous les partages de fichiers apparaissent sur votre facture sous forme de lignes d'article. "Endurance Storage Service" s'affiche pour les volumes de type Endurance et "Performance Storage Service" pour les volumes de type Performance. Le taux varie en fonction de votre niveau de stockage. Vous pouvez développer Endurance ou Performance pour voir qu'il s'agit de {{site.data.keyword.filestorage_short}}.
