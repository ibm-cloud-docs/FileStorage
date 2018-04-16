---
 
copyright:
  years: 2015, 2018
lastupdated: "2018-04-16"
 
---

{:shortdesc: .shortdesc} 
{:new_window: target="_blank"}

# Utilisation de la réplication

La réplication utilise l'un de vos plannings d'instantané pour copier automatiquement des instantanés sur un volume de destination dans un centre de données distant. Les copies peuvent être récupérées sur le site distant en cas d'endommagement des données ou de catastrophe. 

Les répliques vous permettent

- d'effectuer rapidement une reprise après un échec du site et d'autres incidents en basculant sur le volume de destination,
- d'effectuer une bascule sur un point précis dans le temps dans la copie de reprise après incident. 

Avant d'effectuer une réplication, vous devez créer un planning d'instantané. Lorsque vous effectuez une bascule, vous "basculez l'interrupteur" depuis votre volume de stockage dans le centre de données principal vers le volume de destination du centre de données distant. Par exemple, votre centre de données principal peut se situer à Londres et votre centre de données secondaire à Amsterdam. Dans le cas d'un événement d'échec, vous basculez sur Amsterdam en vous connectant au volume désormais considéré comme le volume principal à partir d'une instance de traitement à Amsterdam. Une fois votre volume de Londres réparé, un instantané du volume d'Amsterdam est pris afin de permettre le retour à Londres avec le volume de Londres à nouveau considéré comme le volume principal à partir d'une instance de traitement située à Londres. 

 
**Remarque :** Sauf indication contraire, ces étapes sont identiques pour {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_full}}.

## Comment déterminer le centre de données distant de mon volume de stockage répliqué ? 

Les centres de données d'{{site.data.keyword.BluSoftlayer_full}} du monde entier ont été appariés en combinaisons principal-distant.
Pour obtenir la liste complète de la disponibilité des centres de données et des cibles de réplication, reportez-vous au Tableau 1.
Il est à noter que certaines villes, comme Dallas, San Jose, Washington D.C. et Amsterdam disposent de plusieurs centres de données. 


<table cellpadding="1" cellspacing="1">
	<caption>Tableau 1</caption>
	<tbody>
		<tr>
			<td><strong>EU 1</strong><sup><img src="/images/numberone.png" alt="1" /></sup></td>
			<td><strong>EU 2</strong></td>
			<td><strong>Amérique Latine/du Sud</strong></td>
			<td><strong>Canada</strong></td>
			<td><strong>Europe</strong></td>
			<td><strong>Asie Pacifique</strong></td>
			<td><strong>Australie</strong></td>
		</tr>
		<tr>
			<td>DAL01<br />
				DAL05<br />
				DAL06<br />
				HOU02<br />
				SJC01<br />
				WDC01<br />
				<br />
				<br />
				<br />
			</td>
			<td>SJC03<br />
			       SJC04<br />
			       WDC04<br />
			       WDC06<br />
			       WDC07<br />
			       DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br />
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
				LON02<br />
				LON04<br />
				LON06<br />
				OSL01<br />
				PAR01<br />
				MIL01<br />
			</td>
			<td>HKG02<br />
				TOK02<br />
				SNG01<br />
				SEO01<br />
                                CHE01<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br />
			</td>
		</tr>
		<tr>
			<td colspan="100%"><p><sup><img src="/images/numberone.png" alt="1" /></sup>Les centres de données de ces régions ou comportant une indication spécifique au sein d'une région NE disposent PAS d'un stockage chiffré.<br /><strong>Remarque</strong> : Les centres de données avec stockage chiffré <strong>ne peuvent pas</strong> lancer de réplication avec pour cibles de réplique des centres de données ne permettant pas le chiffrement.</p>
			</td>
		</tr>
	</tbody>
</table>
 

## Comment créer une réplication initiale ?

