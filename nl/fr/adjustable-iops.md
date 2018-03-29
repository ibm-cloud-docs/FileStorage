---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# IOPS ajustables

Cette nouvelle fonctionnalité permet aux utilisateurs d'{{site.data.keyword.filestorage_full}} d'ajuster les opérations d'entrée-sortie par seconde (IOPS) de leur volume {{site.data.keyword.filestorage_short}} à la volée, sans qu'il soit nécessaire de créer un doublon ou de migrer manuellement les données vers un nouveau stockage. Les utilisateurs ne sont confrontés à aucune indisponibilité, ni manque d'accès au stockage lorsque l'ajustement a lieu. 

La facturation du stockage est mise à jour : la différence calculée au prorata du nouveau prix est ajoutée dans le cycle de facturation en cours et le nouveau montant total est ensuite facturé lors du cycle de facturation suivant. 

Cette fonctionnalité est disponible uniquement dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html). 

## Pourquoi bénéficier des IOPS ajustables

- Gestion des coûts : certains clients ont besoin d'un nombre élevé d'IOPS uniquement pendant les pics d'utilisation. Par exemple, un grand magasin de détail connaît un pic d'utilisation pendant les jours fériés et peut alors souhaiter bénéficier d'un nombre d'IOPS plus élevé sur son stockage que par rapport au milieu de l'été. Cette fonctionnalité lui permet de gérer ses coûts et de payer pour un nombre d'IOPS plus élevé uniquement en cas de réel besoin. 

## Existe-t-il des limitations ?

Cette fonctionnalité est disponible uniquement pour un stockage mis à disposition dans des [centres de données](new-ibm-block-and-file-storage-location-and-features.html) dotés de capacités améliorées.

Les clients ne peuvent pas basculer entre les volumes Endurance et Performance lorsqu'ils ajustent leurs IOPS. Les utilisateurs peuvent indiquer un nouveau niveau d'IOPS pour leur stockage en fonction des restrictions/critères suivants : 

- Si le volume d'origine est de type Endurance avec un niveau de 0,25, il n'est pas possible de le mettre à jour. 
- Si le volume d'origine est de type Performance avec un niveau < 0,30 IOPS/Go, les options disponibles incluent uniquement les combinaisons de taille et d'IOPS qui génèrent un niveau < 0,30 IOPS/Go. 
- Si le volume d'origine est de type Performance avec un niveau >= 0,30 IOPS/Go, les options disponibles incluent uniquement les combinaisons de taille et d'IOPS qui génèrent un niveau >= 0,30 IOPS/Go. (Taille supérieure ou égale au volume d'origine)

## Comment l'ajustement des IOPS affecte-t-il la réplication ? 

Si la réplication est activée sur le volume, la réplique est automatiquement mise à jour afin de correspondre à la sélection des IOPS du volume principal.  

## Comment ajuster les IOPS sur mon stockage ?

1. Sur le portail {{site.data.keyword.slportal}}, cliquez sur **Stockage** > **{{site.data.keyword.filestorage_short}}** OU, dans le Catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.filestorage_short}}**.
2. Sélectionnez le volume dans la liste et cliquez sur **Actions** > **Modifier le volume**
3. Sous les options d'IOPS de stockage, effectuez une nouvelle sélection : 
    - Endurance (IOPS hiérarchisées) : sélectionnez un niveau d'IOPS supérieur à 0,25 IOPS/Go de votre stockage. Vous pouvez augmenter le niveau d'IOPS à tout moment. Toutefois, vous ne pouvez le diminuer qu'une seule fois par mois. 
    - Performance (IOPS allouées) : indiquez la nouvelle option d'IOPS pour votre stockage en saisissant une valeur comprise entre 100 et 48 000 IOPS. (Tenez compte des limites spécifiques requises par la taille dans le formulaire de commande.) 
4. Vérifiez votre sélection et la nouvelle tarification. 
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service**, puis cliquez sur **Valider la commande**.
6. Votre nouvelle allocation de stockage devait être disponible en quelques minutes.
