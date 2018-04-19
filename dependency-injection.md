# Dependency Injection

## Introduction au pattern

Source :[https://msdn.microsoft.com/en-us/library/hh323705\(v=vs.100\).aspx](https://msdn.microsoft.com/en-us/library/hh323705%28v=vs.100%29.aspx)



public class Class1  
{  
public readonly IClass2 \_class2;  
  
public Class1\(\):this\(DependencyFactory.Resolve&lt;IClass2&gt;\(\)\)  
{  
  
}  
  
public Class1\(IClass2 class2\)  
{  
\_class2 = class2;  
}  
}

  


De cette manière, Class1 et Class2 sont loosely coupled. et non tight coupled.

  


Intérêt :

Test unitaire : possibilité de mocker Iclass2 pour réellement tester un comportement de Class1.

  


Titre 2 : L’injection de dépendances en .NET, vu de 2009.

  


Source :[https://philippe.developpez.com/articles/dotnet/injectiondedependances/](https://philippe.developpez.com/articles/dotnet/injectiondedependances/)

L’intérêt de cet article, est de comprendre plus en profondeur l’injection de dépendance en .NET et le pattern associé.

Dans l’article sont présentés des outils pour le mettre en place.

Nous découvrirons cela au fur et à mesure de la lecture, mais il semblerait que sur un projet .NET Core par exemple, l’injection de dépendance est déjà “outillée” par défaut ;\). Mais cela peut être intéressant, pour apprécier et comprendre ce qui existe aujourd’hui, de voir ce qui était nécessaire hier.

  


Titre 3 : Clarification du vocabulaire

Inversion of Control \(IoC\) : Lorsqu’un module effectue un traitement, le contrôle du traitement est déporté vers l’appelé, et non vers l’appelant. En pratique, on va chercher à diminuer au maximum la connaissance qu’a l’appelant de la mécanique interne de l’appelé.

  


Dependency inversion principle \(DIP\) : Principe de développement qui stipule qu’on limite le couplage entre les classes.

  


Dependency injection \(DI\) : Pattern permettant de mettre en place l’inversion de dépendance.

  


Une utilisation que l’on rencontre régulièrement de l’injection de dépendances est l’injection d’un Mock ou d’un Stub dans le cas de tests unitaires en isolation.

  


Titre 3 : L’injection de dépendance par le constructeur

  


publicclassCustomerService{  
privateICustomerRepository \_repository;  
  
publicCustomerService\(ICustomerRepository repository\){  
\_repository=repository;  
}  
  
publicCustomerGetOneCustomer\(intcustomerId\){  
returnnewCustomer\(\_repository.GetOneById\(customerId\)\);  
}  
}

  


Titre 3 : Cas de deux dépendances à injecter : Une des deux dépendance est accessible par propriété :

  


publicclassPortfolioService{  
  
privateISecurityService \_securityService;  
privateIPortfolioRepository \_portfolioRepository;  
  
publicISecurityService SecurityService{  
set{  
\_securityService=values;  
}  
}  
  
publicIPortfolioRepository PortfolioRepository{  
set{  
\_portfolioRepository=values;  
}  
}  
  
publicboolSavePortfolio\(Portfolio pf,UserInfo user\){  
if\(\_securityService.IsPortfolioWriteableByUser\(user\)\){  
\_portfolioRepository.Save\(pf\);  
}  
}  
}

  


L’avantage de l’injection de dépendance par le constructeur est de s’assurer que l’appelant va bien fournir la dépendance.

Lorsque la dépendance est accessible par propriété, et que l’appelant oublie de fournir la dépendance, on arrive sur une nullref …

  


Jusqu’ici, l’appelant est résponsable de fournir la dépendance.

Pour palier à ce problème, il existe des conteneurs IoC.

  


Titre 3 : Conteneur d’inversion de contrôle

  


Les conteneurs d’inversion de contrôle sont des outils conçus pour faciliter l’injection de dépendances.

  


Constatation du problème sur l’appelant Form1 sans Conteneur IoC :

  


publicclassForm1{  
  
publicvoidLoadCustomerData\(intcustomerId\){  
ICustomerRepository repository=newCustomerRepository\(\);  
ICustomerService service=newCustomerService\(repository\);  
Customer monClient=service.GetOneCustomer\(1\);  
Messagebox.Show\(monClient.Name\);  
}  
  
}  
  
publicinterfaceICustomerService{  
  
publicDataTableGetOneCustomer\(intcustomerId\){  
...  
}  
}  
publicclassCustomerService{  
privateICustomerRepository \_repository;  
  
publicCustomerService\(ICustomerRepository repository\){  
\_repository=repository;  
}  
  
publicCustomerGetOneCustomer\(intcustomerId\){  
returnnewCustomer\(\_repository.GetOneById\(customerId\)\);  
}  
}  
  
publicinterfaceICustomerRepository{  
  
publicDataTableGetOneById\(intcustomerId\){  
...  
}  
}  
  
publicclassCustomerRepository{  
  
publicDataTableGetOneCustomer\(intcustomerId\){  
...  
}  
}

  


On constate que Form1 \(appelant\) instancie un ICustomerRepository = new CustomerRepository \(couplage élevé\)

puis il instancie un ICustomerService en lui injectant le repository,

puis il fait enfin son appel à

monClient = service.GetOneCustomer\(1\);

  


Voyons différents Framework d’Inversion of Control qui existent en .NET

  


Titre 4 : Spring.NET

Le fichier de config \(Web.config ou App.Config\) dit à Spring :

Pour avoir un ProductService il faut un ProductRepository.

\(j’aurais aprécié aller un peu plus loin et dire que pour avoir un Form1 il faut un ProductService !\)

Mais sinon,

Pour obtenir un service, il n’y a plus qu’à appeler ce code :

  


IApplicationContext ctx=ContextRegistry.GetContext\(\);  
varmonClient=\(\(IProductService\)ctx.GetObject\("ProductService"\)\).  
GetOneProductById\(1\);

  


Context.GetObject\(“ProductService”\)

Contexte qui vient donc de ce fichier de config.

  


Titre 4 : Unity

Idem bien que plus verbeux dans le ficheir de config, il est également possible d’utiliser un RegisterType pour la configuration :



IUnityContainer container=newUnityContainer\(\);  
container.RegisterType&lt;IProductRepository,ProductRepository&gt;\(\)  
.RegisterType&lt;IProductService,ProductService&gt;\(\);

  


l’appel en suite sera :

varmonClient=container.Resolve&lt;IProductService&gt;\(\).  
GetOneProductById\(1\);

  


Titre 4 : Ninject

  


A une philosophie différente des frameworks précédents. Là ou les autres frameworks appellent un objet de contexte, Ninject travaille à partir d’un objet de type Kernel, dans le quel on charge des modules, qui eux, ont la résponsabilité de contenir les informations de résolution de type.

  


Pour notre exemple :

  


classProductModule:StandardModule{  
publicoverridevoidLoad\(\){  
Bind&lt;IProductService&gt;\(\).To&lt;ProductService&gt;\(\);  
Bind&lt;IProductRepository&gt;\(\).To&lt;ProductRepository&gt;\(\);  
}  
}

  


à l’utilisation :

IKernel kernel=newStandardKernel\(newProductModule\(\)\);  
  
varmonClient=kernel.Get&lt;IProductService&gt;\(\).  
GetOneProductById\(1\);

  


Dans le cas d’une configuration simple comme ici, on peut se passer de la création d’un module en utilisant l’objet InlineModule instancié avec des expressions lambda :

  


IKernel kernel=newStandardKernel\(newInlineModule\(  
module=&gt;module.Bind&lt;IProductService&gt;\(\).To&lt;ProductService&gt;\(\),  
module=&gt;module.Bind&lt;IProductRepository&gt;\(\).To&lt;ProductRepository&gt;\(\)  
\)  
\);

  


Intéressant car on n’a pas de configuration XML ici, on se rapproche de ce qu’on a vu en .NET Core.

  


Titre 4 : StructureMap

  


Supporte XML et déclaration avec expressions lambda.

  


Titre 4 : CommonServiceLocator

  


Permet d’utiliser une interface plus haut niveau, et d’agir comme un pattern Adapter pour rerouter les appels au Service Locator vers le framework d’IoC choisi.

Et ce afin de diminuer le couplage au … Framework d’IoC !

  


## L’injection de dépendances dans un projet ASP .NET Core {#net-core}

  


Source :[https://docs.microsoft.com/fr-fr/aspnet/core/fundamentals/dependency-injection](https://docs.microsoft.com/fr-fr/aspnet/core/fundamentals/dependency-injection)

Exemple de code très parlant :

Un Repository, un Service, un Controller, tous 3 construits par injection de dépendance !

[https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)

  


Avec injection de dépendance multiple dans un unique constructeur \(contrairement à la vision de 2009 ci-dessous ou on envisageait d’exposer une propriété publique \(!\)

  


La configuration se fait dans la classe Startup :

  


  


| publicclassStartup |   |
| :--- | :--- |
|   | { |
|   | // This method gets called by the runtime. Use this method to add services to the container. |
|   | publicvoidConfigureServices\(IServiceCollectionservices\) |
|   | { |
|   | services.AddDbContext&lt;ApplicationDbContext&gt;\(options =&gt; |
|   | options.UseInMemoryDatabase\(\) |
|   | \); |
|   |    |
|   | // Add framework services. |
|   | services.AddMvc\(\); |
|   |    |
|   | // Register application services. |
|   | services.AddScoped&lt;ICharacterRepository, CharacterRepository&gt;\(\); |
|   | services.AddTransient&lt;IOperationTransient, Operation&gt;\(\); |
|   | services.AddScoped&lt;IOperationScoped, Operation&gt;\(\); |
|   | services.AddSingleton&lt;IOperationSingleton, Operation&gt;\(\); |
|   | services.AddSingleton&lt;IOperationSingletonInstance&gt;\(newOperation\(Guid.Empty\)\); |
|   | services.AddTransient&lt;OperationService, OperationService&gt;\(\); |
|   | } |
|   |    |
|   | // This method gets called by the runtime. Use this method to configure the HTTP request pipeline. |
|   | publicvoidConfigure\(IApplicationBuilderapp,IHostingEnvironmentenv\) |
|   | { |
|   | if\(env.IsDevelopment\(\)\) |
|   | { |
|   | app.UseDeveloperExceptionPage\(\); |
|   | } |
|   |    |
|   | app.UseStaticFiles\(\); |
|   |    |
|   | app.UseMvcWithDefaultRoute\(\); |
|   | } |

  
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



