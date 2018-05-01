# Node.js

Source : [https://nodejs.org/en/](https://nodejs.org/en/)

Node.js est un environnement d'éxécution JavaScript construit sur le moteur JavaScript V8 de chrome.

Node.js utilise un modèle basé sur l'événementiel et des entrées/sorties non bloquantes. Il est donc léger et efficace.

L'écosystème de logiciels de Node.js est npm.

npm est le plus grand écosystème de bibliothèques open source au monde.

Pour obtenir npm, il faut installer Node.js.

## npm

Source : [https://www.w3schools.com/nodejs/nodejs\_npm.asp](https://www.w3schools.com/nodejs/nodejs_npm.asp)

npm est un package manager pour des packages Node.js.

Un package en Node.js contient tous les fichiers nécessaires pour un module.

Un module est une librairie JavaScript que l'on peut incluse dans son propre projet.

Sans être expert Node.js, dans une application .NET, utiliser npm pour inclure un module \(librairie JavaScript\) peut s'avérer très utile.

### Télécharger un package

pour télécharger le package upper-case :

`C:\Users\g>npm install upper-case`

Conséquence :

Création du dossier node\_modules \(si inexistant\), puis du dossier upper-case :

C:\Users\g\node\_modules\upper-case

qui contient :

LICENSE, package.json, README.md, upper-case.d.ts, upper-case.js

### Utiliser un package

```
var http = require('http');
var uc = require('upper-case');
http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write(uc("Hello World!"));
    res.end();
}).listen(8080);
```

La ligne var uc = require \('upper-case'\) inclut le module upper-case. C'est de cette manière qu'est inclut n'importe quel autre module.

## Node.js

Source : [https://www.w3schools.com/nodejs/default.asp](https://www.w3schools.com/nodejs/default.asp)

Node.js est un environnement serveur open source.

Node.js permet  de run du JavaScript côté sur le Serveur.

