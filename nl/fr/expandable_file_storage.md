---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}

# Extension de la capacité de partage de fichiers

Cette nouvelle fonctionnalité permet aux utilisateurs de {{site.data.keyword.filestorage_full}} d'étendre immédiatement la taille de leur stockage {{site.data.keyword.filestorage_short}} par incréments de Go jusqu'à 12 To. Ils n'ont pas besoin de créer un doublon ou de faire migrer manuellement les données vers un volume plus grand. Les utilisateurs ne sont confrontés à aucune indisponibilité, ni manque d'accès au stockage lorsque le redimensionnement a lieu.

La facturation du volume est automatiquement mise à jour : la différence calculée au prorata du nouveau prix est ajoutée au cycle de facturation en cours. Ensuite, le nouveau montant total est facturé au cours du cycle de facturation suivant.

Cette fonctionnalité est disponible uniquement dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html).

## Avantages du stockage extensible

- **Gestion des coûts** : vous savez que vos données sont susceptibles de croître, mais vous avez besoin d'une quantité de stockage plus petite pour commencer. La possibilité d'extension permet aux clients de réduire les coûts de stockage au début, puis de les adapter à la croissance en fonction de leurs besoins.  

- **Augmentation des besoins en stockage** : les clients qui font face à une croissance rapide dépassant leur capacité de stockage doivent pouvoir augmenter rapidement et facilement la taille de leur stockage.

## Effets de l'extension de la capacité de stockage sur la réplication

L'extension du stockage principal entraîne le redimensionnement automatique de la réplique.

## Limitations

Cette fonctionnalité est disponible uniquement pour un stockage mis à disposition dans des [centres de données](new-ibm-block-and-file-storage-location-and-features.html) dotés de capacités améliorées. Un stockage chiffré qui est mis à disposition dans ces centres de données peut être augmenté à hauteur de 12 To.

Les limitations de taille existantes pour le stockage {{site.data.keyword.filestorage_short}} qui a été mis à disposition avec l'option Endurance sont toujours applicables (jusqu'à 4 To pour un niveau de 10 IOPS et jusqu'à 12 To pour tous les autres niveaux).

## Redimensionnement du stockage

1. Sur le portail {{site.data.keyword.slportal}}, cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** OU, dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Sélectionnez le volume dans la liste et cliquez sur **Actions** > **Modifier le volume**
3. Entrez la nouvelle taille de stockage en Go.
4. Vérifiez votre sélection et la nouvelle tarification.
5. Cliquez sur **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis sur **Passer une commande**.
6. Votre nouvelle allocation de stockage est disponible en quelques minutes.
