# Le Model

Source : [https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-model](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-model)

## Code first avec Entity Framework

Entité POCO :

```
using System;
namespace MvcMovie.Models
{
public class Movie
{
public int ID { get; set; }
public string Title { get; set; }
public DateTime ReleaseDate { get; set; }
public string Genre { get; set; }
public decimal Price { get; set; }
}
}
```

Avec son DbContext :

```
using System;
using System.Data.Entity;
namespace MvcMovie.Models
{
public class MovieDBContext : DbContext
{
public DbSet<Movie> Movies { get; set; }
}
}
```

MovieDBContext représente le contexte Database de Movie qui prend en charge tout : Select, modifier, supression, …

## DataBase first

Source : [https://docs.microsoft.com/en-US/aspnet/visual-studio/overview/2013/aspnet-scaffolding-overview](https://docs.microsoft.com/en-US/aspnet/visual-studio/overview/2013/aspnet-scaffolding-overview)

Il est possible de générer du code pour une app MVC ou une Web API à partir de la base de donnée.

## Créer une Connection String et travailler avec SQL Server LocalDB

LocalDB est une version légère de SQL Server Express Database Engine qui démarre sur demande, et tourne en mode utilisateur.

LocalDB tourne tourne dans un mode d’exécution spécial de SQL Server Express qui nous permet de travailler avec une database en fichiers .mdf

Typiquement, les fichiers d’une database LocalDB sont dans le fichier App\_Data du projet Web.

LocalDB n’est pas fait pour être déployé en production car ce n’est pas designé pour fonctionner avec IIS.

En revanche, une database LocalDB peut facilement être migrée sur SQL Server ou SQL Azure.

Donc on a le schémas : Code first -&gt; Génére LocalDB -&gt; Migre sur SQL Server

Plus d’informations sur les connexions String :

[https://msdn.microsoft.com/library/jj653752.aspx](https://msdn.microsoft.com/library/jj653752.aspx)

Par défaut, Entity Framework se base sur une connexion string \(créée par défaut\) dans Web.config \(pas Web.config dans le dossier View\) :

Web.config :

```
<connectionStrings>
<add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;Initial Catalog=aspnet-MvcMovie-fefdc1f0-bd81-4ce9-b712-93a062e01031;Integrated Security=SSPI;AttachDBFilename=|DataDirectory|\aspnet-MvcMovie-fefdc1f0-bd81-4ce9-b712-93a062e01031.mdf" providerName="System.Data.SqlClient" />
<add name="MovieDBContext" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;Initial Catalog=aspnet-MvcMovie;Integrated Security=SSPI;AttachDBFilename=|DataDirectory|\Movies.mdf" providerName="System.Data.SqlClient" />
</connectionStrings>
```

Il y a par défaut une DefaultConnexion, qui est utilisée pour le membership database, pour controler qui peut accéder à l’application.

Pour en savoir plus sur la database de membership, voir :

[https://docs.microsoft.com/fr-fr/aspnet/core/security/authorization/secure-data](https://docs.microsoft.com/fr-fr/aspnet/core/security/authorization/secure-data)

Il faut rajouter une seconde connexion string, ici MovieDbContext \(qui match la classe MovieDBContext\), afin de maitriser par exemple le fichier qui servira de DB :

AttachDBFilename=\|DataDirectory\|\Movies.mdf"

On remarquera néanmoins que la création de la connexionString MovieDBContext n’était même pas obligatoire, car si on ne spécifie pas de Connexion String, Entity Framework va créer automatiquement une Database LocalDB dans le dossier de l’utilisateur, avec le nom totalement qualifié du DbContext \(ici MvcMovie.Models.MovieDBContext\).

