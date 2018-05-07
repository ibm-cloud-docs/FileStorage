---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Initiation à {{site.data.keyword.filestorage_short}}

La section de brève description doit inclure une ou deux phrases expliquant les raisons pour lesquelles un administrateur système ou un ingénieur en développement serait amené à utiliser ce service ou cette offre d'infrastructure.
Mentionnez brièvement l'objectif d'apprentissage de l'utilisateur et incluez les mots clés d'optimisation pour les moteurs de recherche suivants dans le titre et/ou la brève description : IBM Cloud, ServiceName. Veillez à adopter un style conversationnel. Pour plus de détails, reportez-vous aux conseils relatifs au style conversationnel dans Carbon Design System à l'adresse http://www.carbondesignsystem.com/guidelines/content/general.

Exemple :
En cas d'un accroissement des demandes de ressources, vous avez besoin de services d'infrastructure de cloud qui peuvent s'adapter immédiatement à ces nouvelles exigences, sous votre contrôle. Les serveurs virtuels {{site.data.keyword.Bluemix}} peuvent être déployés en quelques minutes à partir des images de serveur virtuel de votre choix, dans la région géographique pertinente en fonction de vos charges de travail. Dès que vos charges de travail diminuent, il est possible d'interrompre ou de mettre hors tension ces serveurs virtuels pour que votre environnement de cloud s'adapte parfaitement à vos besoins en matière d'infrastructure.

La section des tâches inclut les étapes indiquant comment rendre une unité, un stockage ou un réseau opérationnel.
- Indiquez des informations techniques liées aux tâches à effectuer en rédigeant des instructions directes et succinctes au détriment du style conversationnel.
- INDIQUEZ les étapes du scénario de base le plus courant permettant d'utiliser le service d'infrastructure.
- N'INCLUEZ PAS les étapes permettant d'ajouter le service à partir du catalogue Bluemix ; nous supposons que l'utilisateur a déjà effectué ces étapes dans l'interface utilisateur pour ajouter le service.
- INDIQUEZ les fragments de code dans tous les langages susceptibles d'être copiés, ainsi que les informations du service VCAP. Pour plus d'informations sur les informations du service VCAP, voir https://console.ng.bluemix.net/docs/cli/vcapsvc.html.
- Pour les tâches supplémentaires comme la configuration, la gestion, etc., ajoutez une section de tâche (## Gerund_task_title) sous la section des tâches ou sous la section "A propos de" le cas échéant. Utilisez un titre de tâche similaire à "Configuration de x", "Administration de y", "Gestion de z". -->

## Prérequis
Pour qu'un administrateur puisse mettre à disposition ou gérer ses comptes d'infrastructure, il doit disposer d'un compte {{site.data.keyword.Bluemix}} mis à niveau. Pour plus d'informations, voir [Mise à niveau et unification des comptes de facturation {{site.data.keyword.Bluemix_notm}} et SoftLayer](../docs/admin/softlayerlink.html).

## Titre et description orientés tâche
Pour commencer rapidement à utiliser ce service d'infrastructure, suivez les étapes ci-après OU Effectuez les étapes suivantes pour commencer à utiliser le service Block Storage :

<!-- Use ordered list markup for the step section. For code examples:
- use three backticks ahead of and after the example (```)
- For copyable code snippet, multi-line, include {: codeblock} following the last set of backticks. A copy button will display in framework in output.
- For copyable command, single line, include {: pre} following the last set of backticks. When displayed, it will show "$" at the beginning of the command example and a copy button, but the copy button will include just the command example.
- For non-copyable output snippet, include {: screen} following the last set of backticks.
 -->

1. Etape 1 pour configurer le service.
2. Etape 2 pour configurer le service.

	```
	Copyable example for this step.
	This example might be multiline code
	to copy into a file.
	When displayed in the doc framework,
	it will have a copy button on the right.
	The user can click to copy the example
	so they can paste it into their code editor.
	```
	{: codeblock}

3. Etape 3. Exemple de commande simple sur une ligne. Lorsqu'il est affiché dans l'infrastructure de la documentation, le signe $ apparaît au début de la ligne et un bouton de copie s'affiche sur la droite. Le bouton de copie copie la commande mais pas le signe $.

	```
	my command -and -options
	```
	{: pre}

4. Etape 4
	```
	This is a bunch of output from
		a command or program I ran
			and it can run lots of lines
			and the doc framework will show it as
			output with no copy button.
	```
	{: screen}

## Etapes suivantes

