---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, locations, data centers

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Nouveaux emplacements et fonctions
{: #news}

{{site.data.keyword.BluSoftlayer_full}} propose une nouvelle version d'{{site.data.keyword.filestorage_full}}! Le nouveau stockage est disponible dans des centres de données sélectionnés, et est sécurisé par un stockage flash à des niveaux d'IOPS plus élevés avec un chiffrement au niveau du disque pour les données au repos. La totalité du stockage commandé dans les centres de données sélectionnés est automatiquement créée avec la nouvelle version de {{site.data.keyword.filestorage_short}}.

Le point de montage NFS des nouveaux volumes a été modifié. Pour plus de détails, voir [Nouveau point de montage des volumes {{site.data.keyword.filestorage_short}} améliorés](#new-mount-point-for-enhanced-file-storage-volumes).
{:important}

## Nouveaux emplacements
{: #new-locations}

La nouvelle version de {{site.data.keyword.filestorage_short}} est disponible dans les régions et centres de données suivants (cette disponibilité sera bientôt étendue à d'autres centres de données) :

<table role="presentation">
  <tr>
    <td><strong>EU 2</strong></td>
    <td><strong>UE</strong></td>
    <td><strong>Australie</strong></td>
    <td><strong>Canada</strong></td>
    <td><strong>Amérique latine</strong></td>
    <td><strong>Asie Pacifique</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
        SYD05<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*Le tableau 1 répertorie la disponibilité de nos centres de données. Chaque région correspond à une colonne. Certaines villes, comme Dallas, San José, Washington DC, Amsterdam, Francfort, Londres et Sydney possèdent plusieurs centres de données.*

## Nouvelles fonctions et fonctionnalités
{: #features}

- [Chiffrement géré par le fournisseur pour les données au repos](/docs/infrastructure/FileStorage?topic=FileStorage-encryption). <br/> Tous les volumes {{site.data.keyword.filestorage_short}} sont automatiquement mis à disposition en mode chiffré sans coût supplémentaire.
- Option de niveau de 10 IOPS par Go. <br/> Un nouveau niveau a été ajouté au stockage {{site.data.keyword.filestorage_short}} de type Endurance pour la prise en charge des charges de travail les plus exigeantes.
- Sécurisation de la totalité du stockage par stockage flash. <br/> Le stockage {{site.data.keyword.filestorage_short}} mis à disposition avec un type Endurance ou Performance à un niveau de 2 IOPS par Go ou plus est entièrement sécurisé par un stockage flash.
- Prise en charge des instantanés et de la réplication.
- Options de facturation horaire ajoutée pour un stockage prévu pour une utilisation inférieure à un mois complet.
- Jusqu'à 48 000 IOPS pour un stockage {{site.data.keyword.filestorage_short}} mis à disposition avec l'option Performance.
- Possibilité d'ajuster les taux d'IOPS dans le but d'améliorer les performances relatives aux modifications saisonnières de la charge. Pour en savoir plus sur cette fonction, cliquez [ici](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS).
- Possibilité de créer un clone de vos données grâce à la [fonction de duplication du volume de {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
- Possibilité d'extension immédiate du stockage en incréments de Go jusqu'à 12 To, sans qu'il soit nécessaire de créer un doublon ou de déplacer manuellement les données vers un volume plus grand. Pour en savoir plus sur cette fonction, cliquez [ici](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity).

## Nouveau point de montage des volumes {{site.data.keyword.filestorage_short}} améliorés

Tous les volumes {{site.data.keyword.filestorage_short}} améliorés mis à disposition dans ces centres de données ont un point de montage différent de celui des volumes non chiffrés. Pour vérifier que vous utilisez le bon point de montage pour les deux types de volume de stockage, vous pouvez afficher les informations sur le point de montage sur la page **Détails du volume** de la console. Vous pouvez également accéder au point de montage correct via un appel d'API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Pour pouvoir accéder à toutes les nouvelles fonctions, sélectionnez `Storage-as-a-Service Package 759` lorsque vous passez votre commande via l'API. Pour plus d'informations sur les commandes {{site.data.keyword.filestorage_short}} via l'API, voir [order_file_volume ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){:new_window}.
{:important}

Revenez ici pour savoir si d'autres centres de données ont été mis à niveau et si de nouvelles fonctions et capacités ont été ajoutées pour {{site.data.keyword.filestorage_short}}.
{:tip}
