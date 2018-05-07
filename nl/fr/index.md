---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Initiation à {{site.data.keyword.filestorage_short}}

{{site.data.keyword.filestorage_full}} est un système de stockage de fichiers {{site.data.keyword.filestorage_short}} NAS basé sur NFS, permanent, rapide et flexible. Cet environnement NAS vous permet d'avoir un contrôle total des fonctions et des performances de vos partages de fichiers. Les partages {{site.data.keyword.filestorage_short}} peuvent être connectés à un maximum de 64 unités autorisées via des connexions TCP/IP routées pour la résilience.

{{site.data.keyword.filestorage_short}} offre les meilleurs niveaux du marché en matière de durabilité et de disponibilité avec un jeu de fonctions inégalé et est conçu avec les normes et les meilleures pratiques de l'industrie pour protéger l'intégrité des données et conserver leur disponibilité lors des événements de maintenance et les défaillances imprévues tout en garantissant une base de référence cohérente pour les performances.

Tirez parti des principales fonctionnalités de {{site.data.keyword.filestorage_short}} :

- **Base de référence cohérente pour les performances**
   - Fournie via l'allocation d'opérations d'entrée-sortie par seconde (IOPS) au niveau du protocole à des volumes individuels
- **{{site.data.keyword.filestorage_short}}**
   - Disponibles pour les partages NFS basés sur des fichiers
- **Durabilités et résiliences élevées**
   - Protège l'intégrité des données et maintient la disponibilité lors des événements de maintenance et des défaillances imprévues sans qu'il soit nécessaire de créer et de gérer des tableaux RAID au niveau du système d'exploitation
- **Chiffrement des données au repos** [(disponible dans des centres de données sélectionnés.)](new-ibm-block-and-file-storage-location-and-features.html)
   - Chiffrement géré par le fournisseur pour les données au repos sans coût supplémentaire
- **Stockage entièrement sécurisé par mémoire flash** [(disponible dans des centres de données sélectionnés.)](new-ibm-block-and-file-storage-location-and-features.html)
   - Stockage flash pour les volumes mis à disposition avec des options Endurance ou Performance supérieures ou égales à 2 IOPS/Go
- **Instantanés (si mise à disposition avec les options Endurance ou Performance dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html))**.
   - Captures des instantanés de données ponctuels de manière transparente
- **Réplication** (si mise à disposition avec les options Endurance ou Performance dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html).
   - Copie automatiquement les instantanés sur le centre de données {{site.data.keyword.BluSoftlayer_full}} partenaire
- **Connectivité hautement disponible**
   - Utilise des connexions réseau redondantes pour accroître la disponibilité (connexions TCP/IP routées par {{site.data.keyword.filestorage_short}} basé sur NFS)
- **Accès simultané**
   - Autorise plusieurs hôtes (jusqu'à 64) à accéder simultanément aux volumes de fichiers
- **Cluster de bases de données**
   - Prend en charge des cas d'utilisation avancés, comme un cluster de bases de données

## Facturation horaire/mensuelle

Vous pouvez choisir une facturation horaire ou mensuelle pour un volume de fichier. Le type de facturation sélectionné pour un numéro d'unité logique s'applique à son espace d'image instantanée et à ses répliques. Par exemple, si vous mettez à disposition un numéro d'unité logique avec une facturation horaire, tous les frais liés aux instantanés ou aux répliques seront facturés à l'heure. Si vous mettez à disposition un numéro d'unité logique avec une facturation mensuelle, tous les frais liés aux instantanés ou aux répliques seront facturés au mois. 

Avec la **facturation horaire**, le calcul du nombre d'heures d'existence du volume de fichier sur le compte s'effectue lors de la suppression du volume ou à la fin du cycle de facturation, à la première occurrence de l'un des deux événements.  La facturation horaire est un bon choix si vous avez besoin d'un stockage pour quelques jours ou pour moins d'un mois complet. La facturation horaire est disponible uniquement pour le stockage mis à disposition dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html). 

Avec la **facturation mensuelle**, le calcul du prix est calculé au prorata depuis la date de création jusqu'à la fin du cycle de facturation et la facturation est immédiate. Aucun remboursement n'est possible si un volume de fichier est supprimé avant la fin du cycle de facturation.  La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail utilisant des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).

 
### Performance :
<table>
 <tbody>
  <tr>
   <th>Prix mensuel</th>
   <td>0,10 $/Go + 0,07 $/E-S</td>
  </tr>
  <tr>
   <th>Prix horaire</th>
   <td>0,0001 $/Go + 0,0002 $/E-S</td>
  </tr>
  </tbody>
