# ASP.NET MVC 5

## Reprise des bases du MVC

Source :[https://openclassrooms.com/courses/apprendre-asp-net-mvc](https://openclassrooms.com/courses/apprendre-asp-net-mvc)

Pour comprendre la philosophie d’une application MVC sans aborder de notions avancées, ce cours de 30 heures est convenable. Ce cours est parfaitement adapté pour des gens qui n’ont aucune notion d’ASP \(ou même aucune notion de programmation\).

### 

### 

### 

## Accéder au Model depuis le Controller

Source : [https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller)

Lors de l’ajout d’un Controller, il y a plusieurs choix de template.

Un template intéressant est MVC 5 Controller with views using Entity Framework.

ça fait tout de suite un CRUD.

![](https://lh4.googleusercontent.com/Q5mO6cd6HoZf-CbFwePTYJRWObECloPASGrf1pSufCwfTpScyPx2NV3M-m3zK16qD1MczWACDL7pFQ5B6DBneHCeBg4nwzymYG15tGD-Oe97c31iQYa12_flVEI775SJ3f8YYTvw)

Il suffit alors de sélectionner l’entity et le DbContext qui porte le DbSet de l’entity.

![](https://lh6.googleusercontent.com/pVULsz0PMi3ZuHIsV1eXMtIkQjsUnXLJts7QeqTzcleq1ouwngqLJAkSvr1roN4AfBM-oj_BBGJzF21mV9cXyEvNtQvEcOKl4a50TfYGBAzY_5eha0vxmgpLwAnW1n5jUdQG3fYA)

Visual Studio crée automatiquement un CRUD sur l’entity en question :

Un Controller, un dossier Views/Movies

Les views : create, delete, details, edit, index.

Pratique pour avoir une base, puis personnaliser ensuite.

Personellement, je recommande de remplacer le DbContext du controller par une classe Service à injecter par injection de dépendance.

Ce Service va appeler un Data Acces Object, pour obtenir des Business Object, puis les traduire en DTO \(Data Transfer Object\) qui correspondent exactement à ce que la View a besoin.

Pour plus d’informations sur EntityFramework :

[https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)

Voici ce qui a été créé pour l’Edit dans le controller :

```
// GET: /Movies/Edit/5
public ActionResult Edit(int? id)
    {
        if (id == null)
        {
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
        }
        Movie movie = db.Movies.Find(id);
        if (movie == null)
        {
        return HttpNotFound();
        }
        return View(movie);
    }
// POST: /Movies/Edit/5
// To protect from overposting attacks, please enable the specific properties you want to bind to, for
// more details see 
http://go.microsoft.com/fwlink/?LinkId=317598
.
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Edit([Bind(Include="ID,Title,ReleaseDate,Genre,Price")] Movie movie)
    {
        if (ModelState.IsValid)
        {
        db.Entry(movie).State = EntityState.Modified;
        db.SaveChanges();
        return RedirectToAction("Index");
        }
        return View(movie);
    }
```

2 actions Edit, un pour GET, un pour POST ! \(Ce petit problème n’est plus présent quand on utilise Razor Page.\)

On constate l’attribut Bind : Afin de ne recevoir QUE les propriétées désirée lors d’un POST.

On constate l’attribut ValidateAntiForgeryToken : Afin de se protéger contre les attaques cross-site Forgery.

Cet attribut va de paire avec un appel à @Html.AntiForgeryToken\(\) dans la View :

```
@model MvcMovie.Models.Movie
@{
ViewBag.Title = "Edit";
}
<h2>Edit</h2>
@using (Html.BeginForm())
{
    @Html.AntiForgeryToken()
        <div class="form-horizontal">
            <h4>Movie</h4>
            <hr />
            @Html.ValidationSummary(true)
            @Html.HiddenFor(model => model.ID)
                <div class="form-group">
                @Html.LabelFor(model => model.Title, new { @class = "control-label col-md-2" })
                <div class="col-md-10">
            @Html.EditorFor(model => model.Title)
            @Html.ValidationMessageFor(model => model.Title)
        </div>
    </div>
}
```

Plus d’infos sur cette attaque :[https://docs.microsoft.com/en-US/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages](https://docs.microsoft.com/en-US/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

Lors du Post, le model binder d’ASP.NET prend les valeurs du du POST et en crée un objet Movie dans le paramètre movie.

## Ajouter une barre de recherche

Source : [https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-search](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-search)

