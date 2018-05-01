# La View

Source : [https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-view](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-view)

controller : return View\(\) retourne la vue du même nom que l'action du controller par convention.

La View Index dans le dossier HelloWorld retournée par l'action Index\(\) lorsqu'il return View\(\) :

```
@{
    ViewBag.Title = "Index";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2>Index</h2>
```

En fait, dans le fichier ViewStart.cshtml, est exécuté du code communément à toute les vues. Donc sauf si une vue a besoin d'un layout particulier, on peut supprimer la ligne qui définit le Layout, et laisser ça sur \_ViewStart, qui par défaut est :

```
@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}
```

## Passer de la data du Controller vers la View

Un View Template ne devrait JAMAIS avoir de logique business, ou intéragir avec la DataBase directement.

3 façons de passer de la data du controller vers la View :

Source : [https://blogs.msdn.microsoft.com/rickandy/2011/01/28/dynamic-v-strongly-typed-views/](https://blogs.msdn.microsoft.com/rickandy/2011/01/28/dynamic-v-strongly-typed-views/)

* Le viewbag
* Passer un type dynamique \(en utilisant @model dynamic\)
* Un objet ViewModel fortement typé

### Le viewbag

Une des façons de passer de la data à la View : le ViewBag :

```
        public ActionResult Welcome(string name, int numTimes = 1)
        {
            ViewBag.Message = "Hello " + name;
            ViewBag.NumTimes = numTimes;

            return View();
        }
```

La vue correspondante qui exploite cette data :

```
@{
    ViewBag.Title = "Welcome";
}

<h2>Welcome</h2>

<ul>
    @for (int i = 0; i < ViewBag.NumTimes; i++)
    {
        <li>@ViewBag.Message</li>
    }
</ul>
```

### Passage d'un type dynamique

```
using System.Collections.Generic;
using System.Web.Mvc;

namespace Mvc3ViewDemo.Controllers {

    public class Blog {
        public string Name;
        public string URL;
    }

    public class HomeController : Controller {

        List<Blog> topBlogs = new List<Blog>
      { 
new Blog { Name = "ScottGu", URL = "http://weblogs.asp.net/scottgu/"},
new Blog { Name = "Jon Galloway", URL = "http://weblogs.asp.net/jgalloway"},
new Blog { Name = "Scott Hanselman", URL = "http://www.hanselman.com/blog/"}
      };

        public ActionResult IndexNotStonglyTyped() {
            return View(topBlogs);
        }

        public ActionResult StonglyTypedIndex() {
            return View(topBlogs);
        }

        public ActionResult IndexViewBag() {
            ViewBag.BestBlogs = topBlogs;
            return View();
        }
    }
}
```

Pour utiliser un type dynamic, la view correspondante sera :

```
@model dynamic

@{
    ViewBag.Title = "IndexNotStonglyTyped";
}

<h2>Index Not Stongly Typed</h2>

<p>
 <ul>
@foreach (var blog in Model) {
   <li>
    <a href="@blog.URL">@blog.Name</a>
   </li>   
}
 </ul>
</p>
```

qu'il faudra construire sans aucune aide de l'intellisence car le type est determiné au build.

### Passage d'un ViewModel fortement typé

La façon la plus clean est de passer la data via un ViewModel \(objet du model fortement typé\). La view correspondante :

```
@model IEnumerable<MvcViewDemo.Controllers.Blog>

@{
    ViewBag.Title = "IndexNotStonglyTyped";
}

<h2>Index Not Stongly Typed</h2>

<p>
 <ul>
@foreach (var blog in Model) {
   <li>
    <a href="@blog.URL">@blog.Name</a>
   </li>   
}
 </ul>
</p>
```

ou on bénéficiera de l'aide de l'intellissence car on sait ce qu'est le Model.

