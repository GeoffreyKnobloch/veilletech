# Caching en ASP .net



[https://pseudomuto.com/2012/02/efficient-thread-safe-caching-with-asp.net/](https://pseudomuto.com/2012/02/efficient-thread-safe-caching-with-asp.net/)

Article de 2012 donnant la solution clé en main pour du caching efficace et thread safe en ASP .net à l'aide d'une classe CacheManager qui va mettre  à disposition une méthode :



```
public static TObj GetCachedObject<TObj>(string cacheKey, Func<TObj> creator) where TObj : class
```

Ce manager s'appuira sur un dictionnaire &lt;string, object&gt; \(un objet par value en cache\) pour effectuer des lock minimaliste sans lock l'instance de classe cache en entier.



