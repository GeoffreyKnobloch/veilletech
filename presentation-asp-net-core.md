# Présentation ASP .NET Core

Source :[https://docs.microsoft.com/en-US/aspnet/core/?view=aspnetcore-2.1](https://docs.microsoft.com/en-US/aspnet/core/?view=aspnetcore-2.1)

## Présentation du framework

ASP .NET Core est :

* Cross-platform

* Performance élevée

* Open-source

Permet de :

* Build des Web apps, Web services, apps IoT, back pour mobile.

* Run sur .NET Core ou .NET Framework \(possibilité de pointer vers .NET Framework si besoin\)

## Pourquoi l’utiliser

ASP .NET Core est un redisign de ASP .NET 4.x

\(pour rappel les applications qui tournent sur le framework .NET 4.x sont notamments les applications ASP .NET MVC 2.0, 3.0, 4.0, 5.0, et 5.2 \(current\)\).

Dans ce redesign, les choix d’architecturent ont pour conséquences un framework plus léger, performant et plus modulaire.

ASP .NET Core a les avantages suivants :

* Une même méthode pour build une UI Web ou une API Web

* Gestion de la dependency injection intégré \(avec un Container d’Inversion of Control fourni par défaut\).

* Possible d’host sur IIS, Nginx, Apache, Docker

* Léger, haute performance, Pipeline de de requête HTTP modulaire \(à définir\)

* Open source et focus sur la communauté

* Système de configuration Cloud-ready, basé sur l’environnement.

ASP .NET Core est livré comme un ensemble de packages NuGet. Ceci permet d’optimiser l’application en n’incluant que les dépendances nécessaires.

En fait, les applications ASP .NET Core 2.x qui ne targuettent que .NET Core ne requièrent qu’un seul package NuGet.

Bénéfices : Meilleur sécurité, performance améliorée

## Construire des API Web et des UI Web en utilisant ASP .NET Core MVC

ASP .NET Core MVC fournit un ensemble de fonctionnalitées pour construire des API Web et des Appli Web.

* Le pattern MVC nous permet de faire des Web API et des Web App testables.

* Razor Pages \(nouveau dans ASP.NET Core 2.0\) est un model de programmation basé sur les pages qui rend la construction d’UI Web plus simple et plus productive.

* Le language Razor Markup fournit une syntaxe productive pour les pages Razor et les Views MVC.

* Les Tag Helper permettent à du code côté Serveur de participer à la création et au rendu d’élements HTML dans un fichier Razor.

* Un support fourni pour multiplier les formats de data et accepter la négociation du contenu permet à une API de communiquer à la fois en Json et en XML et en Plain text..

* Le Model binding permet de mapper la data depuis des Requêtes HTTP vers des paramètres d’actions de controller \(détecter l’envoie d’un int dans /students/2 par exemple.\)

* Le Model validation permet la validation automatique côté client et serveur de la data.

## Développement coté client

ASP .NET Core s’intègre parfaitement avec des framework et librairies côté client comme :

Angular, React, Bootstrap.

Titre 2 : Target .NET Framework \(4.x\) avec une application ASP .NET Core

ASP .NET Core peut pointer sur .NET Core, ou sur .NET Framework !

Les applications ASP .NET Core qui pointent sur .NET Framework ne sont pas cross-platform.

Générallement, ASP .NET Core est fait de librairies .NET Standard. Les applications écrites avec .NET Standard 2.0 tournent partout ou .NET Standard 2.0 est supporté.

Il y a plusieurs avantages à pointer sur .NET Core, et ces avantages sont :

* Cross-platform

* Performance

* Versionning Side by Side \(à clarifier\)

* Nouvelels APIs

* Open source

Il y a un gros travail de la part de Microsoft fait pour réduire le gap entre le nombre d’API .NET Framework et le nombre d’API .NET Core.