Les réplications utilisent un planning d'instantané. Vous devez d'abord configurer un espace d'instantané et un planning d'instantané pour le volume source avant de pouvoir répliquer. Vous recevez des invites qui vous rappellent les besoins en espace à acheter ou la nécessité de configurer un planning si vous tentez de configurer la réplication sans avoir défini l'un ou l'autre de ces éléments. Les réplications sont gérées sous **Stockage** > **{{site.data.keyword.filestorage_short}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Cliquez sur votre volume de stockage.
2. Cliquez sur l'onglet **Réplique**, puis sur le lien **Acheter une réplication**.
   Sélectionnez un planning d'instantané existant que vous souhaitez voir respecter par vos réplications. La liste contient tous vos plannings d'instantané actifs.
**Remarque :** Vous ne pouvez sélectionner qu'une seul planning, même si vous désirez combiner des réplications horaires, quotidiennes et hebdomadaires. Tous les instantanés capturés depuis le cycle de réplication précédent sont répliqués quel que soit le planning d'origine.
3. Cliquez sur la flèche déroulante **Emplacement** et sélectionnez le centre de données qui sera votre site de reprise après incident. 
4. Cliquez sur **Continuer**.
5. Entrez un **Code Promo** le cas échéant et cliquez sur **Recalculer**. Les autres zones de la boîte de dialogue sont définies sur leur valeur par défaut. 
6. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.
 

## Comment éditer une réplication existante ?

Vous pouvez éditer votre planning de réplication et modifier votre espace de réplication à partir de l'onglet **Principal** ou **Réplique** sous **Stockage** > **{{site.data.keyword.filestorage_short}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

 

## Comment éditer un planning de réplication ?

Vous modifiez en réalité un planning d'instantané puisque votre planning de réplication repose sur un planning d'instantané existant. Pour modifier le planning de réplication, par exemple de Horaire en Hebdomadaire, vous devez annuler le planning de réplication et en configurer un nouveau.

Vous pouvez modifier le planning à partir de l'onglet **Principal** ou **Réplique**.

1. Cliquez sur le menu déroulant **Actions** dans l'onglet **Principal** ou **Réplique**.
2. Sélectionnez **Modifier le planning d'instantané**.
3. Dans le cadre Instantané sous Planning, recherchez le planning que vous utilisez pour la réplication. Modifiez le planning employé pour la réplication ; par exemple, si votre planning de réplication a pour valeur **Quotidienne**, vous pouvez changer l'heure à laquelle la réplication doit avoir lieu. 
4. Cliquez sur **Enregistrer**.
 

## Comment modifier l'espace de réplication ?

Votre espace d'image instantanée principal et votre espace de réplique doivent être identiques. Si vous modifiez l'espace sur l'onglet **Principal** ou **Réplique**, l'espace est automatiquement ajouté à vos centres de données source et de destination. Gardez à l'esprit que l'augmentation de l'espace d'image instantanée déclenche une mise à jour immédiate de la réplication. 

Cliquez sur le menu déroulant **Actions** dans l'onglet **Principal** ou **Réplique**. Sélectionnez **Ajouter de l'espace instantané supplémentaire**.
Sélectionnez la taille de stockage dans la liste, puis cliquez sur **Continuer**.
Entrez un **Code Promo** le cas échéant et cliquez sur **Recalculer**. Les autres zones de la boîte de dialogue sont définies sur leur valeur par défaut. Cochez la case J'ai lu et j'accepte l'intégralité du Contrat cadre de service, puis cliquez sur le bouton Valider la commande.
 

## Comment consulter mes volumes de réplique dans la liste des volumes ? 

Vous pouvez afficher vos volumes de réplication à partir de la page {{site.data.keyword.filestorage_short}} sous **Stockage** > **{{site.data.keyword.filestorage_short}}**. La zone Nom du volume indique le nom du volume principal, suivi de REP. Le Type est Endurance(Performance) – Réplique, l'Adresse cible a pour valeur N/A car le volume de réplique n'est pas monté dans le centre de données de la réplique et le Statut est Inactif.

 

## Comment consultez les détails d'un volume répliqué dans le centre de données de la réplique ? 

Vous pouvez afficher les détails du volume de réplique sur l'onglet **Réplique** sous **Stockage** > **{{site.data.keyword.filestorage_short}}**. Vous
pouvez également sélectionner le volume de réplique sur la page **{{site.data.keyword.filestorage_short}}** et cliquer sur l'onglet **Réplique**.

 

## Comment spécifier les autorisations de l'hôte avant de basculer vers le centre de données secondaire ? 

Les hôtes et les volumes autorisés doivent figurer dans le même centre de données. Vous ne pouvez pas avoir un volume de réplique à Londres et un hôte à Amsterdam ; ils doivent se trouver tous les deux à Londres ou à Amsterdam.

1. Cliquez sur votre volume source ou de destination sur la page **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur l'onglet **Réplique**.
3. Faites défiler la page jusqu'au cadre **Autoriser les hôtes** et cliquez sur le lien **Autoriser les hôtes** situé sur la droite de l'écran.
4. Mettez en évidence l'hôte qui doit être autorisé pour les réplications. Pour sélectionner plusieurs hôtes, utilisez la touche ctrl et cliquez sur les hôtes concernés. 
5. Cliquez sur **Soumettre**. En l'absence d'hôte, la boîte de dialogue vous permet d'acheter des ressources de traitement dans le même centre de données ; sinon, cliquez sur **Fermer**.
 

## Comment augmenter mon espace d'image instantanée dans le centre de données de réplique lorsque j'augmente l'espace dans le centre de données principal ? 

Les tailles de volume doivent être identiques pour les volumes de stockage principal et de réplique ; un volume ne doit pas être plus volumineux que l'autre. Lorsque vous augmentez votre espace d'image instantanée dans le volume principal, l'espace de réplique est automatiquement augmenté. Gardez à l'esprit que l'augmentation de l'espace d'image instantanée déclenche une mise à jour immédiate de la réplication. L'augmentation des deux volumes apparaîtra sous forme de lignes d'article dans votre facture et sera calculée au prorata si nécessaire. 

Cliquez [ici](snapshots.html) pour savoir comment augmenter votre espace d'image instantanée. 

 

## Comment lancer un basculement depuis un volume vers sa réplique ? 

Dans le cas d'un événement d'échec, l'action **Basculement** vous permet de lancer un basculement vers votre volume de destination, ou volume cible. Le volume cible devient actif, la dernière image instantanée répliquée avec succès est activée et le volume devient actif pour le montage. Toutes les données écrites sur le volume source depuis le cycle de réplication précédent sont détruites. Gardez à l'esprit qu'une fois le basculement lancé, la **relation de réplication est inversée**. Votre volume cible est désormais votre volume source et votre ancien volume source devient votre cible, comme indiqué par le **Nom LUN** suivi de **REP**.

Les basculements sont lancés sous **Stockage** > **{{site.data.keyword.filestorage_short}}** à partir du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Avant de lancer ce processus, il est recommandé de déconnecter le volume. Si vous omettez cette étape, des données seront endommagées ou perdues. **

1. Cliquez sur votre numéro d'unité logique actif ("source").
2. Cliquez sur l'onglet Réplique, puis sur le lien Actions situé dans l'angle supérieur droit. 
3. Sélectionnez Basculement.
   Vous recevez un message dans la partie supérieure de la page indiquant que le démarrage du basculement est en cours. Une icône apparaît également en regard de votre volume sur **{{site.data.keyword.filestorage_short}}**, indiquant qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue précisant la transaction. L'icône disparaît une fois la transaction terminée. Au cours du processus de basculement, les actions liées à la configuration sont en lecture seule ; vous ne pouvez pas éditer de planning d'instantané, ni modifier l'espace d'image instantanée, etc. L'événement est consigné dans l'historique des réplications.
   Un autre message vous indique quand votre volume cible est opérationnel. Le Nom LUN du volume source d'origine est suivi de REP et son Statut devient Inactif.
4. Cliquez sur le lien **Afficher Tout {{site.data.keyword.filestorage_short}}** dans l'angle supérieur droit.
5. Cliquez sur votre numéro d'unité logique actif (anciennement votre volume cible). Ce volume a désormais un statut **Actif**.
6. Montez votre volume de stockage sur l'hôte et associez-les. 
 

## Comment lancer une rétromigration depuis un volume vers sa réplique ? 

Une fois votre volume source d'origine réparé, l'action **Rétromigration** permet de lancer une rétromigration contrôlée vers ce dernier. Lors d'une rétromigration contrôlée, 

- le volume source actif est mis hors ligne ; 
- un instantané est pris ;
- le cycle de réplication est mené à bien ;
- l'image instantanée de données tout juste prise est activée ; 
- le volume source devient actif pour le montage. 

Gardez à l'esprit qu'une fois la rétromigration lancée, la **relation de réplication est à nouveau inversée**. Votre volume source est restauré en tant que volume source et votre volume cible devient à nouveau le volume cible, comme indiqué par le **Nom LUN** suivi de **REP**.

Les rétromigrations sont lancées sous **Stockage** > **{{site.data.keyword.filestorage_short}}** à partir du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}..

