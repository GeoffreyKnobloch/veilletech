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

## L’injection de dépendances en .NET, vu de 2009.

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

Voir l'article : [https://geoffreyknobloch.gitbooks.io/veilletechno/content/dependency-injection-net-core.html](https://geoffreyknobloch.gitbooks.io/veilletechno/content/dependency-injection-net-core.html)

