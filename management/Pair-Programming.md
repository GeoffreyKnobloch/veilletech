# Pair programming

Source :
https://www.infoq.com/articles/introducing-pair-programming
http://alistair.cockburn.us/Costs+and+benefits+of+pair+programming/v/slim


## Difficultées à mettre en place au sein d’une équipe réticente

### Un gros changement

On est plus sur du Casque, musique, tête dans le code.
Tous les développeurs ne sont pas forcément des bons communiquants, qui cherchent à aller vers l’autre.
Avoir un dialogue permanent en codant peut être un frein.

### C’est intense

Une des choses qui rend le pair programming si efficace, c’est qu’en session de pair programming, les développeurs sont concentrés. Il est impossible d’aller voir Facebook, surfer sur internet, répondre à un mail qui vient d’apparaître, …

### C’est fatiguant

Ce niveau d’intensité est fatiguant. Il y a une limite à cette fatigue. D'après Kent Beck :
“Most programmers can’t pair more than five or six hours in a day.”

### Cela nécessite des soft skills que la plupart de l’équipe n’a pas totalement développé

L’être humain ne naît pas avec empathie et aptitude sociale élevée. Si jusqu’alors, l'interaction avec les autres au travail est très limitée, le développement de ces soft skills peut être limité.
Travailler toute la journée avec une personne qui n’a pas développé ces compétences, ou avec un senior impatient en tant que junior, … Ces cas de figures demandent beaucoup de patience et d’empathie, de la part des 2 personnes qui pair programment.

### Le changement c’est difficile

Toute annonce de potentiel changement est reçu avec peur. Les gens ont besoin de savoir ce que ce changement va impacter chez eux.

## Les bénéfices du Pair programing

### Code de meilleure qualité

Dans un environnement où chaque ligne de code fait l’objet d’une review dés qu’elle est écrite (par son pair), la qualité du code augmente considérablement car le reviewer est totalement dans le context du code et participe activement à sa formation.

Une étude de Alistair Cockburn et Laurie Williams montre que :
le code produit en pair présente moins de bug et un meilleur design que le code produit en solo.
Les programmes écrits en pair sont 20% plus petit que les programmes en solo, indiquant une solution plus élégante et maintenable.

### Plus gros “Bus count”

Bus count : Si un membre de l’équipe se fait dégommer par un bus (quitte l’équipe), combien d’entre nous peuvent le remplacer ?
Si le bus count est 1, il y a quelqu’un dans l’équipe sans qui on ne peut rien faire.

Le Pair programming peut amener l’équipe jusqu’à : bus count = #personne dans l’équipe.

Cela signifie que la connaissance est si bien partagée, que tout le monde est capable de remplacer n’importe qui, tout le temps, et pour quoi que ce soit.

### Meilleure cohésion d’équipe

En travaillant une bonne partie de la journée avec son pair, on le connaît mieux qu’avant. Mieux connaître son pair permet de le comprendre mieux. La cohésion d’équipe devrait s’améliorer.

### Meilleure satisfaction au travail

C’est un soulagement de release du code en pair. Lorsqu’il est release, la pression du à la responsabilité n’est plus sur une personne mais réparti sur deux personnes.
Ils peuvent s'entraider en cas de problème.

### Plus grande vélocité

Une notion à laquelle on ne pense pas forcément.
Est-t-on vraiment plus rapide en écrivant une ligne par 2 personnes, plutôt que 2 personnes écrivant chacun des lignes de leurs côtés ?
La réponse est oui, le pair programming améliore effectivement la vélocité car :
Les problèmes qui causent des blocages, ou des micro-blocages sont résolus plus rapidement à 2. On fait moins de pause pour rechercher de la syntax de code, ou des recherches plus complexes, qui se résolvent plus rapidement.
Moins de bug implique plus de temps passé à créer de nouvelles features, et moins de temps dépensé à fixer des bugs. 
L’étude de Williams et Cockburn montre :
Le temps (heure homme) passé à écrire du code en pair augmente de 15%.
Ce temps augmenté est compensé par la réduction de bug : -15% aussi.
Comme la réduction de bug prend bien plus de temps après que le code soit écrit, en global, le temps passé pour obtenir du code qui fonctionne est plus court lorsque l’on développe en pair.

### Code plus maintenable

Lorsque deux personnes codent ensemble, ils doivent se mettre d’accord sur une approche, sur la structure, et même jusqu’au nom des variables et des fonctions.
Cela augmente les chances que les choix faits ne soient pas des choix arbitraire qui ne conviendraient qu’à une personne.
Les pair programmers seront plus enclins pour adhérer à des standards pour la syntaxe et d’autres bonnes pratiques comme le TDD.

### Haut degré d’apprentissage et de développement personnel

Dans ce moment de partage de focus zone qu’est le pair programming, travailler avec une autre personne permet de voir sa façon de résoudre des problèmes, du shortcut de l’IDE à la façon de penser différente, on évolue en apprenant de l’autre.

### Plus de flexibilité pour les gens pour changer d’équipe

Cette capacité à transmettre la connaissance sur des technologies et des domaines de programmation permet aux personnes de changer d’équipe avec un impact réduit.
Cette flexibilité présente des bénéfices pour tout le monde.
Lorsqu’un projet a besoin de plus de personnes, le management peut facilement emprunter des ressources à d’autres projets et avoir des résultats rapidements.
De même, pour le développeur, cela permet d’étendre ses connaissances.

### Moins de réunion

Lorsque les membres d’une équipe passent la journée à se parler, l’information est transmise librement. Il est donc rare d’avoir besoin d’une réunion pour discuter de quoi que ce soit.
C’est un point important pour la plupart des développeurs qui préfèrent généralement avoir le travail fait plutôt que d’être en réunion pour en parler.

## Faire adopter le pair programming dans une équipe
