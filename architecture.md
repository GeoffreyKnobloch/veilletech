# Architecture d'une application

## Architecture en APIs

Une architecture qui semble pertinente :

### Autant d'API que de thème métier

Exemple du retail :

![](/assets/screencapture-developers-kiabi-fr-2018-04-10-11_20_52.png)

### ![](/assets/screencapture-developers-kiabi-fr-2018-04-10-11_20_52.png)Pour chaque API, architecture "MC" sur un seul projet

Model :

* Ensemble de BOs correspondants au métier de l'API qui correspondent aux tables de la bdd.
* Ensemble de DTOs correspondants chacun à un BO. \(n DTOs pour un BO\). \(un BO peut être un DTO\)
* 3 tiers : \(n\) DAO - \(1\) Service \(1\)  - \[Sur le controller \(1\) WebService\]
* DAO va appeler en base et fournir des Bos
* Service va appeler le DAO et me fournir des DTOs
* WebService exposé dans le controller va simplement appeler le service

Controller :

* Prend en charge les requêtes GET POST ... contenant les URL et appelle : Expose les webservice

View :

* Pas besoin de view : nothing to Json ou Json to better Json

## Architecture Micro Service

Source : [https://blog.octo.com/larchitecture-microservices-sans-la-hype-quest-ce-que-cest-a-quoi-ca-sert-est-ce-quil-men-faut/](https://blog.octo.com/larchitecture-microservices-sans-la-hype-quest-ce-que-cest-a-quoi-ca-sert-est-ce-quil-men-faut/)

### Vocabulaire

Service : Un service métier : Un groupe de services techniques \(REST, SOAP\) qui fournissent une fonctionnalité qui a un sens métier.

Projet : Un projet de développement informatique sur l'ensemble de son cycle de vie. \(Pas seulement sur sa phase Projet, avant son basculement en mode Maintenance\).

### Constat

Plus le projet est gros, plus il est difficile de le maintenir, de le faire évoluer sans effet de bord. Même si l'architecture logicielle est bonne, il apparaît avec le temps des rustines, les fonctionnalitées métier deviennent de plus en plus complexe.

### L'architecture microservices

Pour éviter le problème des gros projets, il suffit de n'avoir que des petits projets : une réponse simple et radicale.

On limite donc la taille des projets à quelques personnes pour n'avoir que des Pizza teams d'une taille maximale de 7 personnes tout compris.

Il ne s’agit pas de séparer les gros projets en sous-équipes mais bien de projets _indépendants _: chacun a son organisation, son calendrier, sa base de code et ses donnée.

Les échanges entre les projets se font par des services. \(REST / JSON\).![](https://blog.octo.com/wp-content/uploads/2015/10/microservices-1024x464.png)

On a beaucoup de petits projets.

