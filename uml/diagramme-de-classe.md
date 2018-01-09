# Diagramme de classe

Article passant rapidement en revue les différentes relations du diagramme de classe pour un rappel rapide :

L'avantage d'être concis, mais il commence à dater. \(2003\)

[http://www.linux-france.org/prj/edu/archinet/DA/fiche-uml-relations/fiche-uml-relations.html](http://www.linux-france.org/prj/edu/archinet/DA/fiche-uml-relations/fiche-uml-relations.html "Classe")

## Relation structurelle \(ou association\)

```
  class Contrat {
        Client bénéficiaire;
        ...
      }
```

![](/assets/association.png)

L'association : Trait plein pouvant être orienté

Représentation orientée :

**Contrat -&gt; Client **\(Contrat a un Client\)

### Composition et agregation

### ![](/assets/LOM_base_schema.png)

Le schéma est composé de "General, LifeCycle, ..."

Technical compose le LOMv1.0schema

Technical est dans le LOMv1.0schema

Relation de spécialisation/généralisation

```
      class ClientProfessionnel : Client {

        ...
      }
```

![](/assets/specialisation.png)

Héritage : Flèche fermée  \(avec triangle blanc\) orienté de la classe spécialisée vers son modèle

## Relation de réalisation

```java
      class SaisirClient extends JFrame implements ActionListener {

        ...
        public void actionPerformed(ActionEvent e) {
           // faire quelque chose
        }
      }
```

Implémentation d'une interface

![](/assets/realisation.png)

SaisirClient implémente l'interface ActionListener

## Relation de dépendance

```
      class Contrat {

        ...
        public void impression() {
           Printer imprimante = PrinterFactory.getInstance();
           ...
           imprimante.print(client.getName());
          ...
        }
      }
```

![](/assets/dependance.png)

Contrat n'implémente pas l'interface Printer, mais dépend de Printer car il utilise un objet implémentant l'interface Printer dans l'une de ses méthodes.