1. Cliquez sur votre numéro d'unité logique Endurance ("cible").
2. Cliquez sur l'onglet **Réplique**, puis sur le lien **Actions** dans l'angle supérieur droit.
3. Sélectionnez **Rétromigration**.
   Vous recevez un message dans la partie supérieure de la page indiquant que le démarrage de la rétromigration est en cours. Une icône apparaît également en regard de votre volume sur **{{site.data.keyword.filestorage_short}}**, indiquant qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue précisant la transaction. L'icône disparaît une fois la transaction terminée. Au cours du processus de rétromigration, les actions liées à la configuration sont en lecture seule ; vous ne pouvez pas éditer de planning d'instantané, ni modifier l'espace d'image instantanée, etc. L'événement est consigné dans l'historique des réplications.
   Un autre message vous indique quand votre volume source est opérationnel. Votre volume cible a désormais un statut Inactif.
4. Cliquez sur le lien **Afficher Tout {{site.data.keyword.filestorage_short}}** dans l'angle supérieur droit.
5. Cliquez sur votre numéro d'unité logique Endurance actif (source). Ce volume a désormais un statut **Actif**.
6. Montez votre volume de stockage sur l'hôte et associez-les. Cliquez [ici](provisioning-file-storage.html) pour obtenir des instructions.
 

