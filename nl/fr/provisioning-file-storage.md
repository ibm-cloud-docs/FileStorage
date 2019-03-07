---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Commande de {{site.data.keyword.filestorage_short}} via la console
{: #orderingConsole}

Vous pouvez mettre à disposition {{site.data.keyword.filestorage_short}} et l'ajuster en fonction de vos besoins en termes de capacité et d'IOPS (entrées-sorties par seconde). Profitez pleinement de votre stockage grâce à deux options vous permettant de spécifier les performances.

- Vous pouvez effectuer une sélection parmi les niveaux d'IOPS Endurance qui proposent des niveaux de performances prédéfinis afin de prendre en charge les charges de travail pour lesquelles il n'existe aucune exigence bien définie en matière de performances.
- Vous pouvez ajuster votre stockage en fonction d'exigences de performances spécifiques en spécifiant le nombre total d'IOPS avec Performance.

## Commande de {{site.data.keyword.filestorage_short}} avec des niveaux d'IOPS prédéfinis (Endurance)
{: #endurance}

1. Connectez-vous au [catalogue IBM Cloud](https://{DomainName}/catalog/){:new_window}, puis cliquez sur **Stockage**. Ensuite, sélectionnez {{site.data.keyword.filestorage_short}}. Cliquez sur **Créer**.

   Vous pouvez également vous connecter au portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}, puis cliquer sur **Stockage** > **{{site.data.keyword.filestorage_short}}**. Dans l'angle supérieur droit, cliquez sur **Commander {{site.data.keyword.filestorage_short}}**.
2. Sélectionnez l'**emplacement** de votre déploiement (centre de données).
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes de calcul dont vous disposez.
3. Facturation. Si vous avez sélectionné un centre de données avec des fonctionnalités améliorées (indiqué par un astérisque), vous avez le choix entre une facturation mensuelle ou horaire.
     1. Avec la facturation **horaire**, le nombre d'heures d'existence du volume de fichier sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](/docs/infrastructure/FileStorage?topic=FileStorage-news).
     2. Avec la facturation **mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).

     La facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont **pas** été mis à jour avec les fonctionnalités améliorées.
     {:note}
4. Entrez votre taille de stockage dans la zone **Nouvelle taille de stockage**.
5. Sélectionnez **Endurance (IOPS hiérarchisées)** dans la section **Options d'IOPS de stockage**.
6. Sélectionnez le niveau d'IOPS requis par votre application.
    - L'option **0,25 IOPS par Go** est adaptée aux charges de travail peu exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.
    - L'option **2 IOPS par Go** est adaptée à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données qui sauvegardent des applications Web ou des images de disques de machine virtuelle pour un hyperviseur.
    - L'option **4 IOPS par Go** est adaptée aux charges de travail plus exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
    - L'option **10 IOPS par Go** est adaptée aux charges de travail les plus intensives, telles que celles créées par les bases de données NoSQL et le traitement de données pour Analytics. Ce niveau est disponible dans des [centres de données sélectionnés](/docs/infrastructure/FileStorage?topic=FileStorage-news) pour un stockage qui est mis à disposition à hauteur de 4 To.
7. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots).
8. Sur la droite, passez en revue votre récapitulatif de commande et appliquez votre code promo si vous en avez un.

   Les remises sont appliquées lors du traitement de la commande.
   {:note}
9. Après avoir lu les dispositions, cochez la case **J'ai lu et j'accepte les contrats de service tiers**.
10. Cliquez sur **Créer**. Votre nouvelle allocation de stockage est disponible en quelques minutes.

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).<br/><br/>Pour plus d'informations sur la limite des autorisations simultanées, voir [Foire aux questions](/docs/infrastructure/FileStorage?topic=FileStorage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:tip}

## Commande de {{site.data.keyword.filestorage_short}} avec un nombre d'IOPS personnalisé (Performance)
{: #performance}

1. Connectez-vous au [catalogue IBM Cloud](https://{DomainName}/catalog/){:new_window}, puis cliquez sur **Stockage**. Ensuite, sélectionnez {{site.data.keyword.filestorage_short}}. Cliquez sur **Créer**.

   Vous pouvez également vous connecter au portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}, puis cliquer sur **Stockage** > **{{site.data.keyword.filestorage_short}}**. Dans l'angle supérieur droit, cliquez sur **Commander {{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes de calcul dont vous disposez.
3. Facturation. Si vous avez sélectionné un centre de données avec des fonctionnalités améliorées (indiqué par un astérisque), vous avez le choix entre une facturation mensuelle ou horaire.
     1. Avec la facturation **horaire**, le nombre d'heures d'existence du volume de fichier sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](/docs/infrastructure/FileStorage?topic=FileStorage-news).
     2. Avec la facturation **mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).

     La facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont **pas** été mis à jour avec les fonctionnalités améliorées.
     {:note}
4. Entrez votre taille de stockage dans la zone **Nouvelle taille de stockage**.
5. Sélectionnez **Performance (IOPS allouées)** dans la section **Options d'IOPS de stockage**.
6. Entrez le nombre d'IOPS dans la zone **IOPS allouées**.
7. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots).
8. Sur la droite, passez en revue votre récapitulatif de commande et appliquez votre code promo si vous en avez un.

   Les remises sont appliquées lors du traitement de la commande.
   {:note}
9. Après avoir lu les dispositions, cochez la case **J'ai lu et j'accepte les contrats de service tiers**.
10. Cliquez sur **Créer**. Votre nouvelle allocation de stockage est disponible en quelques minutes.

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).<br/><br/>Pour plus d'informations sur la limite des autorisations simultanées, voir [Foire aux questions](/docs/infrastructure/FileStorage?topic=FileStorage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:important}


## Connexion de votre nouveau stockage
{: #mountingvolumesPortal}

Lorsque votre demande de mise à disposition est terminée, autorisez vos hôtes à accéder au nouveau stockage et configurez votre connexion. Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Montage de {{site.data.keyword.filestorage_short}} sur Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montage de {{site.data.keyword.filestorage_short}} dans CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montage de {{site.data.keyword.filestorage_short}} sur CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec cPanel](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Configuration de {{site.data.keyword.filestorage_short}} en vue de la sauvegarde avec Plesk](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [Montage de volume {{site.data.keyword.filestorage_short}} sur des hôtes ESXi](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Considérations relatives à la reprise après incident

Pour éviter toute perte de données et garantir la continuité opérationnelle, prévoyez de répliquer vos serveurs et votre stockage dans un autre centre de données. La réplication permet de synchroniser vos données entre deux emplacements différents selon votre planning d'instantané. Pour plus d'informations, voir [Réplication de données](/docs/infrastructure/FileStorage?topic=FileStorage-replication).

Si vous voulez cloner votre volume et l'utiliser indépendamment du volume d'origine, voir [Création d'un volume de blocs dupliqué](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).

## Identification des volumes {{site.data.keyword.filestorage_short}} sur la facture

Tous les partages de fichiers apparaissent sur votre facture sous forme de lignes d'article. "Endurance Storage Service" s'affiche pour les volumes de type Endurance et "Performance Storage Service" pour les volumes de type Performance. Le taux varie en fonction de votre niveau de stockage. Vous pouvez développer Endurance ou Performance pour voir qu'il s'agit de {{site.data.keyword.filestorage_short}}.
