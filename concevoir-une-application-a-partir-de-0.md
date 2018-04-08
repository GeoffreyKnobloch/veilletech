# Concevoir une application ASP .net Core à partir de 0

Source : [https://www.twitch.tv/videos/229614321](https://www.twitch.tv/videos/229614321)

Cette vidéo illustre la conception et le développement d'une application ASP .NET CORE en 8 heures par @csharpfritz featuring @spboyer

## **Premièrement, on conçoit l'API de l'application \(Bien séparer le back du \(des ?\) front\)**

* Nouveau Projet - API
* Program.cs :
  * Similaire à une application console, sauf que BuildWebHost démarre le serveur Web qui s'appuie sur la classe Startup.
* Startup.cs :
  * ConfigureServices : Injection des dépendances \(services.AddMvc\(\)\)
  * Configure : Selon l'environnement, des comportements de l'application maitrisés.
* Architecture de l'API :

  * Dossier Models pour ajouter les entity POCO :

    * Trip.cs

      * Id : int
      * Name : str
      * StartDate : DateTimeOffset ==&gt; Car ça va stocker la timeZone

      * EndDate : DateTimeOffset

    * Segment.cs \(vol, ou hotel, ..\)

      * Id : int
      * TripId : int \(Un segment est rattaché à un Trip\)
      * Name : str
      * Description : str
      * StartDateTime : DateTimeOffset
      * EndDateTime : DateTimeOffset

    * Repository.cs : Du "Mocking" à la main, en dur, en attendant d'utiliser le framework d'accès en BDD.

      * MyTrips : List&lt;Trip&gt; private ici en dur initialiser cette variable privée avec 3 Trip
      * public : List&lt;Trip&gt; Get, =&gt; Get sur MyTrip
      * public void Add \(Trip NewTrip\) =&gt; Ajout sur MyTrip
      * public void Update \(Trip tripUpdate\) =&gt; Update selon l'Id
      * public void Remove \(int id\) =&gt; Remode selon l'Id

  * Dossier Controllers \(créé par défaut\) :

    * Comme nous avons fait un projet API, par défaut, un controller ValuesController a été créé.
    * Sur .net core le dossier Controllers est pour tous les controllers, pas de différence entre un dossier APIControllers et le reste des Controllers qui servaient au MVC pour diriger les Views.
    * Création d'un TripsController qui reprend tout le code de ValuesControllers 
    * Pour accéder aux données \(Repository.cs\), on va injecter cette dépendance dans Startup.cs
    * Modification de ConfigureServices -&gt; services.AddTransient&lt;Models.Repository&gt;\(\);
    * Ducoup dans le TripsController, on peut faire référence au Repository directement :

      * private Models.Repository \_repository; 
      * TripsController constructeur avec argument Repository repository qui affecte repository à \_repository
      * Get : retourne \_repository.Get\(\)
      * Put : \_repository.update
      * Post : \_repository.Add
      * Debug =&gt; Peut choisir la page par défaut : api/Trips c'est bien pratique, pour tester l'API nouvellement créée
      * Utilisation de Swagger :

        * Sur le projet d'API : Ajouter new nugget package : Swashbuckle.AspNetCore
        * et Swashbuckle.AspNetCore.SwaggerUI
        * Startup.cs :

          * ConfigureServices : 

          en dessous de services.AddMvc\(\),  
          services.AddSwaggerGen\(options =&gt; options.SwaggerDoc\("v1", new Info {Title = "Trip Tracker", Version = "v1" } \)  
          \);

          * Configure : 

          Au dessus de app.UseMvc : app.UseSwagger\(\); et app.UseSwaggerUI\( avec des options

        En utilisant ces options, /swagger dans l'url nous montre parfaitement l'API. Superbe façon de documenter et partager son API

      * Comme le test fait un réél DELETE, UPDATE, utiliser la variable env dans le Startup.cs est une excellente idée pour éviter d'avoir la géniale UI de swagger en prod.

* Enfin, mettre l'API sur GitHub :

  * New Repository
  * Public, .gitignore : VisualStudio, licence : Apache. Copier le lien pour download or clone ...
  * Win + R / Cmd / cd / DIR ==&gt; Répertoire voulu, ...
  * Apprendre Git de façon détaillé à l'occasion.

## **Deuxième étape après l'API, Travailler sur la base de donnée \(Entity Framework sera utilisé\)**

* EntityFramework pour SqlLite.

* Ouvrir .csproj et s'assurer qu'on a Microsoft.AspNetCore.All \(version 2.0.5 ici\). En faisant référence à ce package, ça fait référence à tous les package nécessaire à EntityFramework et d'autres choses, donc pas besoin d'ajouter de truc fancy pour utiliser EntityFramework.

* Ducoup tout le nécessaire est "magiquement" là, mais c'est pas magique, c'est en fait due à cette référence dans le csproj, qui est fait sur les projets asp net core par défaut.

* Quand on regarde d'ailleurs ducoup dans Dependencies / NuGet / Microsoft.AspNetCore.All / là on voit tous les packages qu'on a include automatiquement !! Et il y en a beaucoup, dont Entity Framework, SqlLite, SqlServer, ... bien évidemment.

* Ducoup on peut se demander !! Je ne veux pas déployer tous ces packages !!! En fait lors d'un déploiement, cela ne va déployer que les packages qu'on utilise réellement. Fort !

* Creer un folder Data

  * classe TripContext : DbContext

    * propriété DbSet&lt;Trip&gt; Trips \(Microsoft.EntityFrameworkCore\), 

* Dans Startup.cs :

  * Dans COnfigureServices : Ajout de AddDbContext&lt;TripContext&gt; \(options =&gt; options.UseSqlLite\("Date Source = YourDbFileName.db"\)

    * Conséquence : On a injecté la dépendance sur DbContext&lt;TripContext&gt; avec l'option de travailler sur Sql Lite.

* Dans le Controller TripsController :

  * Ajout d'une variable privée TripContext \_context \(qui permet l'accès en bdd\) et constructeur : \_context = context

  * Le fait d'avoir un TripContext en argument du constructeur, ASP .net \(behind the scene\) va instancier un TripContext lors de l'instanciation du controller TripsController.

  * Pour information, le controller est instancié par request, donc à chaque request, le controller est instancié, et un nouveau TripContext sera instancié.

  * Donc on précise bien qu'ici un TripContext est instancié dans le controller. Pour une application de taille un peu plus grande, faire plutôt comme j'ai fait pour Lorena.sln, Un nouveau dossier d'accès à la DATA \(ou projet ??\) qui accède à la DATA TripContext et qui encapsule le tout. Et le controller instance un objet de la DAL, et non un TripContext.

  * Data annotation : préciser Key, Requiered, etc. Mais plutôt sur le ViewModel je pense. Sinon on peut override une méthode dans le TripContext qui à l'initiatlisation se lance, et qui précise que la clé primaire c'est l'id par exemple. Mais par convention si la propriété est écrie Id ou TypeId alors c'est directement reconnu comme étant l'Id par entity framework \(Convention over configuration\). et let Get\(Id\) peut s'écrire \_tripContext.Find\(id\) tout simplement, sans passer par Linq t =&gt; t.Id == id\)

  * Possibilité de rendre son API asynchrone :

    * \[HttpGet\]

    public async Task&lt;IActionResult&gt; GetAsync\(\)

    {

    var trips = await \_context.Trips.AsNoTracking\(\).ToListAsync\(\);

    return Ok\(trips\);

    }

    AsNoTracking permet de gagner en performance car on ne garde pas les tracker objects dans notre contexte. On va throw le contexte direct aprés !

    Meilleur façon que de répetter sans cesse la AsNoTracking lorsqu'on sait qu'on ne va pas conserver les objets tracker dans notre utilisation :

    On peut modifier la classe TripContexte afin que lors de sa construction this.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    Plutôt dans la classe qui accéde à sa DATA que là en fait.. Donc ici dans le controller.

    EntityFramework add : context.Trips.Add\(value\); oucontext.Add\(value\) fonctionne également.context.SaveChanges\(\);

    * dans le POST ici , juste aprés : if \(!ModelStats.IsValid\) return BadRequest\(ModelState\) sinon add et return Ok\(\); on retourne dans tous les cas une ActionResult.

    * Update :context.Trips.Update\(value\);\_context.SaveChanges\(\);

    * Delete : \[HttpDelete\("{id}"\] public IActionResulte Delete\(int id\) {

    * var myTrip = context.Trips.Find\(id\); if \(myTrip == null\) return NotFound\(\); context.Trips.Remove\(myTrip\); context.SaveChanges\(\); return NoContent\(\);}

    * Remarque : Le delete sur entity Framework n'est pas possible en une seule ligne : DELETE FROM TRIPS WHERE ID = id

    * Donc Entity Framework n'est pas tres performant sur le delete. On peut imaginer une procédure stockée qui le fait ?

    TripContext :

    public void SeedData\(\) : Trips.AddRange\( ... va mettre 3 Trip en dur\)

    Context.EnsureDbCreated

## Troisième étape : Création d'un projet DTO

Création d'un projet .net Standard TripTrackerDTO \(Data Transfert Object\) sur lequel on copie l'intégralité des entity de l'API

Pourquoi .net Standard ?

[https://docs.microsoft.com/fr-fr/dotnet/standard/net-standard](https://docs.microsoft.com/fr-fr/dotnet/standard/net-standard)

* Définit un ensemble uniforme d’API de bibliothèque de classes de base pour toutes les implémentations de .NET à implémenter, indépendamment de la charge de travail.
* Permet aux développeurs de générer des bibliothèques portables utilisables sur toutes les implémentations de .NET, à l’aide de ce même ensemble d’API.
* Réduit ou même élimine une compilation conditionnelle de source partagée résultant des API .NET, uniquement pour les API de système d’exploitation.

Pour ne pas avoir de code dupliqué, les entity de l'API deviennent des classes héritant simplement des entity \(copiées à 100%\) du DTO.

Donc l'API référence le DTO.

Dans le DTO on rajoute une classe : TripWithSegments qui hérite de Trip et qui rajoute en plus la liste de Segment

ICollection&lt;Segment&gt; Segments

Rapelle de la méthode Get :

\[HttpGet\]

public async Task&lt;IActionResult&gt; GetAsynd\(\)

{

var trips = await \_context.Trips.AsNoTracking\(\).ToListAsync\(\);

return Ok\(trips\);

}

Trip ajoute également la List&lt;Segment&gt; Segments et dans la dal il fait un .Include\(t =&gt; t.Segments\) pour lire les segments en base aussi.

Et il s'en sert pour .Select \( et recevoir des TripWithSegments.

===&gt; Tout a fail avec ce conflit entre Trip et TripWithSegments ...

Donc go back to la fin de la deuxieme étape. Suppression du projet DTO. Gros fail.

\*\*\*\*\*\*\*\*\*\*\*\*\*

Invitation de Jon Galloway.

## Quatrième étape : Création de l'UI

New Project /TripTracker.UI / ASP Web App \(pas la MVC\) \(Razor\) Authentication : Individual User Account \(Compliqué d'ajouter après\)

Individual ou Work or School Account sont des options intéressantes.

### Le choix d'un projet ASP Web App

Il y a le choix pour l'UI :

* Application Web
* Application Web \(Model-View-Controller\)

Nous avons fait ici le premier choix : Application Web : on va donc utiliser des pages Razor

Dans le choix de MVC, on a Model, view , controller

ici on a juste des PAGES razor, pas nécessairement de controller, qui sera plutôt remplacé par du code behind razor page.

En MVC c'est du Razor aussi mais c'est des VIEWS

DOnc on a :

Dossier :

Controllers, Data, Extensions, Pages, Services, et les classes program.cs et startup.cs

On va build l'UI avec des pages Razor.

C'est comme une couche en plus du MVC \(razor\).

MVC normal c'est parfois un peu lourd ! Models deviennent ViewModel le controller les transmet à la View ...

C'est parfait ! Pour des très gros projet, sinon ça peut être overkill.

L'intérêt de Razor, c'est que c'est moins overkill pour des projets de taille pas trop grandes.

Ce n'est qu'un choix. MVC est good aussi.

Razor est + Page Focus, moins Model focus.

Razor pages n'est pas exactement nouveau, pas trop à apprendre ! Car en fait Razor pages est build on top of MVC

Rien de vraiment nouveau, juste une surcouche.

Exemple, explorons la page Index.cshtml.

==&gt; On peut voir le code behind Index.cshtml.cs, sur lequel je peux mettre une propriété qui pourra être accédée par la page Razor

\(Razor c'est juste en fait le cshtml que l'on connait avec les @\* pour faire du csharp avec des html helper @HtmlHelper...\)

Donc on n'a pas la "lourdeur" d'un controller qui doit renvoyer une ActionResult ...

Juste du conde behind qui implémente une méthode OnGet\(\).

"Code Behind ??? sound like WebForms ..."

C'est un PageModel, il faut y penser comme "MVVM",

MVVM : Code associé avec le FrontEnd, il y a du ViewModel qu'on peut bind à l'UI.

On ne connecte pas directement le backend Models. On a une page Models donc on set des propriétés à cette architecture orienté Page. comme du binding.

Enfait le code behind d'une page comme Index :

Index.cshtml : s'appue sur @model définir par une valeur.

@model est défini dans Index.cshtml.cs :

cette page est une classe IndexModel, celle-ci est un ViewModel qu'on peut personnalisé exactement à la page. On ne travaille pas directement avec le flow de Models. On a un ViewModel par défaut, avec un comportement OnGet qui va s'éxécuter quand les gens requiest la page Index, mais aussi des propriétés, des méthodes propres \(lié à l'UI bien sûre\).

Le problème c'était que par abus, des développeurs passaient depuis le Controller directement un model métier pour l'UI, et non forcément un ViewModel. Là on aura plus tendance à de toute façon devoir construire un ViewModel. On ne travaille pas sur le Model, et on évite ce qui devient vite un projet désorganisé.

Un seul Controller est créé par défaut ici : le AccountController.

Autre truc avec le MVC qui peut être un peu lourd :

Le controller pour AccountCOntroller par exemple :

Pour diriger vers la page ManageAccount, il va falloir créer une méthode public IActionResult ManageAccount\(\)

Celle ci pour le Get

Pour le Post,

Le view model subit donc des adaptations suivant le OnGet, OnPost, etc.

il va falloir créer la même méthode IActionResult ManageAccount\(Account compte\)

sauf qu'il sera décoré de \[HttpPost\].

2 méthodes extrémement similaire visuellement mais qui n'ont pas dutout les mêmes fonctionnalitées.

L'intérêt de Razor ici, c'est que dans le code behind \(Donc la classe ViewModel de la page\), il y a une méthode OnGet\(\) pour le Get, une méthode OnPost\(..\) pour le post, bien plus lisible

### Réalisation

Ajout d'un foler dans le dossier Pages : Trips

=&gt; Créer une nouvelle page

=&gt; En utilisant EntityFramework CRUD.

=&gt; Va générer 5 pages dans le dossier Trips, en s'appuyant sur TripContext, ce qui n'est pas la solution finale, mais au moins on peut s'appuyer sur ce code auto généré pour l'adapter.

#### Création du service qui consomme l'API : ApiClient

En suite on va créer la classe dans le dossier Services qui va appeler la data.

une interface :

IApiClient : Task&lt;List&lt;Trip&gt; GetTripAsync\(\)

Task&lt;Trip&gt; GetTripAsync\(\)

et une classe ApiClient qui va implémenter ces méthodes.

Pour implémenter ces méthodes, ApiClient utilise la classe HttpClient qui est la classe qui permet de lancer des requêtes et recevoir une réponse.

Donc on a plus qu'à implémenter le CRUD de l'API REST en utilisant HttpClient.

Comme l'API créée précédemment communique en Json pour le body des request, et que HttpClient.Read / Post / Delete / Put lisent des stream, on crée une extension de HttpClient afin d'implémenter ReadJson en extension.

Lorsque le service est terminé, il faut le référencer dans Startup.cs :

// Configuration de l'API Client :

```
        var httpClient = new HttpClient

        {

            BaseAddress = new Uri\(Configuration\["serviceUrl"\]\)

        };



        services.AddSingleton\(httpClient\);

        services.AddSingleton&lt;IApiClient, ApiClient&gt;\(\);
```

Car l'API prend en constructeur un HttpClient donc il faut le référencer aussi !!!!! et on en profite pour prendre la main sur sa BaseAdresse qui sera configurable !

#### Consommation de ce service par les Razor page

Les Razor page sont créées par défaut pour consommer un DbContext directement \(!!\) il suffit donc de faire les modifications nécessaires, pour ne plus travailler sur ce  dbContext avec Entity Framework, mais plutôt pour faire des appels à l'API, et plus directement au service ClientApi.

Donc chaque Razor Page va travailler sur un ApiClient \_client privé qui va faire les appels pour retrouver les Models.

#### Sécurisation via authentification

On a créé le projet Razor Pages en incluant l'authentification Individual User Account.

Nous allons exploiter ça.

Par défaut, tout est fonctionnel, et tout reste personnalisable.

Dans Startup, on peut constater que des RazorPagesOptions peuvent être spécifiées pour authoriser certaines pages.

Authorizer ==&gt; uniquement pour personne connectée.

La deuxième façon de prévenir l'accés par personne non connectée est \[Authorize\] sur les vm.

On peut également conditionner l'affichage de certaines choses avec @SignInManager.IsSignedIn\(User\)

On peut aussi conditionner avec des Policy custom.

Par exemple \(juste pour voir la possibilité, à ne pas imioter dans un code de production\) :

Dans Configure de Startup.cs :

services.AddAuthorization\(configure =&gt;

{

configure.AddPolicy\("CreateTrips", policy =&gt;

{

policy.RequireUserName\("Geoffrey@gmail.com"\).Build\(\);

}\)

}\);

Mieux : Role.

Mieux que Role : Claimed car Role manque encore de Granularité

Le mieux c'est donc du "CanPublish" ou "CanDoThat" granularisé et géré en dehors de l'application.

05:16 \(heure de sa pause sandwich.\)



