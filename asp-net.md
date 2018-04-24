# ASP .NET

Beaucoup de connaissances exposées sur la partie ASP.NET Core 2.x peuvent être réutilisées dans le cadre d’un projet ASP.NET sur .NET Framework.

Globalement, un skill acquis en .NET Core se retrouvera en .NET. Cependant, il y a des différences lorsque l’on travaille sur une application ASP .NET MVC ou sur une application ASP.NET Core 2.x MVC, ou ASP .NET Core 2.x Razor Pages.

Dans cette section, l’objectif est d’identifier les différences avec .NET Core afin d’être capable de switcher facilement d’un type de projet à l’autre.

Le but est de maîtriser une architecture en API \(donc créer des APIs en ASP.NET\), gérer de la notification en temps réel \(SignalR\), et construire une application cliente MVC.

Cette section a été créée pour répondre notamment aux questions suivantes :

Q : ASP .NET Core a un IoC Container par défaut. Qu’en est-t-il d’une application ASP.NET MVC ? Faut-t-il choisir un IoC ? Si oui, quel est le marché aujourd’hui ?

R : &lt;Unknown&gt;

Q : On a vu l’utilisation des middleware pour travailler requêtes et réponses en ASP.NET Core en ajoutant des Use, Run, Map. Qu’en est-t-il en ASP.NET MVC ?

R : &lt;à confirmer.&gt; A priori, une application ASP.NET MVC s’appuie aussi sur une classe Startup qui run une méthode Configure, on peut donc s’attendre à la même utilisation des Middleware pour le traitement des requêtes et des réponses.

Q : \(Transverse ASP.NET , ASP.NET Core \(comme beaucoup de notions\)\) Comment gérer la sécurité de son API pour que celle-ci soit consommée par le parc d’application de la société, mais pas ouverte à l’extérieur ? ou ouverte en read only à l’extérieur ?

R : &lt;Unknown&gt;, mais ASP.NET est plus mature et peut être que répondre à cette question dans le cadre de l’étude sur ASP.NET sera plus aisé. ASP.NET Core partage probablement énormément de similitude dans ce domaine.

## Overview ASP .NET

Source : [https://docs.microsoft.com/en-US/aspnet/overview](https://docs.microsoft.com/en-US/aspnet/overview): la doc officielle

ASP.NET offre 3 frameworks pour créer des applications web :

Web forms, ASP.NET MVC, et ASP.NET Web Pages.

Source :[https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET)

Scott Hanselman qui explique One ASP.NET \(combiner les technologies ASP\)

![](https://lh4.googleusercontent.com/YIpJYTHbArRqote2_XhnPWZdPXVWAdp4dRxXxEOQzTEOUeefxjMfu-vKZG3Fnkme1kFnSPCEjbXIh27d6CQnxzcWuXxKEALe-cUWqFiCnhIKI51pIG3apt5pk4b_AlN8qv-eJNYy)

### ASP.NET MVC :

* Contrôle absolu des markups HTML

* Supporte le test unitaire, TDD, Méthodologie Agile

* Extrêmement flexible et extensible.

### ASP.NET Web API

ASP.NET Web API est un framework qui facilite la création de services HTTP. ASP.NET Web API est une plateforme idéale pour construire des applications RESTful en .NET.

