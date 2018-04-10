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



