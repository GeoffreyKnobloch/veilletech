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

Mise en place :

```c\#
public interface IVisitor {
  void Visit(PlainText docPart);
  void Visit(BoldText docPart);
  void Visit(Hyperlink docPart);
}
```

```c\#
public class HtmlVisitor : IVisitor {
  public string Output { 
    get { return this.m_output; }
  }
  private string m_output = "";

  public void Visit(PlainText docPart) {
    this.Output += docPart.Text;
  }

  public void Visit(BoldText docPart) {
    this.m_output += "<b>" + docPart.Text + "</b>";
  }

  public void Visit(Hyperlink docPart) {
    this.m_output += "<a href=\"" + docPart.Url + "\">" + docPart.Text + "</a>";
  }
}
```

```c\#
public abstract class DocumentPart {
  public string Text { get; private set; }
  public abstract void Accept(IVisitor visitor);
}

public class PlainText : DocumentPart { 
  public override void Accept(IVisitor visitor) {
    visitor.Visit(this);
  }
}

public class BoldText : DocumentPart { 
  public override void Accept(IVisitor visitor) {
    visitor.Visit(this);
  }
}

public class Hyperlink : DocumentPart {
  public string Url { get; private set; }

  public override void Accept(IVisitor visitor) {
    visitor.Visit(this);
  }
}

public class Document {
  private List<DocumentPart> m_parts;

  public void Accept(IVisitor visitor) {
    foreach (DocumentPart part in this.m_parts) {
      part.Accept(visitor);
    }
  }
}
```

Donc c'est juste que DocumentPart définit la signature de la méthode Accept qui fait que le visitor visite \(la part\)

Pour chaque visitor, un comportement :

Le visiteur Html veut obtenir la version html du document donc on implémente visite\(DocPart\) de façon à ce qu'il allimente son output avec le résultat Html.

D'ou l'utilisation finale :

```
Document doc = ...;
HtmlVisitor visitor = new HtmlVisitor();
doc.Accept(visitor);
Console.WriteLine("Html:\n" + visitor.Output);
```



