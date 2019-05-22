---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-14"

keywords: File Storage, Endurance, Performance, IOPS, replication, billing, file storage, NFS,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# A propos de {{site.data.keyword.filestorage_short}}
{: #about}

{{site.data.keyword.filestorage_full}} est un système de stockage de fichiers {{site.data.keyword.filestorage_short}} NAS basé sur NFS, permanent, rapide et flexible. Cet environnement NAS vous permet d'avoir un contrôle total des fonctions et des performances de vos partages de fichiers. Les partages {{site.data.keyword.filestorage_short}} peuvent être connectés à un maximum de 64 unités autorisées via des connexions TCP/IP routées pour la résilience.
{:shortdesc}

## Fonctions
{: #FileStorageFeatures}

{{site.data.keyword.filestorage_short}} offre les meilleurs niveaux du marché en matière de durabilité et de disponibilité grâce à un ensemble de fonctions inégalé. {{site.data.keyword.filestorage_short}} est conçu avec les normes et les meilleures pratiques de l'industrie pour protéger l'intégrité des données. {{site.data.keyword.filestorage_short}} maintient la disponibilité lors des événements de maintenance et des défaillances imprévues et garantit une base de référence cohérente pour les performances.

Tirez parti des principales fonctionnalités de {{site.data.keyword.filestorage_short}} :

- **Base de référence cohérente pour les performances**
   - Fournie via l'allocation d'opérations d'entrée-sortie par seconde (IOPS) au niveau du protocole à des volumes individuels.
- **{{site.data.keyword.filestorage_short}}**
   - Disponible pour les partages NFS basés sur des fichiers.
- **Durabilité et résilience élevées**
   - Protège l'intégrité des données et maintient la disponibilité lors des événements de maintenance et des défaillances imprévues sans qu'il soit nécessaire de créer et de gérer des tableaux RAID au niveau du système d'exploitation
- **Chiffrement des données au repos** [(disponible dans des centres de données sélectionnés)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Chiffrement géré par le fournisseur pour les données au repos, sans coût supplémentaire
- **Stockage entièrement sécurisé par mémoire flash** [(disponible dans des centres de données sélectionnés)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Stockage flash pour les volumes mis à disposition à des niveaux supérieurs ou égaux à 2 IOPS/Go
- **Instantanés** [(disponibles dans des centres de données sélectionnés)](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - Capture des instantanés de données ponctuels de manière transparente.
- **Replication**  [(disponible dans des centres de données sélectionnés)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Disponible uniquement pour le stockage mis à disposition dans des [centres de données sélectionnés](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - Copie automatiquement des instantanés vers un centre de données {{site.data.keyword.BluSoftlayer_full}} partenaire.
- **Connectivité hautement disponible**
   - Utilise des connexions réseau redondantes pour accroître la disponibilité
   - Connexions TCP/IP routées par {{site.data.keyword.filestorage_short}} basé sur NFS
- **Accès simultané**
   - Autorise plusieurs hôtes (jusqu'à 64) à accéder simultanément aux volumes de fichiers
- **Cluster de bases de données**
   - Prend en charge des cas d'utilisation avancés, tels que des bases de données en clusters.

## Facturation

Vous pouvez choisir une facturation horaire ou mensuelle pour un volume de fichier. Le type de facturation sélectionné pour un numéro d'unité logique s'applique à son espace d'instantané et à ses répliques. Par exemple, si vous mettez à disposition un numéro d'unité logique avec une facturation horaire, tous les frais liés aux instantanés ou aux répliques seront facturés à l'heure. Si vous mettez à disposition un numéro d'unité logique avec une facturation mensuelle, tous les frais liés aux instantanés ou aux répliques sont facturés au mois.

Avec la **facturation horaire**, le nombre d'heures d'existence du volume de fichier sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](/docs/infrastructure/FileStorage?topic=FileStorage-news).

Avec la **facturation mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).


**Performance**
<table>
  <caption>Le tableau 1 contient les prix du stockage Performance avec une facturation mensuelle et horaire.</caption>
  <tr>
   <th>Prix mensuel</th>
   <td>0,10 $/Go + 0,07 $/IOP</td>
  </tr>
  <tr>
   <th>Prix horaire</th>
   <td>0,0001 $/Go + 0,0002 $/IOP</td>
  </tr>
</table>

**Endurance**
<table>
  <caption>Le tableau 2 contient les prix du stockage Endurance avec chaque niveau avec des options de facturation mensuelle et horaire.</caption>
  <tr>
   <th>Niveau d'IOPS</th>
   <th>0,25 IOPS/Go</th>
   <th>2 IOPS/Go</th>
   <th>4 IOPS/Go</th>
   <th>10 IOPS/Go</th>
  </tr>
  <tr>
   <th>Prix mensuel</th>
   <td>0,06 $/Go</td>
   <td>0,15 $/Go</td>
   <td>0,20 $/Go</td>
   <td>0,58 $/Go</td>
  </tr>
  <tr>
   <th>Prix horaire</th>
   <td>0,0001 $/Go</td>
   <td>0,0002 $/Go</td>
   <td>0,0003 $/Go</td>
   <td>0,0009 $/Go</td>
  </tr>
</table>



## Mise à disposition

Les volumes {{site.data.keyword.filestorage_short}} peuvent être mis à disposition à partir de 20 Go et jusqu'à 12 To avec deux options : <br/>
- Effectuez la mise à disposition avec des niveaux **Endurance** offrant des niveaux de performance prédéfinis et d'autres fonctionnalités telles que les instantanés et la réplication.
- Créez un environnement de **Performance** haute puissance avec des opérations d'entrée-sortie par seconde (IOPS) allouées.


### Mise à disposition avec des niveaux Endurance

{{site.data.keyword.filestorage_short}} Endurance est disponible avec quatre niveaux de performance d'IOPS permettant de prendre en charge divers besoins d'application. <br />

- L'option **0,25 IOPS par Go** est adaptée aux charges de travail peu exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.

- L'option **2 IOPS par Go** est adaptée à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données qui sauvegardent des applications Web ou des images de disques de machine virtuelle pour un hyperviseur.

- L'option **4 IOPS par Go** est adaptée aux charges de travail plus exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.

- L'option **10 IOPS par Go** est adaptée aux charges de travail les plus intensives, telles que celles créées par les bases de données NoSQL et le traitement de données pour Analytics. Ce niveau est disponible pour le stockage mis à disposition jusqu'à 4 To uniquement dans des [centres de données sélectionnés](/docs/infrastructure/FileStorage?topic=FileStorage-news).

Un volume de type Endurance de 12 To comporte un maximum de 48 000 IOPS disponibles.

Il est essentiel de choisir le niveau d'endurance adapté pour votre charge de travail. Il est également important d'utiliser la taille de bloc, la vitesse de connexion Ethernet et le nombre d'hôtes appropriés afin d'atteindre des performances maximales. La non-concordance de l'un de ces éléments peut avoir un impact important sur le débit généré.

### Mise à disposition avec Performance

Performance est une classe de {{site.data.keyword.filestorage_short}} conçue pour prendre en charge des applications avec un niveau élevé d'entrée/sortie et nécessitant un niveau de performance bien établi qui ne correspond pas à un niveau Endurance. Pour atteindre la performance prévue, il suffit d'allouer les IOPS au niveau du protocole à des volumes individuels. Des IOPS allant de 100 à 48 000 peuvent être mis à disposition avec des tailles de stockage de 20 Go à 12 To.

Pour {{site.data.keyword.filestorage_short}}, Performance est accessible et monté via une connexion NFS. {{site.data.keyword.filestorage_short}} est généralement utilisé lorsque le volume fait l'objet d'un accès simultané par plusieurs serveurs. Il est possible de commander des volumes avec des performances cohérentes en fonction des tailles et du nombre d'IOPS figurant dans le tableau 1 ; ces volumes peuvent être utilisés avec des systèmes d'exploitation Linux.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>Le tableau 3 présente les combinaisons de taille et d'IOPS possibles pour le stockage Performance.<br/><sup><img src="/images/numberone.png" alt="Note de bas de page" /></sup> Vous pouvez opter pour un nombre d'IOPS supérieur à 6 000 dans des centres de données sélectionnés.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Taille (Go)</th>
            <th>Nb min d'IOPS</th>
            <th>Nb max d'IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1 000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2 000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4 000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6 000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6 000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6 000 ou 10 000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>1 000</td>
            <td>100</td>
            <td>6 000 ou 20 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>2 000</td>
            <td>200</td>
            <td>6 000 ou 40 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>3 000 à 7 000</td>
            <td>300</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>8 000 à 9 000</td>
            <td>500</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>10 000 à 12 000</td>
            <td>1 000</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
</table>


Les volumes Performance sont conçus pour fonctionner d'une manière cohérente proche du niveau d'IOPS mis à disposition. La cohérence facilite le dimensionnement et la mise à l'échelle des environnements d'application avec un niveau de performance donné. De plus, il est possible d'optimiser un environnement en créant un volume avec le rapport idéal prix/performance.