## Comment afficher l'historique des réplications ? 

L'historique des réplications s'affiche dans le **Journal d'audit** de l'onglet **Compte** sous **Gérer**. Les volumes principal et de réplique affichent un historique des réplications identique, qui comprend : 

- le type de réplication (basculement ou rétromigration),
- le moment de son lancement,
- l'instantané utilisé pour la réplication,
- la taille de la réplication,
- le moment de sa fin.
 

## Comment annuler une réplication existante ?

Vous pouvez procéder à une annulation immédiatement ou à la date anniversaire, ce qui entraîne l'arrêt de la facturation. Il est possible d'annuler une réplication à partir de l'onglet **Principal** ou de l'onglet **Réplique**.

1. Cliquez sur le volume dans la page **{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur le menu déroulant **Actions** sur l'onglet **Principal** ou **Réplique**.
3. Sélectionnez **Annuler la réplique**.
4. Choisissez le moment de l'annulation : **Immédiatement** ou **Date Anniversaire**, puis cliquez sur **Continuer**.
5. Cochez la case **Je comprends les risques de perte de données liés à cette annulation** et cliquez sur **Annuler la réplique**.
 

## Comment annuler la réplication lorsque le volume principal est annulé ? 

Lorsqu'un volume principal est annulé, le planning de réplication et le volume figurant dans le centre de données de réplique sont supprimés. Vous pouvez annuler les répliques à partir de la page **{{site.data.keyword.filestorage_short}}**.

 1. Mettez en évidence votre volume sur la page **{{site.data.keyword.filestorage_short}}**.
 2. Cliquez sur le menu déroulant **Actions** et sélectionnez **Annuler pour {{site.data.keyword.filestorage_short}}**.
 3. Choisissez le moment de l'annulation du volume : **Immédiatement** ou **Date Anniversaire**, puis cliquez sur **Continuer**.
 4. Cochez la case **Je comprends les risques de perte de données liés à cette annulation*a* et cliquez sur **Annuler**.
