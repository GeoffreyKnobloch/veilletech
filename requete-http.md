# Requête Http

## Bas niveau

[https://openclassrooms.com/courses/les-requetes-http](https://openclassrooms.com/courses/les-requetes-http)

Aujourd'hui n'importe quel language va utiliser un framework pour faciliter les requêtes POST / GET / DELETE / PUT / UPDATE \(vision API\) ...

\(OPTIONS / HEAD / CONNECT / TRACE existent également\)

Mais qu'est qui est réellement produit pour assurer la communication Client - Server ?

### Requête Client

#### Syntaxe Générale

```
Commande URL Version de protocole (Appelé la Ligne de commande)
En-tête de requête
<nouvelle ligne>
Corps de requête
```

* Commande : POST / GET / ...
* URL : Adresse de la page SUR LE SERVEUR \(Repertoire/Page\)
* Version : HTTP/1.1 par exemple.
* En-tête : Suite de valeurs sous la forme  -&gt; Nom : Valeur
* nouvelle ligne &lt;==&gt; \r\n supplémentaire.
* Corps de requête : Le body, par exemple : variable=valeur&variable2=valeur2

#### Exemple de requête POST

ici body au format SOAP \(peut être JSON, ou autre !\)

```
POST /Quotation HTTP/1.0
Host: www.xyz.org
Content-Type: text/xml; charset = utf-8
Content-Length: nnn

<?xml version = "1.0"?>
<SOAP-ENV:Envelope
   xmlns:SOAP-ENV = "http://www.w3.org/2001/12/soap-envelope"
   SOAP-ENV:encodingStyle = "http://www.w3.org/2001/12/soap-encoding">

   <SOAP-ENV:Body xmlns:m = "http://www.xyz.org/quotations">
      <m:GetQuotation>
         <m:QuotationsName>MiscroSoft</m:QuotationsName>
      </m:GetQuotation>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Remarque intéressante : Il est possible de passer des variables GET via l'URL même dans le cas d'une requête POST.

### Réponde Serveur

#### Syntaxe générale

```
Version Code-réponse Texte-réponse (Appelé la ligne de statut)
En-tête de réponse
<nouvelle ligne>
Corps de réponse
```

* Version : Version HTTP du serveur
* Code-réponse : Code d'erreur retrourné \(200, 403, 404, 500, ...\)
* Texte-réponse : Texte associé à l'erreur \(OK, FORBIDDEN, NOT FOUND, INTERNAL ERROR, ...\)
* En-tête : Suite de valeur sous forme Nom : Valeur
* Nouvelle ligne : \r\n supplémentaire
* Corps de réponse : Le contenu du fichier réponse \(L'ensemble du HTML d'une page par exemple, ou un objet SOAP dans le cas d'une API\)

#### Exemple de réponse

Réponse pour naviguer sur la page d'un site :

```
HTTP/1.1 200 OK
Date: Thu, 11 Jan 2007 14:00:36 GMT
Server: Apache/2.0.54 (Debian GNU/Linux) DAV/2 SVN/1.1.4
Connection: close
Transfer-Encoding: chunked
Content-Type: text/html; charset=ISO-8859-1

178a1

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="fr">
<head>
<meta http-equiv="content-type" content="text/html; charset=ISO-8859-1" />
<meta http-equiv="pragma" content="no-cache" />

<title>Accueil - Le Site du Zéro</title>
...Tout l'html de la page demandée.
```

Réponse d'une API JSON : \(Source : [http://jsonapi.org/examples/ \)](http://jsonapi.org/examples/)

```
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json

{
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "JSON API paints my bikeshed!",
      "body": "The shortest article. Ever.",
      "created": "2015-05-22T14:56:29.000Z",
      "updated": "2015-05-22T14:56:28.000Z"
    },
    "relationships": {
      "author": {
        "data": {"id": "42", "type": "people"}
      }
    }
  }],
  "included": [
    {
      "type": "people",
      "id": "42",
      "attributes": {
        "name": "John",
        "age": 80,
        "gender": "male"
      }
    }
  ]
}
```

Réponse d'une API SOAP \(Source : [https://www.tutorialspoint.com/soap/soap\_examples.htm](https://www.tutorialspoint.com/soap/soap_examples.htm) \)

```
HTTP/1.0 200 OK
Content-Type: text/xml; charset = utf-8
Content-Length: nnn

<?xml version = "1.0"?>
<SOAP-ENV:Envelope
   xmlns:SOAP-ENV = "http://www.w3.org/2001/12/soap-envelope"
   SOAP-ENV:encodingStyle = "http://www.w3.org/2001/12/soap-encoding">

   <SOAP-ENV:Body xmlns:m = "http://www.xyz.org/quotation">
      <m:GetQuotationResponse>
         <m:Quotation>Here is the quotation</m:Quotation>
      </m:GetQuotationResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## Outil PostMan

[https://www.getpostman.com/apps](https://www.getpostman.com/apps)

// TODO : Expérimenter Postman

## Utilisation CRUD

* create - POST
* read - GET
* update - PUT
* delete - DELETE

## Creuser le sujet des API : La solution réactive RX

[https://msdn.microsoft.com/en-us/library/hh242974\(v=vs.103\).aspx](https://msdn.microsoft.com/en-us/library/hh242974%28v=vs.103%29.aspx)

[https://github.com/Reactive-Extensions/Rx.NET](https://github.com/Reactive-Extensions/Rx.NET)

// TODO : Explorer



