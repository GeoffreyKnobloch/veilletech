# Visitor

[http://www.dofactory.com/net/visitor-design-pattern](http://www.dofactory.com/net/visitor-design-pattern "Visitor")

![](/assets/visitor.gif)

[https://www.codeproject.com/Articles/588882/TheplusVisitorplusPatternplusExplained](https://www.codeproject.com/Articles/588882/TheplusVisitorplusPatternplusExplained "Visitor")

Cet article explique de façon très claire l'intérêt du Visitor Pattern

Une fois mis en place, exemple d'utilisation du pattern :

```c\#
Document doc = ...;
HtmlVisitor visitor = new HtmlVisitor();
doc.Accept(visitor);
Console.WriteLine("Html:\n" + visitor.Output);
```



