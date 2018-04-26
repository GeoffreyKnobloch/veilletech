# Blazor

  


Source :[https://learn-blazor.com/](https://learn-blazor.com/)

  


Titre 2 : Qu’est-ce que Blazor

Source :[https://learn-blazor.com/getting-started/what-is-blazor/](https://learn-blazor.com/getting-started/what-is-blazor/)

  


Titre 3 : WebAssembly

  


Avant, JavaScript avait le monopole du développement Web côté Client. En tant que développeurs, on avait le choix du framework : Angular, React, …

Mais au final ça reste du JS.

WebAssembly \([http://webassembly.org/](http://webassembly.org/)\) change cela.

  


WebAssemby est une instruction au format binaire pour une machine virtuelle Stack-based.

C’est une target pour la compilation de langage haut niveau comme C, C++, Rust, qui permet le déploiement d’applications client et serveur.

  


Razor est d’habitude utilisé côté Serveur pour générer dynamiquement du HTML.

Dans Blazor, Razor est utilisé sur le client.

  


Plus spécifiquement, L’engine Razor tourne lors de la compilation pour générer du code C\#.

  


![](https://lh4.googleusercontent.com/ZMZsNG0wIvZbhke9KK-5gxLOFWzECUOSDH4mACsfeJ0Ou6n3inIKa8utXn1mtUe7Iq5P4bTKlOYZadcLesdYry42LEqQ2y8WYrKJQzl2cNa680v8oRYoCkrorIRftUFWWpbDvxhk)

  


Razor ⇒ C\# Render Tree ⇒ JavaScript ⇒ DOM du navigateur.

Lors d’événements, \(clic, …\) , un c\# Render tree est régénéré avec uniquement les différences à apporter au DOM, que javascript applique.

  


Rappel DOM \(Document Object Model\) du navigateur :

Source :[https://www.w3schools.com/js/js\_htmldom.asp](https://www.w3schools.com/js/js_htmldom.asp)

  


L’arbre des objets HTML qui permet à JavaScript de modifier chaques éléments HTML, tous les styles CSS, supprimer, ajouter, modifier :

![](https://lh5.googleusercontent.com/2TP0_Fnnxs8Rh-Ce6mimSCE4JBTARMdwt-asIOrnK-nqv9JHXK8D1gJibN68anwdDvC5NmkUItuW8oQ-kJnRhrRpblfxP25xxHhFu2lLzzlBVrw_XPNTfaMCd63ApqdTmh4CNGh6)

  


Titre 3 : Classes Renderer

  


Les classes Renderer, qui héritent de Microsoft.AspNetCore.Blazor.Rendering.Renderer fournissent un méchanisme pour gérer les components.

  


Les components sont les classes qui implémentent Microsoft.AspNetCore.Blazor.Components.IComponent

  


Les Renderer assurent ces actions aux IComponent :

* la hiérarchie

* Dispatche des event

* Notification lorsque l’UI est updated

  


Pour tourner sur le browser, Blazor a un BrowserRenderer :

Microsoft.AspNetCore.Blazor.Browser.Rendering.BrowserRenderer

  


Pour le test unitaire, Blazor a un test Renderer :

Microsoft.AspNetCore.Blazor.Test.Helpers

  


Titre 2 : Obtenir Blazor

Source :[https://learn-blazor.com/getting-started/getting-blazor/](https://learn-blazor.com/getting-started/getting-blazor/)

  


1. Être à jour sur .NET Core, et avoir si possible la dernière extension de Visual Studio 2017.

2. Installer l’extansion ASP.NET Core Blazor Language Services sur le Marketplace.

3. Créer une nouvelle application ASP.NET Core pour constater les template Blazor

  


![](https://lh5.googleusercontent.com/aaR7pfRBpe-x9CGKLujcC6Xuqernl5C9I28luCXlclRY3OG6-Q4vZIS6MJcH7xZeyf5e210OLIk_02Nwmzawt5MgOWZ_CfrB6iYn95d7NtA1fgzr8TqJie-VSZ1ocEPm3khv_MNs)

  


Titre 2 : Démarrer avec Blazor

  


Source :[https://blogs.msdn.microsoft.com/webdev/2018/03/22/get-started-building-net-web-apps-in-the-browser-with-blazor/](https://blogs.msdn.microsoft.com/webdev/2018/03/22/get-started-building-net-web-apps-in-the-browser-with-blazor/)

  


// TODO : Week-end project this

  


