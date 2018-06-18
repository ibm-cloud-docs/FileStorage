---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Paramètres recommandés pour la profondeur de la file d'attente de l'hôte

Voici la profondeur de file d'attente maximale recommandée pour les entrées-sorties de l'hôte et de l'application pour chaque niveau de performance. Le paramètre défini pour l'hôte n'affecte pas le temps d'attente du disque et du contrôleur, mais uniquement le temps d'attente observé par l'hôte et l'application.

<table align="center">
  <caption>Profondeur de file d'attente recommandée pour chaque niveau IOPS</caption>
        <thead>
	    <tr>
		<th>Niveau de performance</th>
		<th>Profondeur maximale de file d'attente d'hôte</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">0,25 IOPS par Go</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">2 IOPS par Go</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">4 IOPS par Go</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>

Une profondeur de file d'attente définie sur un nombre supérieur aux nombres recommandés est susceptible d'augmenter le temps d'attente des entrées-sorties, alors qu'une valeur inférieure aux recommandations risque de réduire les performances d'E-S de l'hôte. Etant donné que chaque application est différente, vous devez effectuer des observations et procéder à des ajustements pour atteindre les performances maximales du stockage.

La profondeur de la file d'attente de l'hôte est généralement ajustée dans le pilote de l'adaptateur de bus de l'hôte ou dans l'hyperviseur, et parfois dans l'application. Les valeurs par défaut standard comme 32 ou 64 risquent de générer un temps d'attente excessif de la part de l'hôte ou de l'application.

Si un hôte ou un hyperviseur utilise plusieurs niveaux de performance, spécifiez la profondeur de la file d'attente pour le niveau le plus rapide et observez le temps d'attente sur le niveau aux performances les moins élevées. Si le temps d'attente du niveau le plus faible est inacceptable, ajustez la profondeur de la file d'attente jusqu'à ce que vous observiez un équilibre entre le temps d'attente et les performances à tous les niveaux.
