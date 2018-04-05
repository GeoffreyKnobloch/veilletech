# Concevoir une application ASP .net Core à partir de 0

Source : [https://www.twitch.tv/videos/229614321](https://www.twitch.tv/videos/229614321)

Cette vidéo illustre la conception et le développement d'une application ASP .NET CORE en 8 heures par @csharpfritz featuring @spboyer

* Premièrement, on conçoit l'API de l'application \(Bien séparer le back du \(des ?\) front\)

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
        * 39:25 : Continuer \(il va consulter son api via url pour voir ce qu'il obtient\)



