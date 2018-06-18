---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestion de {{site.data.keyword.filestorage_short}}

Vous pouvez gérer vos volumes {{site.data.keyword.filestorage_full}} grâce au portail {{site.data.keyword.slportal}}. Cet article fournit les instructions liées à la plupart des tâches courantes.

## Autorisation des hôtes pour l'accès à {{site.data.keyword.filestorage_short}}

Les hôtes autorisés sont les hôtes qui ont reçu des droits d'accès sur un volume spécifique. Sans autorisation d'hôte, vous ne pouvez pas accéder au stockage, ni l'utiliser depuis votre système. L'autorisation d'un hôte pour l'accès à votre volume génère un nom d'utilisateur et un mot de passe. 

**Remarque** : Vous ne pouvez autoriser et connecter que des hôtes qui résident dans le même centre de données que votre stockage. Si vous disposez de plusieurs comptes, vous ne pouvez pas autoriser un hôte d'un autre compte à accéder à votre stockage sur un autre compte. 

1. Cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}**, puis sur **Nom de volume**.
2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés** de la page.
3. Cliquez sur **Hôte autorisé** sur le côté droit de la page. Sélectionnez les hôtes qui peuvent accéder à ce volume en particulier.

 

## Affichage de la liste des hôtes autorisés sur un volume {{site.data.keyword.filestorage_short}}

Pour afficher la liste des hôtes autorisés sur un volume, procédez comme indiqué ci-après.

1. Cliquez sur **Stockage > {{site.data.keyword.filestorage_short}}**, puis sur **Nom du volume**.
2. Faites défiler la page vers le bas jusqu'à la section **Hôtes autorisés**.

Cette section affiche la liste des hôtes actuellement autorisés à accéder au volume.


## Affichage des volumes {{site.data.keyword.filestorage_short}} sur lesquels un hôte est autorisé

Vous pouvez afficher les volumes auxquels un hôte peut accéder, y compris les informations nécessaires pour l'établissement d'une connexion (Nom du volume, Type de stockage, Adresse cible, Capacité et Emplacement) :

1. Cliquez sur **Unités** > **Liste des unités** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, puis cliquez sur l'unité appropriée.
2. Sélectionnez l'onglet Stockage.

La liste des volumes de stockage auxquels cet hôte particulier a accès s'affiche (les volumes sont regroupés par type de stockage : bloc, fichier, autre). Les menus **Action** respectifs vous permettent d'autoriser davantage de stockage ou de retirer l'accès.

 

## Montage et démontage de {{site.data.keyword.filestorage_short}}

Vous pouvez vous reporter aux informations relatives aux points de montage fournies dans la vue ** Détails du volume** pour monter {{site.data.keyword.filestorage_short}} à partir d'un hôte.

Reportez-vous à l'article suivant qui comporte des détails sur le montage et le démontage de {{site.data.keyword.filestorage_short}} à partir d'un hôte : [Accès à {{site.data.keyword.filestorage_short}} sur Linux](accessing-file-storage-linux.html).

 

## Révocation de l'accès d'un hôte à {{site.data.keyword.filestorage_short}}

Si vous souhaitez ne plus autoriser un hôte à accéder à un volume de stockage en particulier, vous pouvez révoquer l'accès. Lors de la révocation de l'accès, la connexion hôte est supprimée du volume et ni le système d'exploitation, ni les applications ne peuvent alors communiquer avec le volume. 

**Remarque :** Pour éviter les problèmes côté hôte, démontez le volume de stockage de votre système d'exploitation avant de révoquer l'accès afin de prévenir tout incident lié à des unités manquantes ou à l'altération des données.

Vous pouvez révoquer l'accès à partir de Stockage dans la Liste des unités ou dans les vues Stockage.

### Révocation de l'accès à partir de la Liste des unités :

1. Cliquez sur **Unités** > **Liste des unités** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, puis cliquez deux fois sur l'unité appropriée.
2. Sélectionnez l'onglet **Stockage**.
3. La liste des volumes de stockage auxquels cet hôte particulier a accès s'affiche (les volumes sont regroupés par type de stockage : bloc, fichier, autre). Sélectionnez le menu **Action** adéquat en regard du volume pour lequel vous souhaitez révoquer l'accès et cliquez sur **Révoquer le droit d'accès**.
4. Confirmez que vous souhaitez révoquer l'accès à un volume car cette action ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès au volume ou sur **Non** pour annuler l'action.

**Remarque :** Si vous souhaitez déconnecter plusieurs volumes d'un hôte spécifique, vous devez répéter l'action Révoquer le droit d'accès pour chaque volume.

 

### Révocation de l'accès à partir de la vue Stockage :
1. Cliquez sur **Stockage, {{site.data.keyword.filestorage_short}}** et sélectionnez le **Volume** pour lequel vous souhaitez révoquer l'accès.
2. Faites défiler la page jusqu'à la section **Hôtes autorisés**.
3. Cliquez sur **Actions** en regard de l'hôte dont l'accès doit être révoqué et sélectionnez **Révoquer le droit d'accès**. 
4. Confirmez que vous souhaitez révoquer l'accès à un volume car cette action ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès au volume ou sur **Non** pour annuler l'action.

**Remarque :** Si vous souhaitez déconnecter plusieurs hôtes d'un volume spécifique, vous devez répéter l'action Révoquer le droit d'accès pour chaque hôte.

 

## Annulation d'un volume de stockage

Si vous n'avez plus besoin d'un volume spécifique, vous pouvez l'annuler. Pour ce faire, il est d'abord nécessaire de révoquer l'accès à partir de tous les hôtes.

1. Cliquez sur **Stockage**>**{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur **Actions** correspondant au volume à annuler et sélectionnez **Annuler {{site.data.keyword.filestorage_short}}**.
3. Confirmez l'annulation du volume de manière immédiate ou à la date anniversaire de la mise à disposition du volume. Cliquez sur **Continuer** ou sur **Fermer**. 
**Remarque** : Si vous sélectionnez l'option d'annulation du volume à sa date anniversaire, vous pouvez annuler la demande d'annulation avant la date anniversaire.
4. Cochez la case d'accusé de réception et cliquez sur **Confirmer**.

 

## Affichage des détails d'un volume {{site.data.keyword.filestorage_short}} mis à disposition

Vous pouvez afficher un récapitulatif des informations clés concernant le volume de stockage sélectionné, ainsi que les fonctionnalités supplémentaires d'instantané et de réplication qui ont été ajoutées au stockage.

1. Cliquez sur **Stockage**>**{{site.data.keyword.filestorage_short}}**.
2. Cliquez sur le **Nom du volume** approprié dans la liste.
