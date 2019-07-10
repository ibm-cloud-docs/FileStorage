---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: File Storage, file storage, NFS, disaster recovery, duplicate volume, replica volume, failover, failback,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Reprise après incident - Basculement avec un volume principal accessible
{: #dr-accessible}

En cas de défaillance catastrophique ou d'un incident entraînant une indisponibilité sur le site principal avec le stockage principal toujours accessible, les clients peuvent effectuer les actions suivantes pour accéder rapidement à leurs données sur le site secondaire.

Avant de lancer le basculement, vérifiez que toutes les autorisations d'hôte sont en place.

Les hôtes et les volumes autorisés doivent figurer dans le même centre de données. Par exemple, vous ne pouvez pas avoir un volume de réplique à Londres et un hôte à Amsterdam. ils doivent se trouver tous les deux à Londres ou à Amsterdam.
{:note}

1. Connectez-vous à la [console {{site.data.keyword.cloud}}](https://{DomainName}/catalog){: external} et cliquez sur l'icône **menu** dans l'angle supérieur gauche. Sélectionnez **Infrastructure classique**.
2. Cliquez sur votre volume source ou cible à partir de la page **{{site.data.keyword.filestorage_short}}**.
3. Cliquez sur **Réplique**.
4. Faites défiler l'écran vers le bas jusqu'au cadre **Autoriser les hôtes** et cliquez sur **Autoriser les hôtes** à droite.
5. Mettez en évidence l'hôte qui doit être autorisé pour les réplications. Pour sélectionner plusieurs hôtes, utilisez la touche ctrl et cliquez sur les hôtes concernés.
6. Cliquez sur **Soumettre**. En l'absence d'hôte, vous êtes invité à acheter des ressources de calcul dans le même centre de données.

## Démarrage d'un basculement depuis un volume vers sa réplique

Dans le cas d'un événement d'échec, vous pouvez initier un **basculement** vers votre volume de destination, ou volume cible. Le volume cible devient actif. Le dernier instantané répliqué avec succès est activé et le volume est alors disponible pour le montage. Toutes les données écrites sur le volume source depuis le cycle de réplication précédent sont perdues. Une fois le basculement démarré, la relation de réplication est inversée. Votre volume cible devient votre volume source, et le volume source précédent devient votre cible, comme indiqué par le **Nom du volume** suivi de **REP**.

Les basculements sont démarrés sous **Stockage**, **{{site.data.keyword.filestorage_short}}** dans la [console {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

Avant d'exécuter ces étapes, déconnectez le volume. Si vous omettez cette étape, des données seront endommagées et perdues.
{:important}

1. Cliquez sur votre volume actif ("source").
2. Dans l'angle supérieur droit, cliquez sur **Réplique**, puis sur **Actions**.
3. Sélectionnez **Basculement**.

   Un message doit s'afficher pour vous indiquer que le basculement est en cours. En outre, une icône apparaît en regard de votre volume sur **{{site.data.keyword.filestorage_short}}** pour indiquer qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue affichant la transaction. L'icône disparaît une fois la transaction terminée. Durant le processus de basculement, les actions liées à la configuration sont accessibles en lecture seule. Vous ne pouvez pas éditer de planning d'instantané, ni modifier l'espace d'instantané. L'événement est consigné dans l'historique des réplications.<br/> Lorsque le volume cible est opérationnel, vous obtenez un autre message. Le nom de volume de votre volume source d'origine est mis à jour afin de se terminer par "REP" et son statut passe à Inactif.
   {:note}
4. Cliquez sur **Tout afficher ({{site.data.keyword.filestorage_short}})**.
5. Cliquez sur votre volume actif (anciennement votre volume cible). Ce volume a désormais le statut **Actif**.
6. Montez votre volume de stockage sur l'hôte et associez-les. Pour plus d'informations, voir [Connexion de votre nouveau stockage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#mountingstorage).


## Démarrage d'une reprise par restauration depuis un volume vers sa réplique

Une fois votre volume source d'origine réparé, vous pouvez démarrer une reprise par restauration contrôlée vers le volume source d'origine. Dans une reprise par restauration contrôlée,

- le volume source actif est mis hors ligne ;
- un instantané est pris ;
- le cycle de réplication est mené à bien ;
- l'instantané de données tout juste pris est activé ;
- et le volume source est activé pour le montage.

Une fois la reprise par restauration démarrée, la relation de réplication est inversée. Votre volume source est restauré en tant que volume source, et votre volume cible redevient le volume cible, comme indiqué par le **Nom du volume** suivi de **REP**.

Les reprises par restauration sont démarrées sous **Stockage**, **{{site.data.keyword.filestorage_short}}** dans la [console {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

1. Cliquez sur votre volume actif ("cible").
2. Dans l'angle supérieur droit, cliquez sur **Réplique**, puis sur **Actions**.
3. Sélectionnez **Reprise par restauration**.

   Un message doit s'afficher pour vous indiquer que la reprise par restauration est en cours. En outre, une icône apparaît en regard de votre volume sur **{{site.data.keyword.filestorage_short}}** pour indiquer qu'une transaction active est en cours. Survolez cette icône pour ouvrir une boîte de dialogue affichant la transaction. L'icône disparaît une fois la transaction terminée. Durant le processus de reprise par restauration, les actions liées à la configuration sont accessibles en lecture seule. Vous ne pouvez pas éditer de planning d'instantané, ni modifier l'espace d'instantané. L'événement est consigné dans l'historique des réplications.
   {:note}
4. Dans l'angle supérieur droit, cliquez sur **Afficher tout {{site.data.keyword.filestorage_short}}**.
5. Cliquez sur votre volume actif ("source").
6. Montez votre volume de stockage sur l'hôte et associez-les. Pour plus d'informations, voir [Connexion de votre nouveau stockage](/docs/infrastructure/FileStorage?topic=FileStorage-getting-started#mountingstorage).
