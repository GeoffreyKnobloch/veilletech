# Intégration et déploiement avec VSTS et Azure

## Objectif

![](/assets/DevOps.PNG)

## Mise en place

VSTS : [https://geoffreyknobloch.visualstudio.com/\_projects](https://geoffreyknobloch.visualstudio.com/_projects)

### Créer un projet sur VSTS

![](/assets/NewProject.PNG)Cliquer sur New Project puis renseigner son nom, le contrôle de version et la méthodologie.

Ici nous choisisons Git \(et non TFS\), en méthodologie Agile.

![](/assets/NewProjectDetails.PNG)

Cliquez sur Create.

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

Cliquez sur "Créez un projet ou une solution".![](/assets/NewSolutionHelloWorld.PNG)Nous allons ici choisir une application ASP .NET Core Razor Pages avec authentification par compte d'utilisateur individuels.![](/assets/CreationSolution.PNG)Dans l'explorateur de solution, vous pouvez constater la solution créée, avec tout ce que fournit Microsoft afin de faciliter la création d'une application de ce type. Si vous n'êtes pas familiés avec .net Core, Compilez et constatez que par défaut, votre application .net Core Razor page a ce contenu :![](/assets/StartApp.PNG)On retrouve l'ensemble des fichiers en Ajout dans la fenêtre Team Explorer, on va les mettre en ligne sur le serveur Git sur la branche Master pour notre premier Commit.

![](/assets/FirstComit.PNG)

Cliquez sur Valider tout puis sur Synchroniser

![](/assets/PousserComit.PNG)

Puis sur "Pousser".

![](/assets/PullRequest.PNG)

Puis, comme suggéré, cliquez sur "Créez une demande de tirage \(Pull request\) afin de faire une Pull request.

Nous revenons alors sur VSTS. Ici vous pouvez constater dans l'onglet "Files" que le code de la solution Hello World est bien sur la branche Master.

Afin d'ordonner les développements en utilisant un service GIT comme contrôle de code source, nous allons créer une autre branche pour ne pas archiver nos modifications directement sur la branche Master, mais plutôt les archiver sur une branche de Dev, que nous pousserons sur Master pour chaque versions stables.

Pour cela, une des manières de faire est de naviguer sur l'onglet Branches,![](/assets/Branches.PNG)

puis de cliquer sur New Branch, puis créer une branche nommée dev, basée sur master.

Si celle-ci n'apparaît pas dans Visual Studio, créez là en là nommant pareillement : dev, basée sur master en passant par Team Explorer / branches / Nouvelle branche

Pour tester la branche, nous allons faire une modification. En se plaçant sur la branche dev dans Visual Studio, modifions Index.cshtml afin de changer une ligne :  ViewData\["Title"\] devient "Hello World" et non "Home page".

En comparant Dev et Master, on a bien la différence que l'on vient d'archiver :

![](/assets/DevToMaster.PNG)

Maintenant que nous avons un projet stable, ainsi que la maîtrise du contrôle de code source, nous pouvons mettre en place l'intégration et le déploiement continu.

### Intégration continue

Dans l'onglet Build And Release :

![](/assets/Build.PNG)

Cliquez sur New definition.

![](/assets/Source.PNG)

Pour correspondre au projet que nous venons de créer, on choisit d'utiliser VSTS Git, et on sélectionne notre Repository et pour la branche par défaut, on choisit master.

Nous pouvons alors choisir un Template afin d'avoir une définition de build correspondant à notre projet par défaut, que l'on peut customizer par la suite.

![](/assets/Template.PNG)

Pour notre application .NET Core, nous nous baserons sur le template ASP.NET Core.

![](/assets/BuildDefinition.PNG)

Par défaut, est proposé une build definition composée d'une Agent Phase, nommé Phase 1, qui regroupe 4 tasks : Restore, Build, Test, Publish, qui vont run sur le même agent.

Il est possible d'utiliser un agent privé installé sur une machine et qui sera capable d'éxécuter l'ensemble des tasks de l'Agent Phase.

Nous allons utiliser un Agent hosté : Microsoft se charge d'host un agent pour nous, sur une machine distante, et cet agent va exécuter les Tasks de l'Agent Phase.

Après avoir personnalisé les Tasks, on peut Save Build Definition.

Pour assurer l'intégration continue, se rendre dans l'onglet Triggers et cocher Enable continuous integration, et choisir les branches sur lesquels on souhaite mettre en place l'intégration continue.![](/assets/ContinuousIntegration.PNG)On va également mettre en place l'intégration journalière à 3 heures du matin tous les jours ouvrés :![](/assets/Scheduled integration.PNG)Puis sauvegardez.

L'intégration continue est désormai assurée : à chaque nouveau commit, un build sera effectué.

Pour le tester, faites une modification et archivez dans la branche dev.

Vous pouvez alors constater dans l'onglet des builds en cours qu'il y a un build qui vient d'être trigger !![](/assets/Continuous integration test.PNG)

Double cliquez sur le build in progress pour constater les opérations qui sont effectuées en direct. C'est également l'occasion de tester que l'application build bien avec un résultat : Build succeeded.

### Déploiement continu

L'intégration continue est mise en place, il ne reste plus qu'à implémenter le déploiement sur Azure.

Pour cela, il nous faut un compte Azure. Il est possible d'en faire un gratuitement. Vous allez devoir renseigner votre carte bancaire, mais Microsoft précise bien qu'aucun prélévement ne sera fait sans qu'il ne soit explicitement indiqué.

Je conseille de faire un compte Microsoft Azure en utilisant la même adresse que le compte Microsoft que vous utilisez sur VSTS.

On se rend dans l'onglet Build and Release / Releases :

![](/assets/Release.PNG)

Nous allons créer une nouvelle définition de release en cliquant sur New definition.

A nouveau, VSTS nous propose de choisir un Template que l'on pourra personaliser, ce afin de gagner du temps, plutôt que de démarrer avec un processus de déploiement vide :

![](/assets/TemplateRelease.PNG)

Nous allons choisir Azure App Service Deployment.

Une fois créé, il faut renseigner quelques paramètres liés à Azure :![](/assets/AzureAppSetting.PNG)L'abonnement Azure apparaîtra "tout seul" dans un menu déroulant si vous utilisez le même compte Microsoft pour VSTS et pour Azure.

Pour avoir un App Service Name disponible, il faut aller sur Azure et créer une Application Service :![](/assets/Azure.PNG)

Cliquez sur App Services à gauche, puis +Add puis sélectionnez Web App puis Create.![](/assets/Web app.PNG)Il ne vous reste plus qu'à choisir un nom de domaine qui n'est pas encore pris :![](/assets/ChoixDomaine.PNG)Ici on a choisit bonjourmonde.

Cela met environ 3 minutes pour créer l'application Service. Allez vous chercher un café, à votre retour, l'App Service sera disponible pour la Release Definition sur VSTS :![](/assets/VSTSAppservice.PNG)En allant sur le website correspondant : [https://bonjourmonde.azurewebsites.net/](https://bonjourmonde.azurewebsites.net/)

On a la confirmation que lll'application d'App Service est en train de tourner :![](/assets/AppService Up And Running.PNG)Une fois la définition de livraison terminée sous VSTS, sauvegardez là.

Plutôt que déployer à la main, nous allons mettre en place la livraison continue. Pour cela, nous allons dans l'onglet Pipeline.

![](/assets/Pipeline.PNG)

On commence par ajouter un artifact : on va se baser sur le build que l'on a créé précédemment pour l'intégration continue !

![](/assets/Artifact.PNG)

Nous avions fait le choix que le build se déclenche à chaque archivage sur dev et sur master.

Pour la livraison continue, nous souhaitons livrer que lorsqu'un archivage sur master est effectué afin de ne livrer que des versions stables.

Pour que la livraison soit continue, on joue sur les trigger afin d'enable que la livraison soit automatique.

En modifiant les triggers, les users approvals, on arrive par exemple à cette pipeline :![](/assets/Chaine.PNG)On arrive au résultat suivant :

Lorsqu'un build est déclenché à l'issu d'un archivage sur master, le déploiement dans l'environnement de test est automatique, et le déploiement dans l'environnement de production est demandé, mais soumis à l'approbation du collab Geoffrey Knobloch.

Il reste un petit problème : L'environnement de Test et l'environnement de Prod pointent pour l'instant tous les deux sur la même AppService.

Il existe probablement des solutions plus adaptées pour le déploiement en environnement de test \(Docker, Gates ..\), mais ce que nous allons faire pour le moment, c'est créer une deuxième AppService Application sur Azure afin qu'elle héberge notre environnement de Test pour nos testeurs.

C'est chose faite :

Désormai l'environnement de test déploie automatiquement à cette URL :

[https://bonjourmondetestenv.azurewebsites.net/](https://bonjourmondetestenv.azurewebsites.net/)

Pour tester le tout, on va reporter les changements faits sur dev sur la branche master, si l'intégration et la livraison continue fonctionne, cela devrait mettre à jour [https://bonjourmondetestenv.azurewebsites.net/](https://bonjourmondetestenv.azurewebsites.net/) automatiquement !

Tout devient automatique : Build, déploiement, etc juste après le merge sur master !

![](/assets/automatique.PNG)

L'url de test est déployée automatiquement ! Et un mail est reçu pour confirmer le déploiement en prod !

**Le tutoriel de mise en place est terminé !**



**Pour aller plus loin, ci-dessous vous trouverez une exploration de la documentation fournie par Microsoft pour essayer d'en savoir plus sur la théorie qui se cache derrière la pratique que nous venons de réaliser avec succés ! En principe, vous avez déjà une idée solide de ce qu'est un agent de build, ... Sinon, une lecture du prochain chapitre peut aider à éclaircir les choses.**

## Les principes de l'intégration et du déploiement

Source de l'image objectif : [https://azure.microsoft.com/en-us/solutions/architecture/vsts-continuous-integration-and-continuous-deployment-for-azure-web-apps/](https://azure.microsoft.com/en-us/solutions/architecture/vsts-continuous-integration-and-continuous-deployment-for-azure-web-apps/)

Explication de l'objectif : [https://www.visualstudio.com/team-services/continuous-integration/?rr=https%3A%2F%2Fwww.google.fr%2F](https://www.visualstudio.com/team-services/continuous-integration/?rr=https%3A%2F%2Fwww.google.fr%2F)

Déployer sur Azure Web App :

1. [https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-webapps?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-webapps?view=vsts)
2. [https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/actions/ci-cd-part-1?view=vsts) 

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

