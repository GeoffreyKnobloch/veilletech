## L’injection de dépendances dans un projet ASP .NET Core {#net-core}

Source :[https://docs.microsoft.com/fr-fr/aspnet/core/fundamentals/dependency-injection](https://docs.microsoft.com/fr-fr/aspnet/core/fundamentals/dependency-injection)

Exemple de code très parlant :

Un Repository, un Service, un Controller, tous 3 construits par injection de dépendance !

[https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)

Avec injection de dépendance multiple dans un unique constructeur \(contrairement à la vision de 2009 ci-dessous ou on envisageait d’exposer une propriété publique \(!\)



La configuration se fait dans la classe Startup :

`public class Startup`

`{`

`// This method gets called by the runtime. Use this method to add services to the container.`

`publicvoidConfigureServices(IServiceCollectionservices)`

`{`

`services.AddDbContext<ApplicationDbContext>(options =>`

`options.UseInMemoryDatabase()`

`);`

`// Add framework services.`

`services.AddMvc();`

`// Register application services.`

`services.AddScoped<ICharacterRepository, CharacterRepository>();`

`services.AddTransient<IOperationTransient, Operation>();`

`services.AddScoped<IOperationScoped, Operation>();`

`services.AddSingleton<IOperationSingleton, Operation>();`

`services.AddSingleton<IOperationSingletonInstance>(newOperation(Guid.Empty));`

`services.AddTransient<OperationService, OperationService>();`

`}`

`// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.`

`publicvoidConfigure(IApplicationBuilderapp,IHostingEnvironmentenv)`

`{`

`if(env.IsDevelopment())`

`{`

`app.UseDeveloperExceptionPage();`

`}`

`app.UseStaticFiles();`

`app.UseMvcWithDefaultRoute();`

`}`

  




Le Conteneur de Service par défaut fourni par ASP .NET Core offre un ensemble minimal de fonctionnalités et n’a pas vocation à remplacer d’autres conteneurs.

Donc on arrive bien à la confirmation de l'intuition exposée précédemment :

Le framework de Conteneurs de Services était nécessaires avant pour injecter un Repository là ou le caller attend un IRepository.

En ASP.NET Core, un conteneur de sérvice par défaut est fourni.

### Explicit Dependencies Principle

Il faut respecter le principe de dépendances explicites pour maîtriser ce que fait une classe. Une des manière de le faire est d’exiger les dépendances dans le Constructeur, ou en paramètre de méthode pour une dépendance plus locale.

[http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/)

Le plus souvent, les classes déclarent leurs dépendances dans le constructeur, ce qui leur permet de suivre le principe de dépendances explicites. Cette approche est appelée “Injection de constructeurs”.

En ayant un couplage faible, on respecte le principe de l’inversion de dépendances

[http://deviq.com/dependency-inversion-principle/](http://deviq.com/dependency-inversion-principle/)

qui stipule qu’un module supérieur ne doit pas dépendre du module inférieur.

Et que tous doivent dépendre d’abstraction.

Pour faciliter l’instanciation de ces classes loosely coupled, on utilise des classes qui se chargent de l’instanciation, ces classes sont des Conteneurs d’inversions de contrôle.

ou des Conteneurs d’injection de dépendance.

Un Conteneur IOC \(Conteneur d’inversion de contrôle\) ou également appelé Conteneur d’injection de dépendance est une factory spécialisée utilisée pour faciliter l’injection de dépendance.

L’extraction de dépendance dans des interfaces, et la fourniture des implémentations de ces interfaces en tant que paramètres implémentent le design pattern de Stratégie

\(Strategy design pattern[http://deviq.com/strategy-design-pattern/](http://deviq.com/strategy-design-pattern/)\)

Le simple conteneur intégré dans ASP .NET Core est représenté par l’interface IServiceProvider.

Question de vocabulaire, ASP .NET fait référence aux types qu’il gère comme étant des services.

Donc dans le cadre de cette étude, on se référera aux services comme tel : comme étant les types gérés par le conteneur d’inversion de contrôle d’ASP .NET Core.

On configure les services \(ie les types gérés par le conteneur d'inversion de contrôle d’ASP .NET Core\) dans la méthode ConfigureService de la classe startup.

// TODO après avoir épeluché cet article : Voir comment fonctionne l’injection de dépendances des contrôleurs MVC :

[https://docs.microsoft.com/fr-fr/aspnet/core/mvc/controllers/dependency-injection](https://docs.microsoft.com/fr-fr/aspnet/core/mvc/controllers/dependency-injection)

### Exemples et explications

Startup.cs :

// This method gets called by the runtime. Use this method to add services to the container.  
publicvoidConfigureServices\(IServiceCollection services\)  
{  
// Add framework services.  
services.AddDbContext&lt;ApplicationDbContext&gt;\(options =&gt;  
options.UseSqlServer\(Configuration.GetConnectionString\("DefaultConnection"\)\)\);

services.AddIdentity&lt;ApplicationUser, IdentityRole&gt;\(\)  
.AddEntityFrameworkStores&lt;ApplicationDbContext&gt;\(\)  
.AddDefaultTokenProviders\(\);

services.AddMvc\(\);

// Add application services.  
services.AddTransient&lt;IEmailSender, AuthMessageSender&gt;\(\);  
services.AddTransient&lt;ISmsSender, AuthMessageSender&gt;\(\);  
}

Conseil :

Une méthode comme services.AddMvc\(\) ajoute et éventuellement configure les services dont MVC a besoin.

Respecter la convention services.Add&lt;ServiceName&gt;, en plaçant des méthodes d’extensions dans l’espace de noms Microsoft.Extensions.DependencyInjection qui encapsulent des groupes d’inscriptions de services.

AddTransient est utilisé pour mapper un type abstrait \(une interface souvent\), et son service concret.

Exemple lorsque le service CharacterRepository dépend d’un dbContext ApplicationDbContext à injecter :

publicvoidConfigureServices\(IServiceCollection services\)  
{  
services.AddDbContext&lt;ApplicationDbContext&gt;\(options =&gt;  
options.UseInMemoryDatabase\(\)  
\);

// Add framework services.  
services.AddMvc\(\);

// Register application services.  
services.AddScoped&lt;ICharacterRepository, CharacterRepository&gt;\(\);  
services.AddTransient&lt;IOperationTransient, Operation&gt;\(\);  
services.AddScoped&lt;IOperationScoped, Operation&gt;\(\);  
services.AddSingleton&lt;IOperationSingleton, Operation&gt;\(\);  
services.AddSingleton&lt;IOperationSingletonInstance&gt;\(newOperation\(Guid.Empty\)\);  
services.AddTransient&lt;OperationService, OperationService&gt;\(\);  
}

On constate le AddScoped car une dépendance à EntityFramework doit avoir la même dépendance qu’EntityFramework qui est Scoped.

Un DbContext est un service Scoped, un Repository à qui on injecte un DbContext, doit également être injecté en service Scoped.

### Durée de vie du service

Transient : Des services à durée de vie temporaire sont créés CHAQUE FOIS qu’ils sont demandés. Parfait pour des services légers et sans état.

Scoped : Les services sont créés une seule fois par requête.

Singleton : Les services sont créés la première fois qu’ils sont demandés, ou lorsque ConfigureServices est exécuté, si c’est ici qu’on y spécifie une instance.

Le conseil : Si notre application nécessite un singleton, plutôt d’implémenter le modèle de conception Singleton, et gérer la durée de vie de l’objet nous même, il est conseillé de laisser le conteneur de service gérer la durée de vie du service.

On a vu que l’on peut inscrire une implémentation de service avec un type donné en spécifiant le type concret à utiliser.

On peut même spécifier une fabrique qui sera utilisée pour créer l’instance à la demande.

La 3éme méthode consiste à spécifier directement une instance du type à utiliser !

Auquel cas le conteneur n’essaie jamais de créer une instance \(ni d’en disposer\).

Exemple :

services.AddSingleton&lt;IOperationSingleton, Operation&gt;\(\);  
services.AddSingleton&lt;IOperationSingletonInstance&gt;\(newOperation\(Guid.Empty\)\);

### Conception des services

Il faut concervoir les services pour qu’ils utilisent l’injection de dépendances.

Il faut donc éviter d’utiliser des appels de méthode statique avec état qui entrainent un Code Smell appelé Static cling :[http://deviq.com/static-cling/](http://deviq.com/static-cling/)

Si une classe a tendance à avoir trop de dépendances injectées, c’est qu’elle viole probablement le principe de responsabilité unique.

Il vaut mieux essayer de refactoriser.

Pour rappel, les classes Controller doivent se concentrer sur les préocupations de l’UI.

Donc les Business Rules, et les détails de l’implémentation de l’accès aux données doit être encapsulé dans des classes appropriées à ces préocupations. C’est le Principe de Separation of concerns :

[http://deviq.com/separation-of-concerns/](http://deviq.com/separation-of-concerns/)

Exemple : Il est possible d’injecter DbContext directement dans le controller, mais il est conseillé d’injecter une une interface qui encapsule la logique d’accès aux données.

### Mise à disposition des services.

Le conteneur appelle Dispose pour les services Disposable, sauf si le contrôleur n’a pas eu à les instancier.

Exemple :

publicvoidConfigureServices\(IServiceCollection services\)  
{  
// container will create the instance\(s\) of these types and will dispose them  
services.AddScoped&lt;Service1&gt;\(\);  
services.AddSingleton&lt;Service2&gt;\(\);  
services.AddSingleton&lt;ISomeService&gt;\(sp =&gt;newSomeServiceImplementation\(\)\);

// container didn't create instance so it will NOT dispose it  
services.AddSingleton&lt;Service3&gt;\(newService3\(\)\);  
services.AddSingleton\(newService3\(\)\);  
}

### Remplacer le conteneur de service par défaut

Il est possible de remplacer le conteneur de services par défaut en modifiant la signature de ConfigureServices afin d’y return un IServiceProvider.

Possible d’utiliser Autofac par exemple.

### Multi threading

Evidemment, les services singleton doivent être Thread Safe. Si un service Singleton a une dépendance vis à vis d’un service temporaire, celui-ci doit aussi être Thread Safe.

### Recommandations

* Ce pattern est destiné aux objets qui ont des dépendances complexes comme les contrôleurs, les services, les adaptateurs et les référentiels.

* Évitez de stocker des données et des configurations directement dans l’injection de dépendances. Par exemple, le panier d’achat d’un utilisateur ne doit en général pas être ajouté au conteneur de services. La configuration doit utiliser le[modèle d’options](https://docs.microsoft.com/fr-fr/aspnet/core/fundamentals/configuration/options). De même, évitez les objets « conteneurs de données » qui n’existent que pour autoriser l’accès à un autre objet. Il est préférable de demander l’élément réel dont vous avez besoin par le biais de l’injection de dépendances, si possible. \(à clarifier !\)

* Éviter l’accès statique aux services.

* Eviter l’emplacement du service dans le code d’application \(?\)

* Éviter l’accès statique à HttpContext.



