# Programmation asynchrone avec Async et Await

Source : [https://msdn.microsoft.com/fr-fr/library/hh191443\(v=vs.120\).aspx](https://msdn.microsoft.com/fr-fr/library/hh191443%28v=vs.120%29.aspx)

Depuis le .NET Framework 4.5, une approche simplifiée de la programmation asynchrone est possible : Le compilateur effectue le travail difficile dont se chargeait le développeur jusqu'à maintenant.

L'application conserve une structure logique qui ressemble à du code synchrone. **Donc on obtient tous les avantages de la programmation asynchrone avec peu d'effort.**

## Dans quel cas utiliser async

Pour les activitées potentiellement bloquantes comme l'appel à un web service par exemple. Dans un processus synchrone, l'application entière est bloqué par un appel vers une ressource externe lente.

Dans un processus asynchrone, l'application peut poursuivre une autre tâche qui ne dépend pas de la ressource externe jusqu'à ce que la tâche potentiellement bloquante soit terminée.

## Implémentation

```
// Three things to note in the signature: 
//  - The method has an async modifier.  
//  - The return type is Task or Task<T>. (See "Return Types" section.)
//    Here, it is Task<int> because the return statement returns an integer. 
//  - The method name ends in "Async."
async Task<int> AccessTheWebAsync()
{ 
    // You need to add a reference to System.Net.Http to declare client.
    HttpClient client = new HttpClient();

    // GetStringAsync returns a Task<string>. That means that when you await the 
    // task you'll get a string (urlContents).
    Task<string> getStringTask = client.GetStringAsync("http://msdn.microsoft.com");

    // You can do work here that doesn't rely on the string from GetStringAsync.
    DoIndependentWork();

    // The await operator suspends AccessTheWebAsync. 
    //  - AccessTheWebAsync can't continue until getStringTask is complete. 
    //  - Meanwhile, control returns to the caller of AccessTheWebAsync. 
    //  - Control resumes here when getStringTask is complete.  
    //  - The await operator then retrieves the string result from getStringTask. 
    string urlContents = await getStringTask;

    // The return statement specifies an integer result. 
    // Any methods that are awaiting AccessTheWebAsync retrieve the length value. 
    return urlContents.Length;
}
```

* La méthode asynchrone se note NomMethodeAsync
* Elle a un modifier async
* Elle retourne un Type&lt;T&gt; et à la run return un T. \(pas de retour si retour est Task\)
* L'idée est de faire un appel Asynchrone : client.GetStringAsync, qui va donc renvoyer un Task&lt;string&gt;.
* En suite, continuer d'autres traitements indépendants de ce résultat. \(optionnel\)
* puis obtenir le résultat de ce Task&lt;string&gt; grace au modifier await : string urlContent = await getStringTask

Ce qui est intéressant dans ce process, c'est que l'appelant de la méthode asynchrone ne va pas stopper le thread d'exécution de l'appelant.

Le mot clé await provoque un stop de l'exécution du thread.

Au sein de la méthode AccessTheWebAsync, L'appel à GetStringAsync ne bloque rien.

en revanche la ligne await getStringTask va bloquer l'exécution de la méthode AccesTheWebAsync, et le contrôle va retourner à l'appelant. Lorsque la task est terminée, on va revenir ici et obtenir la fameuse url, et poursuivre la méthode.

Si il n'y a pas de travail à faire entre l'appel à GetStringAsync et le await, en une ligne on peut dire

string url = await client.GetStringAsync\(\);

## Caractèristiques d'une méthode async

* La signature inclut un modificateur async
* Le nom de la méthode a un suffixe Async par convention
* Le type de retour est :
  * Task&lt;T&gt; si return T
  * Task si pas de return
  * void dans le cas d'un gestionnaire d'événements async.
  * 



