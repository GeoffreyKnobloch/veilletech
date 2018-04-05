# Framework .net

## Nouveautées apportées par les différentes versions

[https://docs.microsoft.com/fr-fr/dotnet/framework/whats-new/](https://docs.microsoft.com/fr-fr/dotnet/framework/whats-new/ "MSDN")

NEO : .net version 3.5

Dernière version à date avril 2018 : .net version 4.7.1

## Différence entre .net Core, .net Framework et Xamarin

Source : [https://stackoverflow.com/questions/38063837/whats-the-difference-between-net-core-net-framework-and-xamarin](https://stackoverflow.com/questions/38063837/whats-the-difference-between-net-core-net-framework-and-xamarin)

## ![](https://i.stack.imgur.com/sw8Ln.png)

* .net Framework : Pour Windows, version FULL, Traditionnel, pour application client Lourd, ou application UWP \(Universal Windows Platform\), ou pour travailler sur une application ASP .net
* .net Core : Cross platform : Windows, Mac, Linux : Pour application console ou application web hebergée sur n'importe quelle plateforme, incluant les containers Docker. Application UWP ou client lourd non supporté pour le moment.
* Xamarin : Pour travailler sur des applications mobiles. iOS, Android ou Windows Phone Devices. Xamarin se base sur MONO, qui est une version de .net construite pour le cross-platform avant que Microsoft décide de proposer la solution .net Core pour répondre à l'attente Cross Platform. Tout comme Xamarin, Unity se base sur MONO.

Attention à la confusion qui peut être faite parfois :

Une application ASP .net Core peut s'appuyer sur le framework .net \(Windows\) ou le framework .net Core \(cross-platform\).

Cette possibilité est donnée afin de développer aujourd'hui une aplication ASP .net Core, en utilisant des fonctionnalitées qui ne sont pas encore supportées par le framework .net Core, mais en se donnant la possibilité de passer full .net Core et donc bénéficier du cross-platform lors que cela sera implémenté.

## Application ASP .net Core Web s'appuyant sur .net Core ou .net Framework

* ASP .Net Core sur .Net Core : Cross-platform ASP .NET Core : Windows, Mac, Linux \(==&gt; Docker\), Le serveur n'a pas besoin d'avoir .net Core installé, les dépendances peuvent être installées avec l'application.
* ASP .Net Core sur .Net Framework : Application qui se base sur la version Full / Bureau du framework .net. \(4.7.1\) Ces applications ne peuvent tourner que sur Windows.

En terme de performance, une application ASP .net Core se basant sur .net Core, installé sur une machine Windows semble le choix optimal pour l'application web de demain :

```
ASP.NET 4.6: <50k req/sec

ASP.NET Core (CLR): 400k req/sec

ASP.NET Core (.NET Core, Linux): 900k req/sec

ASP.NET Core (.NET Core, Windows): >1.1m req/sec
```

### Plan d'exécution 

.net Framework :

![](https://i.stack.imgur.com/hG22P.png)

.net Core :

![](https://i.stack.imgur.com/nWPth.png)

## Pourquoi choisir .net Core

* Cross-Platform
* Microservices : Si une fonctionnalité n'est pas supporté par .net Core, il est possible de développer un microservice basé sur le framework .net qui me fournit la fonctionnalité du .net framework.
* Solution la plus performante et scalable

Source : [http://www.codechannels.com/article/microsoft/when-should-i-still-use-net-framework-4-x-instead-of-net-core/](http://www.codechannels.com/article/microsoft/when-should-i-still-use-net-framework-4-x-instead-of-net-core/)

TL;DR : "In summary, do not use the .NET Framework if what you want to do can be perfectly done with .NET Core, like many front-end web sites or microservices."

Ce que je veux faire peut être fait parfaitement avec .net Core ? \(Le cas de beaucoup de web site front end ou microservices\)

Oui ==&gt; Go le faire en ASP .net Core s'appuyant sur .net Core.

Non ==&gt; En ASP .net Core s'appuyant sur .net Framework, ou ASP .net 4.x

