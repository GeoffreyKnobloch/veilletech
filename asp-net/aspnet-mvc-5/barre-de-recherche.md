# Barre de recherche

Source : [https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-search](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-search)

Déjà, en uploadant l’action Index en ajoutant un paramètre searchString comme ceci :

```
public ActionResult Index(string searchString)
{ 
var movies = from m in db.Movies
select m;

if (!String.IsNullOrEmpty(searchString))
{
movies = movies.Where(s => s.Title.Contains(searchString));
}

return View(movies);
}
```

Ducoup, avec l’url serveur/movies?searchString=ghost, filtrera sur les films contenant “ghost”.

Remarquons, rappelons, que si on change le paramètre de l’action Index en le nommant Id, étant donné que la route est définit comme suit :

{controller}/{action}/{id},

une url serveur/movies/ghost en GET permettrait de filtrer.

Le bouton permettant de Get l’Index avec searchString valorisé en query string dans le @using :

```
@model IEnumerable<MvcMovie.Models.Movie>

@{
ViewBag.Title = "Index";
}

<h2>Index</h2>

<p>
@Html.ActionLink("Create New", "Create")

@using (Html.BeginForm()){ 
<p> Title: @Html.TextBox("SearchString") <br />
<input type="submit" value="Filter" /></p>
}
</p>
```

Html.BeginForm ⇒ &lt;form&gt;

Par défaut et en l’état, cette form va Post sur /Index \(itself\) avec en body SearchString prenant la valeur de la TextBox.

Donc l’URL résultante d’un click sur le bouton sera Serveur/Index.

Et le param de recherche est contenue dans le body de la requête POST.

Or, on serait plus cohérent du GET \(on ne modifie pas de valeur\), et on ne peut pas copier coller l’URL pour partager la recherche filtrée.

Il faudrait donc créer une méthode Index décorée de \[HttpPost\] :

```
[HttpPost]
public string Index(FormCollection fc, string searchString)
{
return "<h3> From [HttpPost]Index: " + searchString + "</h3>";
}
```

Pour ce problème d’URL et de cohérence sur le fait qu’on obtient de la DATA, et qu’on n’apporte pas de modification,

mieux vaut modifier la &lt;form&gt; afin de générer une requête GET :

```
@using (Html.BeginForm(“Index”, “Movies”, FormMethod.Get))
{
<p> Title: @Html.TextBox("SearchString") <br />
<input type="submit" value="Filter" /></p>
}
```

Voilà qui est parfait :

On est explicite sur l’action et le controller, sur le type de requête, c’est facile à comprendre.

Le résultat est comme prévu :

![](https://lh3.googleusercontent.com/kcjxNlCPbQj2ylEIH25pyd5ppWAyhcHuoG9GoQS75nPzOxCIeBcUiKshxvkF1Gg5Np5twZ7t9P0zlWuUpwwdssdUb0w6JLtxIbYDmMuc1wKuap3EoWVL2_w_hPi4YSyHpLrR16LZ)

## Lookup

Méthode Index :

```
public ActionResult Index(string movieGenre, string searchString)
{
var GenreLst = new List<string>();
var GenreQry = from d in db.Movies
orderby d.Genre
select d.Genre;
GenreLst.AddRange(GenreQry.Distinct());
ViewBag.movieGenre = new SelectList(GenreLst);
var movies = from m in db.Movies
select m;
if (!String.IsNullOrEmpty(searchString))
{
movies = movies.Where(s => s.Title.Contains(searchString));
}
if (!string.IsNullOrEmpty(movieGenre))
{
movies = movies.Where(x => x.Genre == movieGenre);
}
return View(movies);
}
```

On utilise ici le ViewBag :

ViewBag.movieGenre = new SelectList\(GenreLst\);

On aurait pu envisager également d’utiliser un ViewModel qui contient List&lt;Movie&gt; ainsi que List&lt;string&gt; pour les genres.

C’est un choix. C’est vrai que là l’utilisation du ViewBag pour une lookup de filtre semble cohérent.

Puis dans la View :

```
@model IEnumerable<MvcMovie.Models.Movie>
@{
ViewBag.Title = "Index";
}
<h2>Index</h2>
<p>
@Html.ActionLink("Create New", "Create")
@using (Html.BeginForm("Index", "Movies", FormMethod.Get))
{
<p>
Genre: @Html.DropDownList("movieGenre", "All")
Title: @Html.TextBox("SearchString")
<input type="submit" value="Filter" />
</p>
}
</p>
<table class="table">
```

Genre: @Html.DropDownList\("movieGenre", "All"\)

⇒ Utilisation de la propriété movieGenre du ViewBag.

Par défaut, vaut All, qui a pour value string.Empty, et donc pas de filtre.

Va valoriser ?movieGenre=value

Donc visiblement pour cette méthode, il semble que la propriété movieGenre du ViewBag et le paramètre movieGenre de l’action doivent être nommé de la même façon.

