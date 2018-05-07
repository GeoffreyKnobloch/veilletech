# MSBuild

Quelques notes sur la MSBuild ...

07/05/18

## AppCenter

Outil de CI / CD pour développement mobile qui semble tout  aussi simple d'utilisation que VSTS mais pour déployer des applications mobiles \(sur Azure\) et en production sur l'App Store

## Projet DevOps directement depuis Azure

On fait d'abord le model DevOps, et en suite on choisit le Repo correspondant ^^

## MicroServices qui runnent en continue, et pour débuguer, je ne lance que le micro service suspect en debug, dans mon "Espace"

Pour déterminer le MicroService suspect, on a utilisé Azure pour déterminer les logs ...

## Loosely Serverless Architecture

Grace à un hébergement Azure et des microServices indépendants

Serverless code, serverless workflow

## Event grid

Azure Event Grid pour de l'Event Delivery \(Commit, Build, classiquement\)

Setup des Event dans l'Azure portal \(plutôt que VSTS ?\)

ou quand une vidéo est upload, etc ...

## Azure IoT Edge

Ce qui est intéressant, c'est qu'on peut, au lieu de consommer une API, consommer des fonctionnalitées  sur Azure, depuis notre application.

## Azure Cosmos DB

Solution pour appli planétaire

Cosmos DB account on choisit ou on veut mettre des DB dans le monde,

chacun peut R/W et c'est synchronisé \(répliqué\) en temps réel



