# Intégration et déploiement avec VSTS et Azure

## Objectif

![](/assets/DevOps.PNG)Source de l'image : [https://azure.microsoft.com/en-us/solutions/architecture/vsts-continuous-integration-and-continuous-deployment-for-azure-web-apps/](https://azure.microsoft.com/en-us/solutions/architecture/vsts-continuous-integration-and-continuous-deployment-for-azure-web-apps/)

Explication de l'objectif : [https://www.visualstudio.com/team-services/continuous-integration/?rr=https%3A%2F%2Fwww.google.fr%2F](https://www.visualstudio.com/team-services/continuous-integration/?rr=https%3A%2F%2Fwww.google.fr%2F)

Déployer sur Azure Web App :

1. [https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-webapps?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-webapps?view=vsts) : Pour le déploiement sur Azure Web App. On se basera d'abord sur cette source.
2. [https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1?view=vsts) Pour le déploiement d'un Hello World !. On pourra utiliser cette source pour la mise en pratique \(ou cliquer concrètement sur VSTS ..\)

## Les principes de l'intégration et du déploiement

Nous allons dans un premier temps mettre en place l'intégration continue afin d'avoir un build Continuous Integration qui publie un package Web Deploy pour notre application .NETCore.

Source : [https://docs.microsoft.com/en-us/vsts/build-release/apps/aspnet/build-aspnet-core?view=vsts&tabs=gitvsts%2Cweb%2Cdeploy-windows](https://docs.microsoft.com/en-us/vsts/build-release/apps/aspnet/build-aspnet-core?view=vsts&tabs=gitvsts%2Cweb%2Cdeploy-windows)

Nous utiliserons VSTS comme service Git.

### Configuration de l'Agent de Build

Source : [https://docs.microsoft.com/en-us/vsts/build-release/actions/agents/v2-windows?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/actions/agents/v2-windows?view=vsts)

Afin de mettre en place le build d'intégration, il nous faut un Agent de build.

Nous allons donc mettre en place un agent de build.

#### Qu'est-ce qu'un Agent \(de Build, de Release\)

Source : [https://docs.microsoft.com/en-us/vsts/build-release/concepts/agents/agents?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/concepts/agents/agents?view=vsts)

Pour build le code ou déployer le logiciel, il est nécessaire d'avoir au moins un Agent. Lorsque le projet augmente en volume, il est possible d'en avoir besoin de plus.

Lors du build ou du déploiement, le système démarre un ou plus **jobs**.** **

_**Un agent est un logiciel installable qui exécute un job de build ou de déploiement à la fois.**_

##### Agent hébergé \(Hosted Agent\)

Source : [https://docs.microsoft.com/en-us/vsts/build-release/concepts/agents/hosted?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/concepts/agents/hosted?view=vsts)

En travaillant avec VSTS, une option confortable pour build et deploy est d'utiliser un Hosted Agent.

En utilisant un Hosted Agent, Windows prend en charge sa maintenance et ses mises à jour. Pour beaucoup d'équipes, c'est la façon la plus simple d'assurer le Build et le Deploy.

Une bonne idée est d'essayer cette solution dans un premier temps. Si cela ne fonctionne pas, il faudra faire son propre agent privé.

Pour sélectionner un Agent hébergé, cela se fait lors de l'édition de la définition de Build :

Ici même dans VSTS : [https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1?view=vsts\#create-a-build-definition](https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1?view=vsts#create-a-build-definition)

Lors de la sélection de Agent Queue, il est possible de sélectionner un des Hosted Agent suivant :

* Hosted VS2017 \(si l'équipe utilise VS2017\)
* Hosted Linux
* Hosted macOS Preview
* Hosted \(si l'équipe utilise Visual Studio 2013 ou 2015\)

Dans notre cas, nous sélectionnerons Hosted VS2017.

##### Agent privé

Dans le cas ou les Hosted Agent mis à disposition par Microsoft ne conviennent pas \(voir leurs limites dans la source du paragraphe Agent hébergé\), il est possible de mettre en place soi-même un agent privé pour run les jobs de déploiement et de build.

Cela permet de controller l'installation de software nécessaire pour les builds et deployments.

Il est possible d'installer un agent sur Windows, Linux, macOS, et même un Linux Docker container.

Après avoir installé son agent privé sur une machine, il est possible d'installer n'importe quelle software sur cette machine requis par nos jobs de build et de déploiement.

Microsoft recommande l'utilisation d'un Hosted Agent si notre code est sur VSTS.

##### Capabilities

Chaque agent a une liste de Capabilities \(capacitées\). Les Capabilites sont des paires nom-valeur qui sont :

* découverte par le logiciel Agent : system capabilities \(capcitées system\)
* définies par l'utilisateur : user capabilities

Le logiciel Agent détermine automatiquement diverses system capabilities comme :

* Nom de la machine
* Type d'OS
* Version de certains logiciels installés sur la machine
* Les variables d'environnements définis dans la machine apparaissent automatiquement dans la liste de system capabilities

Lorsque l'on crée une build ou release definition, ou lorsque l'on queue une build ou déploiement, le système donne le job uniquement aux agents qui ont la** capabilitie **correspondante à la demande spécifié dans **la définition de build.**

**Definition de build** : [https://docs.microsoft.com/en-us/vsts/build-release/concepts/definitions/build/options?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/concepts/definitions/build/options?view=vsts)

De ce fait, les Capabilities permettent de diriger les builds et les déploiements vers un agent spécifique.

On peut consulter les System Capabilities et modifier les User capabilities en navigant sur le hub Agent pools.

Pour VSTS : [https://{your\_account}.visualstudio.com/\_admin/\_AgentPool](https://{your_account}.visualstudio.com/_admin/_AgentPool)

##### Communication

// TODO : Revenir à la doc et continuer ce chapitre sur les principes de l'intégration et du déploiement.

## Mise en place

VSTS : [https://geoffreyknobloch.visualstudio.com/\_projects](https://geoffreyknobloch.visualstudio.com/_projects)

Choix d'un projet existant : [https://geoffreyknobloch.visualstudio.com/TripTrackerLive](https://geoffreyknobloch.visualstudio.com/TripTrackerLive)

### Créer un projet sur VSTS

![](/assets/NewProject.PNG)Cliquer sur New Project puis renseigner son nom, le contrôle de version et la méthodologie.

Ici nous choisisons Git \(et non TFS\), en méthodologie Agile.

![](/assets/NewProjectDetails.PNG)

Cliquer sur Create.

On arrive alors sur la page de démarrage du nouveau projet.

![](/assets/ProjectCreated.PNG)Cette page a son importance car c'est ici que l'on va définir le serveur Git utilisé, si l'on se base sur un projet existant, et faire la liaison avec l'IDE Visual Studio.

Ici nous allons choisir comme serveur Git : VSTS \(et non GitHub, conseillé pour un projet public\).

Et nous allons simplement lier ce contrôle de code source à Visual Studio 2017 puis créer un nouveau projet.

Pour cela, cliquons sur Clone in Visual Studio.

Cela a pour effet d'ouvrir automatiquement \(!\) Visual Studio 2017 avec la fenêtre Team Explorer ouverte, et le choix du chemin local pour travailler sur le projet.![](/assets/VisualStudioProject.PNG)Une fois le choix du chemin local fait, cliquer sur Cloner.

Nous avons donc cloné en local la solution hébergée sur VSTS.

Sur VSTS, il n'y a pour le moment rien car nous venons de créer un projet vide.

Nous allons donc Créer une solution en local que nous irons comit en suite. C'est d'ailleurs ce que suggère la fenêtre Team Explorer.

![](/assets/NewSolution.PNG)

Cliquer sur "Créez un projet ou une solution".![](/assets/NewSolutionHelloWorld.PNG)Nous allons ici choisir une application ASP .NETCore.



