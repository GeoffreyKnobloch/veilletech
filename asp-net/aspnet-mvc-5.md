# ASP.NET MVC 5

## Reprise des bases du MVC

Source :[https://openclassrooms.com/courses/apprendre-asp-net-mvc](https://openclassrooms.com/courses/apprendre-asp-net-mvc)

Pour comprendre la philosophie d’une application MVC sans aborder de notions avancées, ce cours de 30 heures est convenable. Ce cours est parfaitement adapté pour des gens qui n’ont aucune notion d’ASP \(ou même aucune notion de programmation\).

### Le Controller

Source : [https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-controller](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-controller)

Le controller permet de rediriger une requête vers une view à laquelle il fournit de la data du Model layer.

Exemple :

```
using System.Web;
using System.Web.Mvc;

namespace MvcMovie.Controllers
{
public class HelloWorldController : Controller
{
//
// GET: /HelloWorld/

public string Index()
{
return "This is my <b>default</b> action...";
}

//
// GET: /HelloWorld/Welcome/

public string Welcome()
{
return "This is the Welcome action method...";
}
}
}
```

Par défaut, le routing est assuré ainsi :

`/[Controller]/[ActionName]/[Parameters]`

La route est personnalisable dans la classe RouteConfig :

```
public static void RegisterRoutes(RouteCollection routes)
{
routes.IgnoreRoute("{resource}.axd/{*pathInfo}");


routes.MapRoute(
name: "Default",
url: "{controller}/{action}/{id}",
defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
);
}
```

Simplement, lorsqu’on envoie l’url :

Serveur/ ⇒ Serveur/home/index

Serveur/Home ⇒ Serveur/home/index

Serveur/Autre ⇒ Serveur/Autre/index

Serveur/Autre/edit/1 ⇒ Serveur/Autre/edit/1 ⇒ Controlleur AutreController, méthode Edit, argument : 1.

Utilisation des paramètres :

```
        // GET : HelloWorld/Welcome/?name=Geoffrey&numtimes=4
        public string Welcome(string name, int numTimes = 1)
        {
            return HttpUtility.HtmlEncode("Hello " + name + ", NumTimes is: " + numTimes);
        }
```

Ici on exploite : {controller} {action} mais toujours pas {id}, on a passé les paramètres names et numTimes en Query string.

Pour utiliser {id} dans la configuration de route, il faut un paramètre id dans la méthode.

```
       // GET : HelloWorld/Welcome2/1?name=Geoffrey
        public string Welcome2(string name, int ? id)
        {
            return HttpUtility.HtmlEncode("Hello " + name + ", id is: " + id);
        }
```

En ASP.NET MVC, il est d'usage de passer les paramètres comme data de route \(comme l'id\) plutôt qu'en Query string.

```
public class RouteConfig
{
   public static void RegisterRoutes(RouteCollection routes)
   {
      routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

      routes.MapRoute(
          name: "Default",
          url: "{controller}/{action}/{id}",
          defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
      );

      routes.MapRoute(
           name: "Hello",
           url: "{controller}/{action}/{name}/{id}"
       );
   }
}
```

Ici appeler Controller/action/nom/id aura pour effet d'appeler l'action du controller et lui passer en parametre nom pour le paramètre name et id pour le paramètre id.

Appel :

Il suffit donc d'appeler /Helloworld/welcome2/geoffrey/1 pour avoir le même résultat que précédement, maintenant qu'on a ajouté cette route.

On a passé des route data plutôt que des query string.

Pour beaucoup d'applications MVC, la route par défaut est suffisante. On passe plutôt de la data en utilisant le model binder.

Pour passer les paramètres en Query string vers les paramètres de la méthode, le model binding system de ASP.NET MVC est utilisé.

