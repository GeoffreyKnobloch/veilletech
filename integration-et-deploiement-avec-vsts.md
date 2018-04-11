# Intégration et déploiement avec VSTS et Azure

## Objectif

![](/assets/DevOps.PNG)Source de l'image : [https://azure.microsoft.com/en-us/solutions/architecture/vsts-continuous-integration-and-continuous-deployment-for-azure-web-apps/](https://azure.microsoft.com/en-us/solutions/architecture/vsts-continuous-integration-and-continuous-deployment-for-azure-web-apps/)

Explication de l'objectif : [https://www.visualstudio.com/team-services/continuous-integration/?rr=https%3A%2F%2Fwww.google.fr%2F](https://www.visualstudio.com/team-services/continuous-integration/?rr=https%3A%2F%2Fwww.google.fr%2F)

Déployer sur Azure Web App :

[https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-webapps?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-webapps?view=vsts)

## Mise en place d'un build d'intégration continue

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

**Un agent est un logiciel installable qui exécute un job de build ou de déploiement à la fois.**



