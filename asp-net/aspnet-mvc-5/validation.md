# Validation

Source :[https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-validation](https://docs.microsoft.com/en-US/aspnet/mvc/overview/getting-started/introduction/adding-validation)

## DRY

DRY : Don’t repeat yourself

ASP.NET MVC nous encourage à spécifier un comportement une seule et unique fois.

## Les rules de validation dans le model

StringLength : Taille du string

DisplayName : Nom affiché via les @html helper

DataType : Type de Data pour aider les @html helper

DisplayFormat : Pour la date nottament

RegularExpression : Expression rég

Required : Requis

Range : Valeur min et max

Exemple :

```
public class Movie
{
public int ID { get; set; }


[StringLength(60, MinimumLength = 3)]
public string Title { get; set; }


[Display(Name = "Release Date")]
[DataType(DataType.Date)]
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }

[RegularExpression(@"^[A-Z]+[a-zA-Z'\s]*$")]
[Required]
[StringLength(30)]
public string Genre { get; set; }


[Range(1, 100)]
[DataType(DataType.Currency)]
public decimal Price { get; set; }


[RegularExpression(@"^[A-Z]+[a-zA-Z'\s]*$")]
[StringLength(5)]
public string Rating { get; set; }
}
```

Ces attributs ont un impact en terme d’affichage, en terme de validation, et en terme de DataBase.

Côté Client :

Validation avec JavaScript et jQuery, encapsulé par les helper @html.

Côté Serveur \(si un utilisateur a JavaScript disabled\) :

DataBase protégée par les rules de validation \(non nullable, maxlength, … \)

## DataBase

Pour assurer les changements en DB, une migration s’impose :

```
add-migration DataAnnotations
update-database
```

## UI

Lors d’un ajout ou modification, la validation jQuery côté Client détecte les erreurs et afficht un message d’erreur.

## Controller

le if \(ModelState.IsValid\) assure la validation.

## L’attribut DataType

Exemple :

```
[DataType(DataType.Date)]
public DateTime ReleaseDate { get; set; }

[DataType(DataType.Currency)]
public decimal Price { get; set; }

```

L’attribut DataType fournit au @html helper des indices sur comment formater la donnée.

Ajout d’un &lt;a&gt; pour une url,

Ajout d’un mailto: pour un email,

Un sélectionneur de date pour une Date,

L’attribut DataType ne fournit aucune forme de validation.

Pour expliciter un format de date :

```
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime EnrollmentDate { get; set; }
```



