Titre 1 : Startup en ASP .NET Core

  


Source :[https://docs.microsoft.com/en-US/aspnet/core/fundamentals/startup?view=aspnetcore-2.1](https://docs.microsoft.com/en-US/aspnet/core/fundamentals/startup?view=aspnetcore-2.1)

  


  


2 méthodes :

Configure =&gt; Obligatoire

ConfigureService =&gt; Optionnel

  


Titre 2 : Méthode ConfigureServices

  


* Optionnelle

* Appelée par le Web Host AVANT la méthode Configure

* Là ou les options de configurations et l’ajout de services à l’Inversion of Control Container est appliqué par défaut

  


Ajouter des services au Service Container ici les rend disponible au sein de l’application, mais aussi dans la méthode Configure.

  


Les services sont retrouvés via l’injection de dépendance, ou via IApplicationBuilder.ApplicationServices.

  


Pour les services qui requièrent un setup subséquent, une méthode Add\[Service\] existe.

  


Il est de bon usage d’ajouter des méthodes d’extensions sur IServiceCollection avec Add\[Service\] pour les services conséquents.

  


Exemple :

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

  


Titre 2 : Services disponibles pour Startup

  


Le WebHost fournit des services disponible pour le constructeur de Startup.

L’application peut en ajouter de nouveaux via ConfigureServices.

Les services rendus disponibles par le WebHost et par l’application sont disponibles dans la méthode Configure, mais aussi pour toute l’application.

  


Titre 2 : La méthode Configure

  


Elle est utilisée pour spécifier comment l’application répond aux requêtes HTTP.

  


Le template configure le pipeline http avec un support pour les excéptions en environnement de developpement, Browslink, pages d’erreurs, fichiers statiques, et ASP.NET MVC :

  


publicvoidConfigure\(IApplicationBuilder app, IHostingEnvironment env\)  
{  
if\(env.IsDevelopment\(\)\)  
{  
app.UseDeveloperExceptionPage\(\);  
app.UseBrowserLink\(\);  
}  
else  
{  
app.UseExceptionHandler\("/Error"\);  
}  
  
app.UseStaticFiles\(\);  
  
app.UseMvc\(routes =&gt;  
{  
routes.MapRoute\(  
name:"default",  
template:"{controller}/{action=Index}/{id?}"\);  
}\);  
}

  


La pipeline de request est configurée en ajoutant des composants midleware à une instance de IApplicationBuilder.

IApplicationBuilder est disponible pour la méthode Configure, mais n’est pas enregistré dans le conteneur de Service. Weh Host crée un IapplicationBuilder et le passe directement dans le Configure.

  


// TODO : Continuer[https://docs.microsoft.com/en-US/aspnet/core/fundamentals/startup?view=aspnetcore-2.1](https://docs.microsoft.com/en-US/aspnet/core/fundamentals/startup?view=aspnetcore-2.1)

  