</table>
 
### Endurance :
<table>
 <tbody>
  <tr>
   <th>Niveau d'IOPS</th>
   <th>0,25 IOPS/Go</th>
   <th>2 IOPS/Go</th>
   <th>4 IOPS/Go</th>
   <th>10 IOPS/Go</th>
  </tr>
  <tr>
   <th>Prix mensuel</th>
   <td>0,10 $/Go</td>
   <td>0,20 $/Go</td>
   <td>0,35 $/Go</td>
   <td>0,58 $/Go</td>
  </tr>
  <tr>
   <th>Prix horaire</th>
   <td>0,0002 $/Go</td>
   <td>0,0003 $/Go</td>
   <td>0,0005 $/Go</td>
   <td>0,0009 $/Go</td>
  </tr>
  </tbody>
</table>

 

## Mise à disposition

Les volumes {{site.data.keyword.filestorage_short}} peuvent être mis à disposition à partir de 20 Go et jusqu'à 12 To avec deux options de mise à disposition : <br/>
- Effectuez la mise à disposition avec des **Niveaux Endurance** offrant des niveaux de performance prédéfinis et des fonctionnalités telles que les instantanés et la réplication.
- Créez un environnement de **Performance** haute puissance avec des opérations d'entrée-sortie par seconde (IOPS) allouées.

 
### Niveaux Endurance

Lorsque vous sélectionnez Endurance, choisissez un des nombreux niveaux de performance pour la prise en charge des besoins de vos applications.

A cet effet, l'option Endurance est disponible avec trois niveaux de performance d'IOPS :

- L'option **0,25 IOPS par Go** est adaptée aux charges de travail peu exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.
- L'option **2 IOPS par Go** est adaptée à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données utilisées par des applications Web ou d'images de disques de machine virtuelle pour un hyperviseur.
- L'option **4 IOPS par Go** est adaptée aux charges de travail plus exigeantes en E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
- L'option **10 IOPS par Go** est adaptée aux charges de travail les plus intensives telles que celle créées par les bases de données NoSQL et le traitement de données pour Analytics.  Ce niveau est disponible pour le stockage mis à disposition jusqu'à 4 To dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html).

Un volume de type Endurance de 12 To comporte un maximum de 48 000 IOPS disponibles.


Il est essentiel de choisir le niveau d'endurance adapté de {{site.data.keyword.filestorage_short}} pour votre charge de travail, mais il est également important d'utiliser la taille de bloc, la vitesse de connexion Ethernet et le nombre d'hôtes appropriés afin d'atteindre des performances maximales. Si l'un de ces éléments n'est pas cohérent par rapport aux autres, l'impact sur le débit final peut s'avérer significatif.
 
### Performance

Performance est une classe d'{{site.data.keyword.filestorage_full}} conçue pour prendre en charge des applications avec un niveau élevé d'entrée/sortie et nécessitant un niveau de performance bien établi qui ne correspond pas à un niveau Endurance. Pour atteindre la performance prévue, il suffit d'allouer les IOPS au niveau du protocole à des volumes individuels. Des IOPS allant de 100 à 6 000 peuvent être mis à disposition avec des tailles de stockage de 20 Go à 12 To. 

La Performance {{site.data.keyword.filestorage_short}} est accessible et montée via une connexion NFS. {{site.data.keyword.filestorage_short}} est généralement utilisé lorsque le volume fait l'objet d'un accès simultané par plusieurs machines. Il est possible de commander des volumes avec des performances cohérentes en fonction des tailles et du nombre d'IOPS figurant dans le tableau 1 ; ces volumes peuvent être utilisés avec des systèmes d'exploitation Linux.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
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
            <td>6 000 ou 10 000<sup><img src="/images/numberone.png" alt="note de bas de page" /></sup></td>
          </tr>
          <tr>
            <td>1 000</td>
            <td>100</td>
            <td>6 000 ou 20 000<sup><img src="/images/numberone.png" alt="note de bas de page" /></sup></td>
          </tr>
          <tr>
            <td>2 000-3 000</td>
            <td>200</td>
            <td>6 000 ou 40 000<sup><img src="/images/numberone.png" alt="note de bas de page" /></sup></td>
          </tr>
          <tr>
            <td>4 000-7 000</td>
            <td>300</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="note de bas de page" /></sup></td>
          </tr>
          <tr>
            <td>8 000-9 000</td>
            <td>500</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="note de bas de page" /></sup></td>
          </tr>
          <tr>
            <td>10 000-12 000</td>
            <td>1 000</td>
            <td>6 000 ou 48 000<sup><img src="/images/numberone.png" alt="note de bas de page" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![footnote](/images/numberone.png)</sup> Vous pouvez opter pour un nombre d'IOPS supérieur à 6 000 dans des centres de données sélectionnés.


