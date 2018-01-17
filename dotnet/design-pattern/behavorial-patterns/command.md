# Command

[http://www.dofactory.com/net/command-design-pattern](http://www.dofactory.com/net/command-design-pattern "Command")

![](/assets/command.gif)

Client a un Receiver.

ConcreteCommand hérite de Command.

ConcreteComment a un Receiver aussi.

Un Invoker a des Command \(Un agrégat de commandes\)

Article permettant une compréhension plus aisée :

[http://www.dotnettricks.com/learn/designpatterns/command-design-pattern-dotnet](http://www.dotnettricks.com/learn/designpatterns/command-design-pattern-dotnet "Command")

## Quand l'utiliser ?

Callback, redo, undo, execution différée, ...

[https://scottlilly.com/c-design-patterns-the-command-pattern/](https://scottlilly.com/c-design-patterns-the-command-pattern/ "Command")

Un exemple via un Compte bancaire \(Business Object Receiver\) sur lequel on veut ajouter/retirer de l'argent, mais on stocke les commandes et les execute en différé.

## Mise en place

[https://github.com/GeoffreyKnobloch/DesignPatternCommand](https://github.com/GeoffreyKnobloch/DesignPatternCommand)

Voir solution exemple

