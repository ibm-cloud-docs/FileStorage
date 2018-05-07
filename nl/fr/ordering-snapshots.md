---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Commande d'instantanés

Pour créer des instantanés de votre volume de stockage, que ce soit de manière automatisée ou manuelle, vous devez acheter de l'espace afin de les conserver. Vous pouvez acquérir une capacité maximale égale à la quantité de votre volume de stockage (lors de l'achat initial du volume ou ultérieurement en suivant les étapes ci-après).

1. Accédez à votre stockage via l'onglet **Stockage** > **{{site.data.keyword.filestorage_short}}** du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Dans le cadre **Instantanés**, cliquez sur le lien **Ajouter de l'espace d'instantané**.
3. Dans le cadre **Instantanés**, cliquez sur **Achetez maintenant de l'espace pour les instantanés**.
3. Sélectionnez la quantité d'espace dont vous avez besoin en cliquant sur le bouton radio en regard de la quantité souhaitée.
4. Cliquez sur **Continuer**.
5. Entrez un code promo le cas échéant et cliquez sur **Recalculer**. Les zones **Prix pour cette commande** et **Vérification de la commande** indiquent les valeurs par défaut.
6. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**. Votre espace d'image instantanée est mis à disposition en quelques minutes.

## Comment déterminer la quantité d'espace d'image instantanée à commander ? 

Malheureusement, il n'existe pas de valeur recommandée autre que celle qui repose sur votre application. En règle générale, l'espace d'image instantanée est utilisé par les instantanés en fonction de deux critères :
- la quantité de modifications apportées à votre système de fichiers actif et  
- la durée de conservation envisagée des instantanés.   

La principale méthode de calcul de la quantité d'espace nécessaire est la suivante : **(taux de modification)** x **(nombre d'heures/jours/semaines/mois de conservation)**.  
**Remarque** : Le premier instantané utilise une quantité négligeable d'espace car il s'agit uniquement d'une copie des métadonnées (pointeurs) indiquant les blocs du système de fichiers actif.  

Un volume comportant un nombre important de modifications des données (par exemple, un taux élevé de modifications de la base de données) et une longue période de conservation des instantanés aura besoin de davantage d'espace pour les instantanés qu'un volume doté d'un nombre moyen de modifications (par exemple, un magasin de données de machine virtuelle) et d'un planning de conservation des instantanés plus modéré. 

Dans l'instance d'un volume qui comprend 500 Go de données réelles, si vous avez pris 12 instantanés par heure et que vous avez observé 1 % de modification entre chaque instantané, vous obtenez (5 Go de taux de modification) x (12 instantanés par heure) = 60 Go pour les instantanés. 

A l'inverse, si ces 500 Go de données réelles avec 12 instantanés par heure connaissent 10 % de modification toutes les heures, vous obtenez (50 Go de taux de modification) x (12 instantanés par heure) = 600 Go.

Vous devez donc tenir compte du taux de modification lorsque vous déterminez la quantité d'espace d'image instantanée dont vous avez besoin. Il influe en effet considérablement sur la quantité d'espace d'image instantanée. Même si la taille d'un volume est davantage susceptible d'entraîner un taux élevé de modification, un volume de 500 Go avec 5 Go de modification et un volume de 10 To avec 5 Go de modification génèrent une utilisation identique de l'espace d'image instantanée. 

De plus, pour la plupart des charges de travail, plus le volume est élevé, plus l'espace initial devant être défini pour les instantanés est réduit. Cela est principalement dû aux efficiences des données sous-jacentes de notre plateforme, ainsi qu'à la nature du fonctionnement des instantanés dans notre environnement. 


