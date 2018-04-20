# Startup en ASP .NET Core

Source :[https://docs.microsoft.com/en-US/aspnet/core/fundamentals/startup?view=aspnetcore-2.1](https://docs.microsoft.com/en-US/aspnet/core/fundamentals/startup?view=aspnetcore-2.1)

2 méthodes :

Configure =&gt; Obligatoire

ConfigureService =&gt; Optionnel

## Méthode ConfigureServices

* Optionnelle

* Appelée par le Web Host AVANT la méthode Configure

* Là ou les options de configurations et l’ajout de services à l’Inversion of Control Container est appliqué par défaut

Ajouter des services au Service Container ici les rend disponible au sein de l’application, mais aussi dans la méthode Configure.

Les services sont retrouvés via l’injection de dépendance, ou via IApplicationBuilder.ApplicationServices.

Pour les services qui requièrent un setup subséquent, une méthode Add\[Service\] existe.

Il est de bon usage d’ajouter des méthodes d’extensions sur IServiceCollection avec Add\[Service\] pour les services conséquents.

Exemple :

`public void ConfigureServices(IServiceCollection services)    
{    
// Add framework services.    
services.AddDbContext<ApplicationDbContext>(options =>    
options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));`

`services.AddIdentity<ApplicationUser, IdentityRole>()    
.AddEntityFrameworkStores<ApplicationDbContext>()    
.AddDefaultTokenProviders();`

`services.AddMvc();`

`// Add application services.    
services.AddTransient<IEmailSender, AuthMessageSender>();    
services.AddTransient<ISmsSender, AuthMessageSender>();    
}`

## Services disponibles pour Startup

Le WebHost fournit des services disponible pour le constructeur de Startup.

L’application peut en ajouter de nouveaux via ConfigureServices.

Les services rendus disponibles par le WebHost et par l’application sont disponibles dans la méthode Configure, mais aussi pour toute l’application.

## La méthode Configure

Elle est utilisée pour spécifier comment l’application répond aux requêtes HTTP.

Le template configure le pipeline http avec un support pour les excéptions en environnement de developpement, Browslink, pages d’erreurs, fichiers statiques, et ASP.NET MVC :

`public void Configure(IApplicationBuilder app, IHostingEnvironment env)    
{    
if(env.IsDevelopment())    
{    
app.UseDeveloperExceptionPage();    
app.UseBrowserLink();    
}    
else    
{    
app.UseExceptionHandler("/Error");    
}`

`app.UseStaticFiles();`

`app.UseMvc(routes =>    
{    
routes.MapRoute(    
name:"default",    
template:"{controller}/{action=Index}/{id?}");    
});    
}`

La pipeline de request est configurée en ajoutant des composants midleware à une instance de IApplicationBuilder.

IApplicationBuilder est disponible pour la méthode Configure, mais n’est pas enregistré dans le conteneur de Service. Weh Host crée un IapplicationBuilder et le passe directement dans le Configure.

Chaque utilisation de IapplicationBuilder.Use&lt;Middleware&gt; ajoute un composant Middleware pour la pipeline de request.

Par exemple, la méthode d’extension UseMvc ajoute le middleware de routing à la pipeline de request et configure MVC comme handler par défaut.

Chaque composant Middleware dans la pipeline de requête est responsable d’invoquer le prochain composant dans la pipeline ou de court-circuiter la chaîne, si approprié. Si il n’y a pas de court circuit pendant la chaine de middleware, chaque middleware a une seconde chance de manipuler la requête avant de l’envoyer au client.

Des services additionnels comme IHostingEnvironment ou ILoggerFactory peuvent également être spécifié dans la signature de la méthode.

Lorsqu’ils sont présents, des services additionnels sont injectés s’ils sont disponibles.

Le chapitre sur les MiddleWare permettra d’éclaircir ces notions.

Source :[https://docs.microsoft.com/en-US/aspnet/core/fundamentals/middleware/index?view=aspnetcore-2.1&tabs=aspnetcore2x](https://docs.microsoft.com/en-US/aspnet/core/fundamentals/middleware/index?view=aspnetcore-2.1&tabs=aspnetcore2x)

## Convenience Method

Plutôt que de spécifier une classe Startup, il est possible d’utiliser des Convenience Method de ConfigureServices et Configure.

Exemple dans class Program :

`public class Program`

`{`

`public static IHostingEnvironment HostingEnvironment { get; set; }`

`public static IConfiguration Configuration { get; set; }`

`public static void Main(string[] args)`

`{`

`BuildWebHost(args).Run();`

`}`

`public static IWebHost BuildWebHost(string[] args) =>`

`WebHost.CreateDefaultBuilder(args)`

`.ConfigureAppConfiguration((hostingContext, config) =>`

`{`

`HostingEnvironment = hostingContext.HostingEnvironment;`

`Configuration = config.Build();`

`})`

`.ConfigureServices(services =>`

`{`

`services.AddMvc();`

`})`

`.Configure(app =>`

`{`

`if (HostingEnvironment.IsDevelopment())`

`{`

`app.UseDeveloperExceptionPage();`

`}`

`else`

`{`

`app.UseExceptionHandler("/Error");`

`}`

`// Configuration is available during startup. Examples:`

`// Configuration["key"]`

`// Configuration["subsection:suboption1"]`

`app.UseMvcWithDefaultRoute();`

`app.UseStaticFiles();`

`})`

`.Build();`

`}`

## Filtres Startup

On peut utiliser IStartupFilter pour configurer un middleware au début ou à la fin d’une pipeline middleware de la méthode Configure.

IStarterFiler est utile pour s’assurer qu’un middleware va tourner avant ou après un autre middleware ajouté par library au début ou à la fin d’une pipeline de requête de l’application.

Application Sample qui met en oeuvre l’utilisation de IStartupFilter :

[https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/)

