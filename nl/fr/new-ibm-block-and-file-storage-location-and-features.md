---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Nouveaux emplacements et nouvelles fonctions de {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} propose une nouvelle version de {{site.data.keyword.blockstorageshort}} et d'{{site.data.keyword.filestorage_full}}.  Le nouveau stockage est disponibles dans des centres de données sélectionnés, et est sécurisé par un stockage flash à des niveaux d'IOPS plus élevés avec un chiffrement au niveau du disque pour les données au repos. La totalité du stockage mis à disposition dans les centres de données sélectionnés est automatiquement mise à disposition avec la nouvelle version de {{site.data.keyword.blockstorageshort}} et de {{site.data.keyword.filestorage_short}}.

**Remarque :** Le point de montage NFS des nouveaux volumes a été modifié. Pour plus de détails, voir **Nouveau point de montage des volumes {{site.data.keyword.filestorage_short}} chiffrés** ci-dessous.

Le nouveau stockage {{site.data.keyword.filestorage_short}} est actuellement disponible dans les régions/centres de données suivants (cette disponibilité sera bientôt étendue à d'autres centres de données) :
<table style="width:100%;">
	<caption>Disponibilité dans les centres de données</caption>
	<tbody>
		<tr>
			<td><strong>EU 2</strong></td>
			<td><strong>UE</strong></td>
			<td><strong>Australie</strong></td>
			<td><strong>Canada</strong></td>
			<td><strong>Amérique latine</strong></td>
			<td><strong>Asie Pacifique</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br /><br /></p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				FRA04<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
				SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


Le nouveau stockage inclut les nouvelles fonctions et capacités suivantes :

-  [Chiffrement géré par le fournisseur pour les données au repos](block-file-storage-encryption-rest.html). Tous les volumes {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} sont automatiquement mis à disposition en mode chiffré sans coût supplémentaire.
-  Option de niveau de 10 IOPS par Go. Un nouveau niveau a été ajouté au stockage {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} de type Endurance pour la prise en charge des charges de travail les plus exigeantes.
-  Sécurisation de la totalité du stockage par stockage flash.  Le stockage {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} mis à disposition avec un type Endurance ou Performance à un niveau de 2 IOPS par Go ou plus est entièrement sécurisé par un stockage flash.
-  Prise en charge des instantanés et de la réplication dans les volumes {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} mis à disposition avec l'option Endurance ou Performance.
-  Options de facturation horaire ajoutée pour un stockage prévu pour une utilisation inférieure à un mois complet. 
-  Jusqu'à 48 000 IOPS pour un stockage {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}} mis à disposition avec l'option Performance.
-  Possibilité d'ajuster les taux d'IOPS dans le but d'améliorer les performances en cas de modifications saisonnières de la charge. Pour en savoir plus sur cette fonction, cliquez [ici](adjustable-iops.html).
-  Création d'un nouveau clone de vos données avec la [fonction de duplication de volume {{site.data.keyword.filestorage_short}}](how-to-create-duplicate-volume.html).
- Possibilité d'extension à la volée du stockage en incréments de Go jusqu'à 12 To, sans qu'il soit nécessaire de créer un doublon ou de migrer manuellement les données vers un volume plus grand. Pour en savoir plus sur cette fonction, cliquez [ici](expandable_file_storage.html).

## Nouveau point de montage des volumes {{site.data.keyword.filestorage_short}} chiffrés

Tous les volumes {{site.data.keyword.filestorage_short}} chiffrés mis à disposition dans ces centres de données ont un point de montage différent de celui des volumes non chiffrés.  Pour vérifier que vous utilisez le bon point de montage pour les volumes de stockage de fichier chiffrés et non chiffrés, vous pouvez afficher les informations sur le point de montage sur la page Détails du volume de l'interface utilisateur ou accéder au point de montage correct via un appel d'API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Revenez ici pour savoir si des centres de données supplémentaires ont été mis à niveau et si de nouvelles fonctions et capacités disponibles ont été ajoutées pour {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}}.
