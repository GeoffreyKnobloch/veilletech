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

1. Need to implement callback functionalities.

2. Need to support Redo and Undo functionality for commands.

3. Sending requests to different receivers which can handle it in different ways.

4. Need for auditing and logging of all changes via commands.



