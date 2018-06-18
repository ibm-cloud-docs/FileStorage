---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacité de partage de fichiers extensible

Cette nouvelle fonctionnalité permet aux utilisateurs d'{{site.data.keyword.filestorage_full}} d'étendre immédiatement la taille de leur stockage {{site.data.keyword.filestorage_short}} en incréments de Go jusqu'à 12 To, sans qu'il soit nécessaire de créer un doublon ou de faire migrer manuellement les données vers un volume plus grand.  Les utilisateurs ne sont confrontés à aucune indisponibilité, ni manque d'accès au stockage lorsque le redimensionnement a lieu. 

La facturation du volume est automatiquement mise à jour : la différence calculée au prorata du nouveau prix est ajoutée au cycle de facturation en cours. Ensuite, le nouveau montant total est facturé au cours du cycle de facturation suivant. 

Cette fonctionnalité est disponible uniquement dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html). 

## Pourquoi bénéficier des partages de fichiers extensibles ?

- **Gestion des coûts** : vous savez peut-être que vos données sont susceptibles de croître, mais vous avez besoin d'une quantité de stockage plus petite pour commencer. La possibilité d'extension permet aux utilisateurs de réduire les coûts de stockage au début, puis de les adapter à la croissance en fonction de leurs besoins.  

- **Augmentation des besoins en stockage** : les clients qui font face à une croissance rapide dépassant leur capacité de stockage doivent pouvoir augmenter rapidement et facilement la taille de leur stockage.

## Comment le redimensionnement du stockage affecte-t-il la réplication ?

L'extension du stockage principal entraîne le redimensionnement automatique de la réplique.

## Existe-t-il des limitations ?

Cette fonctionnalité est disponible uniquement pour un stockage mis à disposition dans des [centres de données](new-ibm-block-and-file-storage-location-and-features.html) dotés de capacités améliorées. Le stockage chiffré mis à disposition dans ces centres de données peut être augmenté à hauteur de 12 To. 

Les limitations de taille existantes pour un stockage {{site.data.keyword.filestorage_short}} mis à disposition avec l'option Endurance sont toujours applicables (jusqu'à 4 To pour un niveau de 10 IOPS et jusqu'à 12 To pour tous les autres niveaux).

## Comment savoir si mon stockage mis à disposition est extensible ?

Un stockage mis à disposition avec des capacités améliorées est toujours chiffré au repos. Si votre stockage est marqué par une icône en forme de verrou dans l'interface utilisateur du portail, cela signifie qu'il est éligible pour l'extension. 

## Comment redimensionner mon stockage ?

1. Sur le portail {{site.data.keyword.slportal}}, cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Sélectionnez le volume dans la liste et cliquez sur **Actions** > **Modifier le volume**
3. Entrez la nouvelle taille de stockage en Go.
4. Vérifiez votre sélection et la nouvelle tarification.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.
6. Votre nouvelle allocation de stockage devait être disponible en quelques minutes.

