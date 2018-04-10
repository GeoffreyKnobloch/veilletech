# Architecture d'une application

## Architecture en APIs

Une architecture qui semble pertinente :

### Autant d'API que de thème métier

Exemple du retail :

![](/assets/screencapture-developers-kiabi-fr-2018-04-10-11_20_52.png)

### ![](/assets/screencapture-developers-kiabi-fr-2018-04-10-11_20_52.png)Pour chaque API, architecture DMC sur un seul projet

DataLayer :

Couche d'accès à la base de donnée



Model :

* Ensemble de BOs correspondants au métier de l'API qui correspondent aux tables de la bdd.
* Ensemble de DTOs correspondants chacun à un BO. \(n DTOs pour un BO\). \(un BO peut être un DTO\)

Controller :

* Prend en charge les requêtes GET POST ... contenant les URL.

View :

* Pas besoin de view : nothing to Json ou Json to better Json



