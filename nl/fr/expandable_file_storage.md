---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-24"

keywords: File Storage, modify volume, NFS, file storage, expand capacity

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Extension de la capacité de partage de fichiers
{: #expandCapacity}

Cette nouvelle fonctionnalité permet aux utilisateurs de {{site.data.keyword.filestorage_full}} d'étendre immédiatement la taille de leur stockage {{site.data.keyword.filestorage_short}} par incréments de Go jusqu'à 12 To. Ils n'ont pas besoin de créer un doublon ou de faire migrer manuellement les données vers un volume plus grand. Les utilisateurs ne sont confrontés à aucune indisponibilité, ni manque d'accès au stockage lorsque le redimensionnement a lieu.

La facturation du volume est automatiquement mise à jour : la différence calculée au prorata du nouveau prix est ajoutée au cycle de facturation en cours. Ensuite, le nouveau montant total est facturé au cours du cycle de facturation suivant.

Cette fonctionnalité est disponible uniquement dans des [centres de données sélectionnés](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC).

## Avantages du stockage extensible

- **Gestion des coûts** : vous savez que vos données sont susceptibles de croître, mais vous avez besoin d'une quantité de stockage plus petite pour commencer. La possibilité d'extension permet aux clients de réduire les coûts de stockage au début, puis de les adapter à la croissance en fonction de leurs besoins.  

- **Augmentation des besoins en stockage** : les clients qui font face à une croissance rapide dépassant leur capacité de stockage doivent pouvoir augmenter rapidement et facilement la taille de leur stockage.

## Effets de l'extension de la capacité de stockage sur la réplication

L'extension du stockage principal entraîne le redimensionnement automatique de la réplique.

## Limitations
{: #limitsofextension}

Cette fonctionnalité est disponible uniquement pour un stockage mis à disposition dans des [centres de données](/docs/infrastructure/FileStorage?topic=FileStorage-selectDC) dotés de capacités améliorées. Un stockage chiffré qui est mis à disposition dans ces centres de données peut être augmenté à hauteur de 12 To.

Les limitations de taille existantes pour le stockage {{site.data.keyword.filestorage_short}} qui a été mis à disposition avec l'option Endurance sont toujours applicables (jusqu'à 4 To pour un niveau de 10 IOPS et jusqu'à 12 To pour tous les autres niveaux).

## Redimensionnement du stockage
{: #resizingsteps}

1. Accédez à la [console {{site.data.keyword.cloud}}](https://{DomainName}/){: external}. Dans le menu, sélectionnez **Infrastructure classique**. Cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Sélectionnez le volume dans la liste, cliquez sur **...** puis sélectionnez l'option de modification de partage****
3. Entrez la nouvelle taille de stockage en Go.
4. Vérifiez votre sélection et la nouvelle tarification.
5. Cliquez sur **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis sur **Passer une commande**.
6. Votre nouvelle allocation de stockage est disponible en quelques minutes.

Pour que le système d'exploitation reconnaisse l'espace de stockage supplémentaire, démontez et montez à nouveau le volume modifié.
{:tip}

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli file volume-modify --help
Usage: slcli file volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of file volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help      Show this message and exit.
```
