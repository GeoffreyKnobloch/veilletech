# ASP.NET MVC 5

## Les bases du MVC

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



