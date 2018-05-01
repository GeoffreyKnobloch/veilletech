# SignalR

Source :[https://docs.microsoft.com/en-us/aspnet/core/signalr/introduction?view=aspnetcore-2.1](https://docs.microsoft.com/en-us/aspnet/core/signalr/introduction?view=aspnetcore-2.1)

## Introduction

### Qu’est-ce que c’est

SignalR est une library qui simplifie l’ajout de fonctionnalitée web en temps réel.

Ceci permet à du code côté serveur de push du content au client instantanément.

Bons candidats pour SignalR :

* Applications qui ont besoin d’update du serveur fréquemment comme les jeux vidéos, réseaux sociaux, du vote, des enchères, des cartes et GPS

* Dashboard, application de surveillance, de vente instantannée, d’alertes de voyages

* Applications collaboratives : Application de WhiteBoard, de team meeting, …

* Applications qui ont besoin de notifications : Réseaux sociaux, email, chat, jeux, alertes de voyages, et plein d’autres applications

SignalR fournit une API afin de créer des appels REMOTE PROCEDURE CALLS \(RPC\) Serveur vers client :

Les RPC appellent des fonctions JavaScript sur le client depuis du code .NET côté Serveur.

La library SignalR sur ASP .NET Core :

* Prend en charge la gestion des connexions

* Permet le broadcast de messages pour tous les clients connectés en simultanés. Par exemple, une chat room.

* Permet d’envoyer des messages à des clients spécifiques ou des groupes de clients.

* Est open-source sur GitHub : [https://github.com/aspnet/signalr](https://github.com/aspnet/signalr)

* Est Scalable

La connexion entre le client et le serveur est persistante, contrairement à une connexion HTTP.

### Transport

SignalR prend en charge plusieurs techniques pour construire des applications web en temps réel.

WebSocket est le transport optimal, mais d’autres techniques comme Server-sent Events, Long polling peuvent être utilisés si des transports plus optimisés ne peuvent pas être utilisés.

SignalR va automatiquement détecter et initialiser le transport approprié basé sur les protocoles supportés par le serveur et le client.

### Hubs et Endpoints

SignalR utilise Hubs et Endpoints pour communiquer entre clients et serveurs.

L’API Hubs couvre la plupart des scénarios.

Un hub est une pipeline haut niveau construite sur l’API de EndPoint qui permet au client et serveur d’appeler des méthodes chez l’un et l’autre.

SignalR prend en charge le dispatching entre les machines, permettant aux clients d’appeler des méthodes sur le serveur aussi facilement que localement, et vice-versa.

Hubs permet le passage de type fortement typé en paramètre des méthodes, ce qui permet le model binding. SignalR fournit 2 protocoles hub par défaut :

Un protocol de text basé sur du JSON, et un protocol en binaire, basé sur MessagePack.

[https://msgpack.org/](https://msgpack.org/)

\(MessagePack : Comme JSON mais ne plus petit\)

Hubs appelle du code côté client en envoyant des messages en utilisant le transport actif.

Le message contient le nom et les paramètres de la méthode côté client.

Les objets passés en paramètres sont désérializés en utilisant le protocole configuré.

Le client essaye de matcher le nom de la méthode avec une méthode dans le code côté client.

Quand il y a match, le client run la méthode en utilisant la data en paramètre, désérializée.

Endpoints fournit une API raw socket-like, permettant de lire et écrire depuis le client. Le développeur peut gérer du grooping, broadcasting, et d’autres fonctions.

L’API Hubs est construite en s’appuyant sur la couche Endpoints.

![](https://lh4.googleusercontent.com/DrZcgmtumeQh_FiqufI6tCOvocN-estwXM8tNjjXU_0l5bjOQcErToX0WAYeTSVCc5DFYiw0rXRjV1TpRg755qdrAb6rUHd4pMTQsw3qAdks0rKxMdypX2zNvOxEXt_BQXJ7uuBo)

## Démarrer avec SignalR sur ASP.NET Core

Source :[https://docs.microsoft.com/en-us/aspnet/core/signalr/get-started?view=aspnetcore-2.1&tabs=visual-studio](https://docs.microsoft.com/en-us/aspnet/core/signalr/get-started?view=aspnetcore-2.1&tabs=visual-studio)

En javascript : Capte le click du boutton, et appelle le hub, qui lui va appeler le javascript pour écrire le message à l'ensemble des users.

à l'utilisation ça donne :

![](/assets/SignalRSerious.PNG)

