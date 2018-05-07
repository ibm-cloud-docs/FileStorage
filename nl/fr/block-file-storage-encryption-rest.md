---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Sécurisation de vos données - Chiffrement au repos géré par le fournisseur 

{{site.data.keyword.BluSoftlayer_full}} prend au sérieux les besoins en matière de sécurité et comprend l'importance du chiffrement des données afin de les sécuriser. Grâce au chiffrement géré par le fournisseur, {{site.data.keyword.filestorage_full}} mis à disposition avec un niveau Endurance ou Performance offre par défaut une fonction de chiffrement sans coût supplémentaire et sans impact sur les performances.

Le chiffrement au repos géré par le fournisseur utilise les protocoles suivants conformes aux normes de l'industrie :

* Chiffrement AES-256 conforme aux normes de l'industrie
* Gestion en interne des clés via le protocole conforme aux normes de l'industrie KMIP (Key Management Interoperability Protocol)
* Conformité du stockage avec la norme Federal Information Processing Standard (FIPS) Publication 140-2 validée pour les lois Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA), Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386) et la directive européenne 95/46/CE sur la protection des données personnelles

## Chiffrement au repos pour le stockage des instantanés et le stockage répliqué  

Tous les instantanés et toutes les répliques d'un stockage de fichier chiffré sont également chiffrés par défaut. Il est possible de désactiver cette fonctionnalité volume par volume.

## Mise à disposition du stockage sans chiffrement

La fonction de chiffrement au repos gérée par le fournisseur est disponible uniquement pour un stockage {{site.data.keyword.filestorage_short}} mis à disposition dans des centres de données sélectionnés, auxquels de nouveaux centres de données sont régulièrement ajoutés. La totalité du stockage mis à disposition dans ces centres de données est automatiquement doté du chiffrement des données au repos. Cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html) pour afficher la liste des centres de données dans lesquels le chiffrement {{site.data.keyword.filestorage_short}} des données au repos est disponible.


Lorsque vous commandez votre stockage {{site.data.keyword.filestorage_short}}, sélectionnez un centre de données marqué d'un astérisque (`*`) et assorti d'un message indiquant que le chiffrement est disponible. Une icône en forme de verrou figure à droite du Nom LUN/Nom du volume pour indiquer qu'il est chiffré. Voir la Figure 1.

![L'icône en forme de verrou indique que le numéro d'unité logique est chiffré](/images/encryptedstorage.png)
<caption>Figure 1. Exemple d'icône en forme de verrou indiquant que le volume est chiffré.</caption>



**Notez** qu'un stockage non chiffré mis à disposition avant la mise à niveau du centre de données **n'est pas** automatiquement chiffré. Si vous disposez d'un stockage non chiffré dans un centre de données mis à niveau, vous devez créer un nouveau volume et procéder à la migration des données. Vous trouverez de l'aide à ce sujet dans l'article suivant :

* [Migration de stockage de fichier dans des centres de données mis à niveau](migrate-file-storage-encrypted-file-storage.html)