Source : [https://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx](https://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)

### La View

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

#### Passer de la data du Controller vers la View

Un View Template ne devrait JAMAIS avoir de logique business, ou intéragir avec la DataBase directement.

3 façons de passer de la data du controller vers la View :

Source : [https://blogs.msdn.microsoft.com/rickandy/2011/01/28/dynamic-v-strongly-typed-views/](https://blogs.msdn.microsoft.com/rickandy/2011/01/28/dynamic-v-strongly-typed-views/)

* Le viewbag
* Passer un type dynamique \(en utilisant @model dynamic\)
* Un objet ViewModel fortement typé

##### Le viewbag

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

##### Passage d'un type dynamique

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

##### Passage d'un ViewModel fortement typé

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

### Le Model

Source : [https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-model](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-model)

#### Code first avec Entity Framework

  
Entité POCO :

  


using System;

  


namespace MvcMovie.Models

{

public class Movie

{

public int ID { get; set; }

public string Title { get; set; }

public DateTime ReleaseDate { get; set; }

public string Genre { get; set; }

public decimal Price { get; set; }

}

}

  


Avec son DbContext :

  


using System;

using System.Data.Entity;

  


namespace MvcMovie.Models

{

public class MovieDBContext : DbContext

{

public DbSet&lt;Movie&gt; Movies { get; set; }

}

}

  


MovieDBContext représente le contexte Database de Movie qui prend en charge tout : Select, modifier, supression, …

  


Titre 4 : DataBase first

  


Source :[https://docs.microsoft.com/en-US/aspnet/visual-studio/overview/2013/aspnet-scaffolding-overview](https://docs.microsoft.com/en-US/aspnet/visual-studio/overview/2013/aspnet-scaffolding-overview)

  


Il est possible de générer du code pour une app MVC ou une Web API à partir de la base de donnée.

  


Titre 3 : Créer une Connection String et travailler avec SQL Server LocalDB

  


LocalDB est une version légère de SQL Server Express Database Engine qui démarre sur demande, et tourne en mode utilisateur.

LocalDB tourne tourne dans un mode d’exécution spécial de SQL Server Express qui nous permet de travailler avec une database en fichiers .mdf

  


Typiquement, les fichiers d’une database LocalDB sont dans le fichier App\_Data du projet Web.

  


LocalDB n’est pas fait pour être déployé en production car ce n’est pas designé pour fonctionner avec IIS.

En revanche, une database LocalDB peut facilement être migrée sur SQL Server ou SQL Azure.

  


Donc on a le schémas : Code first -&gt; Génére LocalDB -&gt; Migre sur SQL Server

Plus d’informations sur les connexions String :

[https://msdn.microsoft.com/library/jj653752.aspx](https://msdn.microsoft.com/library/jj653752.aspx)

  


Par défaut, Entity Framework se base sur une connexion string \(créée par défaut\) dans Web.config \(pas Web.config dans le dossier View\) :

  


Web.config :

&lt;connectionStrings&gt;

&lt;add name="DefaultConnection" connectionString="Data Source=\(LocalDb\)\MSSQLLocalDB;Initial Catalog=aspnet-MvcMovie-fefdc1f0-bd81-4ce9-b712-93a062e01031;Integrated Security=SSPI;AttachDBFilename=\|DataDirectory\|\aspnet-MvcMovie-fefdc1f0-bd81-4ce9-b712-93a062e01031.mdf" providerName="System.Data.SqlClient" /&gt;

&lt;add name="MovieDBContext" connectionString="Data Source=\(LocalDb\)\MSSQLLocalDB;Initial Catalog=aspnet-MvcMovie;Integrated Security=SSPI;AttachDBFilename=\|DataDirectory\|\Movies.mdf" providerName="System.Data.SqlClient" /&gt;

&lt;/connectionStrings&gt;

  


Il y a par défaut une DefaultConnexion, qui est utilisée pour le membership database, pour controler qui peut accéder à l’application.

Pour en savoir plus sur la database de membership, voir :

[https://docs.microsoft.com/fr-fr/aspnet/core/security/authorization/secure-data](https://docs.microsoft.com/fr-fr/aspnet/core/security/authorization/secure-data)

  


Il faut rajouter une seconde connexion string, ici MovieDbContext \(qui match la classe MovieDBContext\), afin de maitriser par exemple le fichier qui servira de DB :

AttachDBFilename=\|DataDirectory\|\Movies.mdf"

  


On remarquera néanmoins que la création de la connexionString MovieDBContext n’était même pas obligatoire, car si on ne spécifie pas de Connexion String, Entity Framework va créer automatiquement une Database LocalDB dans le dossier de l’utilisateur, avec le nom totalement qualifié du DbContext \(ici MvcMovie.Models.MovieDBContext\).

  


Titre 2 : Accéder au Model depuis le Controller

  


Source :[https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller)

  


Lors de l’ajout d’un Controller, il y a plusieurs choix de template.

Un template intéressant est MVC 5 Controller with views using Entity Framework.

ça fait tout de suite un CRUD.

  


![](https://lh4.googleusercontent.com/Q5mO6cd6HoZf-CbFwePTYJRWObECloPASGrf1pSufCwfTpScyPx2NV3M-m3zK16qD1MczWACDL7pFQ5B6DBneHCeBg4nwzymYG15tGD-Oe97c31iQYa12_flVEI775SJ3f8YYTvw)

  


  


Il suffit alors de sélectionner l’entity et le DbContext qui porte le DbSet de l’entity.

  


![](https://lh6.googleusercontent.com/pVULsz0PMi3ZuHIsV1eXMtIkQjsUnXLJts7QeqTzcleq1ouwngqLJAkSvr1roN4AfBM-oj_BBGJzF21mV9cXyEvNtQvEcOKl4a50TfYGBAzY_5eha0vxmgpLwAnW1n5jUdQG3fYA)

  


Visual Studio crée automatiquement un CRUD sur l’entity en question :

Un Controller, un dossier Views/Movies

Les views : create, delete, details, edit, index.

  


Pratique pour avoir une base, puis personnaliser ensuite.

Personellement, je recommande de remplacer le DbContext du controller par une classe Service à injecter par injection de dépendance.

Ce Service va appeler un Data Acces Object, pour obtenir des Business Object, puis les traduire en DTO \(Data Transfer Object\) qui correspondent exactement à ce que la View a besoin.

  


Pour plus d’informations sur EntityFramework :

[https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)

  


Voici ce qui a été créé pour l’Edit dans le controller :

  


// GET: /Movies/Edit/5

public ActionResult Edit\(int? id\)

{

if \(id == null\)

{

return new HttpStatusCodeResult\(HttpStatusCode.BadRequest\);

}

Movie movie = db.Movies.Find\(id\);

if \(movie == null\)

{

return HttpNotFound\(\);

}

return View\(movie\);

}

  


// POST: /Movies/Edit/5

// To protect from overposting attacks, please enable the specific properties you want to bind to, for

// more details see http://go.microsoft.com/fwlink/?LinkId=317598.

\[HttpPost\]

\[ValidateAntiForgeryToken\]

public ActionResult Edit\(\[Bind\(Include="ID,Title,ReleaseDate,Genre,Price"\)\] Movie movie\)

{

if \(ModelState.IsValid\)

{

db.Entry\(movie\).State = EntityState.Modified;

db.SaveChanges\(\);

return RedirectToAction\("Index"\);

}

return View\(movie\);

}

  


2 actions Edit, un pour GET, un pour POST ! \(Ce petit problème n’est plus présent quand on utilise Razor Page.\)

  


  


On constate l’attribut Bind : Afin de ne recevoir QUE les propriétées désirée lors d’un POST.

On constate l’attribut ValidateAntiForgeryToken : Afin de se protéger contre les attaques cross-site Forgery.

Cet attribut va de paire avec un appel à @Html.AntiForgeryToken\(\) dans la View :

  


@model MvcMovie.Models.Movie

  


@{

ViewBag.Title = "Edit";

}

&lt;h2&gt;Edit&lt;/h2&gt;

@using \(Html.BeginForm\(\)\)

{

@Html.AntiForgeryToken\(\) 

&lt;div class="form-horizontal"&gt;

&lt;h4&gt;Movie&lt;/h4&gt;

&lt;hr /&gt;

@Html.ValidationSummary\(true\)

@Html.HiddenFor\(model =&gt; model.ID\)

  


&lt;div class="form-group"&gt;

@Html.LabelFor\(model =&gt; model.Title, new { @class = "control-label col-md-2" }\)

&lt;div class="col-md-10"&gt;

@Html.EditorFor\(model =&gt; model.Title\)

@Html.ValidationMessageFor\(model =&gt; model.Title\)

&lt;/div&gt;

&lt;/div&gt;

}

  


Plus d’infos sur cette attaque :[https://docs.microsoft.com/en-US/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages](https://docs.microsoft.com/en-US/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

  


Lors du Post, le model binder d’ASP.NET prend les valeurs du du POST et en crée un objet Movie dans le paramètre movie.

  


Titre 2 : Ajouter une barre de recherche

  


Source :

[https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-search](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-search)

