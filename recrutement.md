# Entretien technique

Document permettant de préparer le passage d'entretien techniques, selon le profil.

## Questions général pour candidat ASP .Net

* Comment utiliser les controllers ?
* Peux tu écrire ta propre using reflexion ?
* Sais tu manipuler IEnumerable avec LINQ ?
* Quelle est ton expérience avec les database ? Sais tu écrire une requête ? jointure ?

## Candidat avec connaissance Entity Framework

### Delete

#### Mise en place

Demander comment construire un delete à partir d'une id \(Question de base, assez simple\)

Amener le débat : Que penses tu de la performance ?

#### Résultat attendu

Peu performant, Entity Framework n'est pas capable d'écrire DELETE FROM TABLE WHERE ID = id : Un appel en base.

Obligé de faire 2 appels en base.

Le candidat doit être suffisament sûre de lui pour ne pas remettre en question son code lorsqu'on lui demande ce qu'il pense de la performance de ce qu'il vient d'écrire.

Il doit en suite faire preuve de bon sens pour expliquer clairement en quoi ceci est une limite du framework Entity.

#### Difficulté

Facile si le candidat est sûre de lui.

## 



