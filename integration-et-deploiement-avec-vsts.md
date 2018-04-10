# Intégration et déploiement avec VSTS et Azure

## Objectif

![](/assets/DevOps.PNG)Source de l'image : [https://azure.microsoft.com/en-us/solutions/architecture/vsts-continuous-integration-and-continuous-deployment-for-azure-web-apps/](https://azure.microsoft.com/en-us/solutions/architecture/vsts-continuous-integration-and-continuous-deployment-for-azure-web-apps/)

Explication de l'objectif : [https://www.visualstudio.com/team-services/continuous-integration/?rr=https%3A%2F%2Fwww.google.fr%2F](https://www.visualstudio.com/team-services/continuous-integration/?rr=https%3A%2F%2Fwww.google.fr%2F)

Déployer sur Azure Web App :

[https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-webapps?view=vsts](https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-webapps?view=vsts)

## Mise en place d'un build CI

Nous allons dans un premier temps mettre en place l'intégration continue afin d'avoir un build Continuous Integration qui publie un package Web Deploy pour notre application .NETCore.

Pour mettre en place l'intégration continue, nous nous appuierons sur cette source :

[https://docs.microsoft.com/en-us/vsts/build-release/apps/aspnet/build-aspnet-core?view=vsts&tabs=gitvsts%2Cweb%2Cdeploy-windows](https://docs.microsoft.com/en-us/vsts/build-release/apps/aspnet/build-aspnet-core?view=vsts&tabs=gitvsts%2Cweb%2Cdeploy-windows)

Nous utiliserons VSTS comme service Git.

// TODO : Continuer ça. Sachant que je pense que mon projet TrickTracker fera tout à fait l'affaire.

