# Requête Http

Bas niveau : [https://openclassrooms.com/courses/les-requetes-http](https://openclassrooms.com/courses/les-requetes-http)

Aujourd'hui n'importe quel language va utiliser un framework pour faciliter les requêtes POST / GET / DELETE / PUT / UPDATE \(vision API\) ...

\(OPTIONS / HEAD / CONNECT / TRACE existent également\)

Mais qu'est qui est réellement produit pour assurer la communication Client - Server ?

Syntaxe générale d'une requête :

```
Commande URL Version de protocole
En-tête de requête
<nouvelle ligne>
Corps de requête
```

Commande : POST / GET / ...

URL : Adresse de la page SUR LE SERVEUR \(Repertoire/Page\)

Requête POST :

```
POST /fichier.ext HTTP/1.1
Host: www.site.com
Connection: Close
Content-type: application/x-www-form-urlencoded
Content-Length: 33
<nouvelle ligne>
variable=valeur&variable2=valeur2
```



Outil PostMan : [https://www.getpostman.com/apps](https://www.getpostman.com/apps)



