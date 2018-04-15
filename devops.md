# DevOps

Sources :

* [https://fr.wikipedia.org/wiki/Devops](https://fr.wikipedia.org/wiki/Devops)
* \[[https://fr.wikipedia.org/wiki/Chaîne\_d'outils\_Devops\]\(https://fr.wikipedia.org/wiki/Chaîne\_d'outils\_Devops](https://fr.wikipedia.org/wiki/Chaîne_d'outils_Devops]%28https://fr.wikipedia.org/wiki/Chaîne_d'outils_Devops)\)

## Définition

Concaténation de Dev et Ops. Avec la lourdeur grandissante des solutions informatiques, on a séparé Dev \(développeurs\) et Ops \(Opérations\).

L'équipe de dev répond aux contraintes :

* Pousser une fonctionnalité
* Temps moindre

L'équipe d'Ops répond aux contraintes :

* Stabilité de la production
* Quitte à prendre du temps supplémentaire

Des objectifs contradictoires qui peuvent amener une guerre d'équipe au sein de l'entreprise.

La méthode DevOps est une des quatre nouvelles tendances de l'agilité : la pratique s'appuie sur la méthode Scrum :

* Daily Scrum meeting
* Product backlog
* Sprint
* Product Owner
* Scrum Master
* Rétrospectives des Sprint

La méthode DevOps assure l'**ITSM** \(IT Service Management\), une des bases de l'ITIL \(IT Infrastructure Library\) : identifier un ensemble de **capabilities **\(Capacitées organisationnelles\)  à fournir au client sous forme de services. La capabilities est assurée par les **functions **\(équipes techniques, spéciastes, et processus répondant au besoin\).

Les capabilities auquel répond DevOps sont : \(_les functions sont mes propres suggestions_\)

* Gestion des changements =&gt; Ingénieur DevOps + **Feedback rapide** après déploiement
* Gestion des actifs et des configurations =&gt; Ingénieur DevOps + automatisé los du déploiement
* Gestion des mises en production et des déploiements =&gt; Ingénieur DevOps + **Intégration et déploiement continu**
* Gestion des incidents =&gt; Ingénieur DevOps + méthode Scrum
* Gestion des problèmes =&gt; Ingénieur DevOps + méthode Scrum
* Gestion de la connaissances =&gt; ? Documentation ?

à chaque capabilites une function donc. Et à priori en DevOps, pour chacune de ces capabilities, la function serait :

Ingénieur\(s\) DevOps + Processus

Sanjeev Sharma et Bernie Coyne recommandent, pour répondre à ces capabilities :

* Un déploiement régulier des applications, la seule répétition contribuant à fiabiliser le processus
* Un décalage des tests « vers la gauche », autrement dit de tester au plus tôt
* Une pratique des tests dans un environnement similaire à celui de production
* Une intégration continue incluant des tests continus
* Une boucle d'amélioration courte \(i.e. un feed-back rapide des utilisateurs\)
* Une surveillance étroite de l'exploitation et de la qualité de production factualisée par des métriques et indicateurs clés.

## Chaîne d'outil DevOps

Plan - Create - Verify - Package - Release - Configure - Monitor - Repeat

## CI / CD / CD

Source : [https://devops.com/i-want-to-do-continuous-deployment/\#disqus\_thread](https://devops.com/i-want-to-do-continuous-deployment/#disqus_thread)

![](/assets/cicdcd-1024x447.png)

Ce schémas résume très bien la différence qu'il y a entre Continuous Delivery \(Livraison continue\), et Continuous Deployment \(Déploiement continu\).

Bien que le déploiement continu soit un objectif noble, dans la pratique il est presque impossible de réellement vouloir qu'un archivage entraîne automatiquement un déploiement en production.

Il y a confusion entre les deux parfois, mais c'est en fait nettement différent.

La continuous delivery \(livraison continue\) est le déploiement continu en environnement de Test. En revanche, le passage en environnement de production nécessite que des personnes approuvent le déploiement. En cela il n'est pas totalement automatique donc pas tout à fait continu.