Les volumes de performance sont conçus pour fonctionner d'une manière cohérente proche du niveau d'IOPS mis à disposition. La cohérence facilite le dimensionnement et la mise à l'échelle des environnements d'application avec un niveau de performance donné. De plus, compte tenu de la plage des tailles de volume et du nombre d'IOPS, il devient possible d'optimiser un environnement en créant un volume avec le rapport idéal prix/performance.

Le nombre d'IOPS, pour les options Endurance et Performance, est mesuré sur la base d'une taille de bloc de 16 ko avec un mélange de lecture/écriture 50/50. Si vous souhaitez atteindre le nombre maximal d'IOPS sur un volume, vous devez mettre en place les ressources réseau adéquates. Vous devez également tenir compte de l'utilisation du réseau privé en dehors du stockage, ainsi que des réglages côté hôte et spécifiques aux applications (pile IP, nombre de lignes de file d'attente, etc.). 

## Conseils pour la mise à disposition des IOPS pour {{site.data.keyword.filestorage_short}}

Le nombre d'IOPS pour les options Endurance et Performance est basé sur une taille de bloc de 16 ko avec une charge de travail aléatoire à 50 % de lecture/écriture 50/50. Un bloc d'environ 16 ko est l'équivalent d'une écriture sur le volume.

La taille de bloc utilisée par votre application a une incidence directe sur les performances du stockage.  Si la taille de bloc employée par votre application est inférieure à 16 ko, la limite des opérations d'entrée-sortie est atteinte avant la limite de débit.  A l'inverse, si la taille de bloc utilisée par votre application est supérieure à 16 ko, la limite de débit est atteinte avant la limite des opérations d'entrée-sortie.

La modification de la taille de bloc affecte les performances comme suit :

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Taille de bloc (ko)</th>
            <th>IOPS</th>
            <th>Débit (Mo/s)</th>
          </tr>
          <tr>
            <td>4 (standard pour Linux)</td>
            <td>1 000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (standard pour Oracle)</td>
            <td>1 000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1 000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (standard pour SQLServer)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

Il est essentiel de bien choisir le {{site.data.keyword.blockstorageshort}} adapté à votre charge de travail, mais il importe également d'éviter les goulots d'étranglement. La vitesse de votre connexion Ethernet doit être supérieure au débit maximal attendu de votre volume. En règle générale, vous ne devriez pas saturer votre connexion Ethernet au-delà de 70 % de la bande passante disponible. Par exemple, si vous disposez de 6 000 IOPS et que vous utilisez une taille de bloc de 16 ko, le volume est capable de fournir environ 94 Mo par seconde. Si vous disposez d'une connexion Ethernet de 1 Gbps vers votre numéro d'unité logique, vous rencontrerez un goulot d'étranglement lorsque vos serveurs tenteront d'utiliser le débit maximal disponible car 70 % de la limite théorique d'une connexion Ethernet de 1 Gbps (125 Mo par seconde) n'autorisent que 88 Mo par seconde.


Un autre facteur à prendre en compte est le nombre d'hôtes qui utilisent votre volume. Si un seul hôte accède au volume, il peut s'avérer difficile de réaliser le nombre d'IOPS maximal disponible, surtout avec des nombres d'IOPS extrêmes (10 000). Si votre charge de travail requiert un débit élevé, il est préférable de configurer au moins deux ou trois serveurs pour accéder à votre volume afin d'éviter un goulot d'étranglement dû à un seul serveur.


Pour atteindre le nombre maximal d'IOPS, vous devez mettre en place les ressources réseau adéquates. Vous devez également tenir compte de l'utilisation du réseau privé en dehors du stockage, ainsi que des réglages côté hôte et spécifiques aux applications (pile IP, nombre de lignes de file d'attente, etc.).

NFS version 3 et NFS version 4.1 sont pris en charge dans l'environnement {{site.data.keyword.BluSoftlayer_full}}. Il est toutefois recommandé d'utiliser NFS version 3. NFS version 4.1 est un protocole avec état (et non sans état comme NFS version 3) et donc susceptible de générer des anomalies lors des événements de réseau. NFS version 4.1 doit mettre au repos les opérations puis effectuer la réclamation de verrou. Sur un serveur de fichiers relativement occupé, l'augmentation du temps d'attente peut entraîner une interruption. L'absence de fonctions de multi-accès et d'établissement de liaison dans NFS version 4.1 peut également allonger la reprise des opérations NFS.
