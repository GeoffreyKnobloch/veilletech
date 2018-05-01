Titre 2 : Utilisation de la migration lors de changements du model

  


Source :[https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-new-field](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-a-new-field)

Titre 3 : Mise en place de la Migration Code First

  


D’abord : Supprimer la bdd. \(fichier .mdf généré par Entity Framework\).

En suite, ouvrir la Package Manager Console :

Tools/NuGet Package Manager/Package Manager Console

  


Puis entrer la commande :

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

  


Cela va créer un fichier Configuration.cs dans le dossier Migrations

  


Dans ce fichier il y a une méthode Seed, qu’on peut remplacer par w/e est prévu dans le projet pour peupler la DataBase.

  


La migration Code First appelle la méthode Seed après chaque migration.

  


Pour éviter tous problèmes, il est préférable d’utiliser la méthode AddOrUpdate pour peupler la DB dans Seed :

  


protected override void Seed\(MvcMovie.Models.MovieDBContext context\)

{

context.Movies.AddOrUpdate\( i =&gt; i.Title,

new Movie

{

Title = "When Harry Met Sally",

ReleaseDate = DateTime.Parse\("1989-1-11"\),

Genre = "Romantic Comedy",

Price = 7.99M

},

  


new Movie

{

Title = "Ghostbusters ",

ReleaseDate = DateTime.Parse\("1984-3-13"\),

Genre = "Comedy",

Price = 8.99M

},

  


new Movie

{

Title = "Ghostbusters 2",

ReleaseDate = DateTime.Parse\("1986-2-23"\),

Genre = "Comedy",

Price = 9.99M

},

  


new Movie

{

Title = "Rio Bravo",

ReleaseDate = DateTime.Parse\("1959-4-15"\),

Genre = "Western",

Price = 3.99M

}

\);



}

  


Une migration : Appel à update-database dans le package Manager Console.

  


La méthode doit update les rows s’ils sont déjà insérés. ou les insérer s’ils n’existent pas déjà.

C’est pourquoi on utilise AddOrUpdate afin de faire un “upsert”.

  


AddOrUpdate semble n’être adapté qu’en cas de Seed dans le cadre de la migration justement.

Source :[http://thedatafarm.com/data-access/take-care-with-ef-4-3-addorupdate-method/](http://thedatafarm.com/data-access/take-care-with-ef-4-3-addorupdate-method/)

  


En suite, il faut build le projet \(CTRL SHIFT B\).

  


En suite, il faut créer une classe DbMigration. Celle-ci va créer une nouvelle DataBase. C’est pourquoi nous avons supprimé la base \(\*.mdf\).

  


Dans le Package Manager Console, taper la commande :

add-migration Initial

\(Le mot Initial est arbitraire, il est utilisé pour nomer le fichier de migration ainsi créé\).

  


Code First Migrations crée alors une nouvelle classe dans le dossier Migrations, avec le nom {Date}\_Initial.cs

Cette classe contient le code qui crée le schéma de database.

  


Dans le package Manager Console, taper :

update-database

  


Cela a pour conséquence de run le fichier {Date}\_Initial pour créer la DataBase, ainsi que la méthode Seed pour là peupler.

  


Ces simples étapes ont permis de se préparer à la migration en cas de changement dans le model :

1. Supprimer la base \(\*.mdf\)

2. Ouvrir Package Manager Console \(Tools/NuGet Package Manager/Package Manager Console\)

3. Commande : Enable-Migrations -ContextTypename MvcMovie.Models.MovieDBContext

4. Réécrire la méthode Seed du fichier Configuration.cs du dossier Migrations

5. Build l’application \(Ctrl + Shift + B\)

6. Commande : add-migration Initial

7. Commande : update-database

  


Titre 3 : S’adapter à un changement du model

  


Ajouter la propriété sur le model

Build la solution

  


Adapter l’attribut Bind pour Create et Edit du controller correspondant :

\[Bind\(Include = "ID,Title,ReleaseDate,Genre,Price,Rating"\)\]

  


Dans Index.cshtml, ajouter l’affichage de la propriété

Dans Create.cshtml, ajouter l’édition pour cette propriété

  


Si on run l’application à ce stade, la DataBase n’a pas évolué, le model ne correspond plus à la DB et une erreur va se produire pour cette raison.

  


Pour faire évoluer la DB, plusieurs approches sont possibles :

1. Faire en sorte qu’entity Framework drop et recrée automatiquement la DB, puis là seed de données de test. ⇒ Ceci est une façon productive de développer, mais on perd les données donc ce n’est pas une option en production. Voir[https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)

2. Modifier soi même le schema de la database existante pour matcher avec les nouvelles données. Soit manuellement, soit en écrivant un script de changement de database. L’avantage étant qu’on conserve les données si besoin.

3. Utiliser Code First Migrations pour mettre à jour le schema de Database.

  


  


Nous allons utiliser Code FIrst Migrations.

  


D’abord, on met à jour la méthode de Seed pour y rajouter le champ manquant pour chaque objets :

new Movie

{

Title = "When Harry Met Sally",

ReleaseDate = DateTime.Parse\("1989-1-11"\),

Genre = "Romantic Comedy",

Rating = "PG",

Price = 7.99M

},

  


Build la solution,

Ouvrir le Package Manager Console, commande : add-migration Rating

Cela aura pour conséquence de créer la classe DbMigration AddRatingMig

Build la solution,

Commande : update-database

La database est updaté, le nouveau champ est ajouté.

Voir[https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)pour plus d’information sur EF.

  


