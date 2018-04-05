# Concevoir une application ASP .net Core à partir de 0

Source : [https://www.twitch.tv/videos/229614321](https://www.twitch.tv/videos/229614321)

Cette vidéo illustre la conception et le développement d'une application ASP .NET CORE en 8 heures par @csharpfritz featuring @spboyer

* **Premièrement, on conçoit l'API de l'application \(Bien séparer le back du \(des ?\) front\)**

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

* **Deuxième étape après l'API, Travailler sur la base de donnée \(Entity Framework sera utilisé\)**

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



