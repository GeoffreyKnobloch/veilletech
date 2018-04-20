# Middleware

## Présentation du Middleware

Un Middleware est un logiciel assemblé dans la pipeline d’une application pour gérer les requêtes et les réponses.

Chaque composant :

* Choisit de passer la requête au composant suivant dans la pipeline ou pas.

* Peut agir avant et après que le composant suivant dans la pipeline soit invoqué.

Les délégués de requêtes sont utilisé pour construire la pipeline de requête.

Les délégués de requêtes gérent chaques requêtes HTTP.

Les délégués de requêtes sont configurés en utilisant les méthodes d’extension

* Run

* Map

* Use

Un délégué de requête peut être spécifié en une ligne, en tant que méthode anonyme \(on appelle ça un in-line middleware\), ou il peut être défini dans une classe réutilisable.

Ces classes réutilisables et méthodes anonymes en une ligne sont des middlewareou Composant Middleware.

Chaque composant middleware dans la pipeline de requête est réponsable pour invoquer le prochain composant dans la pipeline, ou pour court-circuiter la chaine, si cela est approprié.

Pour mieux comprendre la différence entre la pipeline de requête en ASP .NET Core et en ASP .NET 4.x :[https://docs.microsoft.com/en-US/aspnet/core/migration/http-modules?view=aspnetcore-2.1](https://docs.microsoft.com/en-US/aspnet/core/migration/http-modules?view=aspnetcore-2.1)

## Créer un pipeline middleware avec IApplicationBuilder

En ASP .NET Core, la pipeline de requête est constituée d’une séquence de délégué de requête appelé l’un après l’autre :

![](https://lh4.googleusercontent.com/CNZn8ryZV3evXTkCPMIwJxBoxq6hELNt9mCzyFPAvWqvDtIQJXxI_0TBZ-wa5DLVCb5cTdfx72evMdZcjST25M0DtGt1akH4ajXaisQJuneJ7owpfcB9UIzkExOy2OSvdPqQxal7)

Chaque délégués peuvent faire une opération avant et après le prochain délégué.

Un délégué peut aussi décider de ne pas passer une requête pour le délégué suivant, ce qui s’appelle un Court-circuit.

Court circuiter est utilisé pour éviter du traitement non nécessaire.

Danc cet exemple :

`using Microsoft.AspNetCore.Builder;`

`using Microsoft.AspNetCore.Hosting;`

`using Microsoft.AspNetCore.Http;`

`public class Startup`

`{`

`public void Configure(IApplicationBuilder app)`

`{`

`app.Run(async context =>`

`{`

`await context.Response.WriteAsync("Hello, World!");`

`});`

`}`

`}`

Une seule méthode anonyme est appelée, pour toutes les requêtes HTTP.

Ce n’est pas réellement une pipeline de requête.

Pour toute requête, ce middleware est Run :

`async context =>`

`{`

`await context.Response.WriteAsync("Hello, World!");`

`});`

Le premier app.Run\(délégué\) termine le pipeline.

On peut utiliser plusieurs délégués de requête ensemble avec

app.Use\(délégué\)

La paramètre next représente le prochain délégué dans la pipeline.

\(S’il n’est pas appelé, ce sera un court circuit\)

Exemple :

`public class Startup`

`{`

`public void Configure(IApplicationBuilder app)`

`{`

`app.Use(async (context, next) =>`

`{`

`// Do work that doesn't write to the Response.`

`await next.Invoke();`

`// Do logging or other work that doesn't write to the Response.`

`});`

`app.Run(async context =>`

`{`

`await context.Response.WriteAsync("Hello from 2nd delegate.");`

`});`

`}`

`}`

Donc le premier délégué travaille \(sur le contexte ?\) \(sans écrire dans la Response\), puis appelle next, puis continue de travailler \(sur le contexte ?\). \(sans écrire dans la Response\).

Le deuxième délégué écrit dans le contexte.Response.

Une remarque simple mais importante.

Il faut écrire une seule fois dans la Response.

Si la Response a déjà été Set, et qu’on l’édite à nouveau \(exemple, dans le premier délégué on écrit dans Response, puis dans le secong également\),

on va throw une exception, car Set plusieurs fois la Response risque de violer le protocol \(écire + que content-length\) ou corrompre le format du Body.

## Ordre

L’ordre dans lequel les composants middleware sont ajoutés dans la méthode Configure définit l’ordre par lequel ils sont invoqués lors d’une requête.

Et l’ordre inverse pour la réponse.

Par exemple :

`public void Configure(IApplicationBuilder app)`

`{`

`app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions`

`// thrown in the following middleware.`

`app.UseStaticFiles(); // Return static files and end pipeline.`

`app.UseAuthentication(); // Authenticate before you access`

`// secure resources.`

`app.UseMvcWithDefaultRoute(); // Add MVC to the request pipeline.`

`}`

L’exception Handler est le premier middleware appelé, il peut court-circuiter le reste si une exception arrive.

Il peut également catch les exceptions au retour !

Le middleware static file est dans les premiers également car il court-circuite quand il le faut.

Si le static file ne court-circuite pas la requête, elle passe au middleware Identity.

Ce midleware \(Identity\) construit l’authentification.

Identity ne court-circuite pas une requête non authentifiée.

Identity a le rôle d’authentifier les requêtes, mais authorisation \(et la réjection\) ne se passe que lorsque MVC sélectionne une page Razor ou un controller et une action.

Autre exemple :

`public void Configure(IApplicationBuilder app)`

`{`

`app.UseStaticFiles(); // Static files not compressed`

`// by middleware.`

`app.UseResponseCompression();`

`app.UseMvcWithDefaultRoute();`

`}`

Dans cet exemple, le midleware n’aura pas la possibilité d’être appelé lorsque le midleware static file court circuite la requête.

En revanche, pour une page retournée par MVC \(Razor page ou Controller et Action\), la réponse sera bien compressé par le middleware de compression de réponse.

## Use, Run et Map

Use : Peut court circuiter la pipeline si il n’appelle pas next

Run : Est une convention et des composants middleware peuvent exposer Run\[Middleware\] qui run et termine la pipeline.

Map : Ces extensions sont utilisées par convention pour “brancher” la pipeline.

Exemple :

`public class Startup`

`{`

`private static void HandleMapTest1(IApplicationBuilder app)`

`{`

`app.Run(async context =>`

`{`

`await context.Response.WriteAsync("Map Test 1");`

`});`

`}`

`private static void HandleMapTest2(IApplicationBuilder app)`

`{`

`app.Run(async context =>`

`{`

`await context.Response.WriteAsync("Map Test 2");`

`});`

`}`

`public void Configure(IApplicationBuilder app)`

`{`

`app.Map("/map1", HandleMapTest1);`

`app.Map("/map2", HandleMapTest2);`

`app.Run(async context =>`

`{`

`await context.Response.WriteAsync("Hello from non-Map delegate. <p>");`

`});`

`}`

`}`

Requête ⇒ réponse :

* localhost:1234 ⇒ hello from non-Map delegate.

* localhost:1234/map1 ⇒ Map Test 1

* --------------------/map2 ⇒ Map Test 2

* --------------------/map3 ⇒ hello from non-Map delegate

C’est du mapping sur url en fait.

Lorsque Map est utilisé, le chemin qui match est supprimé de HttpRequest.Path et il est rajouté à httpRequest.PathBase

Utilisation de MapWhen :

MapWhen branche la pipeline de requête en sa basant sur le résultat d’un prédicat donné.

Tous les prédicats de type

Func&lt;HttpContext, bool&gt; peuvent être utilisés pour maper la requête sur une nouvelle branche du pipeline.

Exemple :

`public class Startup`

`{`

`private static void HandleBranch(IApplicationBuilder app)`

`{`

`app.Run(async context =>`

`{`

`var branchVer = context.Request.Query["branch"];`

`await context.Response.WriteAsync($"Branch used = {branchVer}");`

`});`

`}`

`public void Configure(IApplicationBuilder app)`

`{`

`app.MapWhen(context => context.Request.Query.ContainsKey("branch"),`

`HandleBranch);`

`app.Run(async context =>`

`{`

`await context.Response.WriteAsync("Hello from non-Map delegate. <p>");`

`});`

`}`

`}`

Dans cet exemple, si une requête contient la clé “branch”, alors on part sur la branche de pipeline de requête HandleBranch,

sinon on continue sur la branche par défaut.

Requête ⇒ réponse :

* localhost:1234 ⇒ hello from non-Map delegate.

* localhost:1234/?branch=master ⇒ branch used = master

Map support le nesting et peut aussi match plusieurs segments en une fois :

Nesting :

app.Map\("/level1", level1App =&gt; {

level1App.Map\("/level2a", level2AApp =&gt; {

// "/level1/level2a"

//...

}\);

level1App.Map\("/level2b", level2BApp =&gt; {

// "/level1/level2b"

//...

}\);

}\);

Plusieurs segments à la fois :

app.Map\("/level1/level2", HandleMultiSeg\);

## Les middleware fournis par .NET Core

[https://docs.microsoft.com/en-US/aspnet/core/fundamentals/middleware/?view=aspnetcore-2.1&tabs=aspnetcore2x\#built-in-middleware](https://docs.microsoft.com/en-US/aspnet/core/fundamentals/middleware/?view=aspnetcore-2.1&tabs=aspnetcore2x#built-in-middleware)

## Ecrire son middleware

Un midleware est en général encapsulé dans une classe.

Exemple :

`public class Startup`

`{`

`public void Configure(IApplicationBuilder app)`

`{`

`app.Use((context, next) =>`

`{`

`var cultureQuery = context.Request.Query["culture"];`

`if (!string.IsNullOrWhiteSpace(cultureQuery))`

`{`

`var culture = new CultureInfo(cultureQuery);`

`CultureInfo.CurrentCulture = culture;`

`CultureInfo.CurrentUICulture = culture;`

`}`

`// Call the next delegate/middleware in the pipeline`

`return next();`

`});`

`app.Run(async (context) =>`

`{`

`await context.Response.WriteAsync(`

`$"Hello {CultureInfo.CurrentCulture.DisplayName}");`

`});`

`}`

`}`

L’exemple suivant utilise un middleware qui recherche le param culture de la requête appelante pour la stocker dans l’application, puis appelle le middleware suivant pour continuer la pipeline de requête.

\(Pour montrer, car il y a quelque chose de fourni par ASP.NET pour cela.

Source :[https://docs.microsoft.com/en-US/aspnet/core/fundamentals/localization?view=aspnetcore-2.1](https://docs.microsoft.com/en-US/aspnet/core/fundamentals/localization?view=aspnetcore-2.1)\)

Exemple d’URL pour tester ce middleware :

[http://localhost:7997/?culture](http://localhost:7997/?culture)= no

Encapsulation :

`using Microsoft.AspNetCore.Http;`

`using System.Globalization;`

`using System.Threading.Tasks;`

`namespace Culture`

`{`

`public class RequestCultureMiddleware`

`{`

`private readonly RequestDelegate _next;`

`public RequestCultureMiddleware(RequestDelegate next)`

`{`

`_next = next;`

`}`

`public Task InvokeAsync(HttpContext context)`

`{`

`var cultureQuery = context.Request.Query["culture"];`

`if (!string.IsNullOrWhiteSpace(cultureQuery))`

`{`

`var culture = new CultureInfo(cultureQuery);`

`CultureInfo.CurrentCulture = culture;`

`CultureInfo.CurrentUICulture = culture;`

`}`

`// Call the next delegate/middleware in the pipeline`

`return this._next(context);`

`}`

`}`

`}`

Classe d’extension :

`using Microsoft.AspNetCore.Builder;`

`namespace Culture`

`{`

`public static class RequestCultureMiddlewareExtensions`

`{`

`public static IApplicationBuilder UseRequestCulture(`

`this IApplicationBuilder builder)`

`{`

`return builder.UseMiddleware<RequestCultureMiddleware>();`

`}`

`}`

`}`

Utilisation :

`public class Startup`

`{`

`public void Configure(IApplicationBuilder app)`

`{`

`app.UseRequestCulture();`

`app.Run(async (context) =>`

`{`

`await context.Response.WriteAsync(`

`$"Hello {CultureInfo.CurrentCulture.DisplayName}");`

`});`

`}`

`}`

## Dépendances par requête

Possibilité d’injecter une dépendance dans le middleware.

Exemple :

`public class MyMiddleware`

`{`

`private readonly RequestDelegate _next;`

`public MyMiddleware(RequestDelegate next)`

`{`

`_next = next;`

`}`

`public async Task Invoke(HttpContext httpContext, IMyScopedService svc)`

`{`

`svc.MyProperty = 1000;`

`await _next(httpContext);`

`}`

`}`



