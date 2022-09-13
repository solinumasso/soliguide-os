# Notre workflow

<aside>
🎯 Décrit le workfow git utilisé par l’équipe tech de Solinum et les process à utiliser en tant que développeur.euse salarié.e, prestataire ou pro bono.

</aside>

# Introduction

## a) Les environnements

Actuellement chez Solinum, et plus particulièrement pour le Soliguide, 3 environnements distincts sont utilisés :

- La prod qui est sur [soliguide.fr](https://soliguide.fr/) ;
- La preprod qui est sur [soliguide.site](https://soliguide.site/) ;
- L'environnement de test qui est sur [prosoliguide.fr](https://prosoliguide.fr/) ;
- Les environnements de dev locaux.

## b) Le repo git

On utilise un monorepo pour Soliguide avec le frontend et le backend (ou l'API) dessus. Pour avoir plus de détails sur l’installation, veuillez vous référer au [README](https://github.com/solinumasso/soliguide) du projet sur GitHub.

# A. Les besoins

Pour le développement du Soliguide, nous avons des besoins spécifiques liés à notre mode de financement et à l'utilisation de notre produit dans un contexte social fort.

- [x] Les mises en prod doivent pouvoir se faire régulièrement et avec des versions stables et testés au préalable.
- [x] On doit pouvoir mettre en prod des bugfix et tester des features sans pour autant que l'un empiète sur l'autre.
- [x] On doit pouvoir mettre en prod plusieurs fonctionnalités d'un coup en ayant vérifié qu'elles soient stables tout en continuant à en développer d'autres si besoins.
- [x] On doit pouvoir faire des reviews de code facilement avant de merger une fonctionnalité.
- [x] On doit pouvoir faire des commentaires sur le code si on ne le comprend pas pour documenter le code et faciliter sa reprise.

# B. Le workflow

## a) Les branches

Il y a 3 branches principales :

- **master** : c'est la branche de la prod. Tout le code qui est mergé sur cette branche doit être revue et stable.
- **release** : c'est la branche pour stabiliser le code avant une release. C'est cette branche qui sera sur la preprod et qui servira pour les démos. Elle servira de tampon avant de passer le code vers master. Le nom de la branche sera le numéro de version sous la forme _release-x.x.x_
- **develop** : c'est la branche de test. Elle servira au⋅à la PO d'environnement pour tester les fonctionnalités et les valider dans le backlog.

En plus de ces 3 branches principales, il y a d'autres branches qui correspondent à des tickets et seront utilisé par un⋅e seul⋅e développeur⋅euse :

- **feature** : sert de base pour créer une feature. Son nom sera de la forme _numero-type-nom_. _type_ sera compris parmi : _bug_, _feat_, _tech, data_ et _test_ dépendamment du type de ticket dans le backlog. _numero_ correspond au numéro du ticket et _nom_ correspond au titre du ticket simplifié, il doit être compréhensible.
- **ticket** : sert pour répondre à un ticket
- **hotfix** : sert pour les bugs critiques, qui ne peuvent pas attendre une mise en prod classique pour être déployé. Son nom sera de la forme _numero-hotfix-nom-du-bug_. Le nom du bug est différent du nom du ticket. Il s'agit d'une version simplifié du retour fait par le⋅la PO.

⚠️ Les types _bug_ devront absolument correspondre à des tickets dans le backlog. Si un développeur trouve un bug, il doit créer un ticket et l'estimer avant de le corriger.

Dans le cas où un ticket pourrait être segmenté en plusieurs tickets pendant le sprint, il faudra que le développeur ou la développeuse responsable du ticket créé un ticket par segment et développe son ticket sur une branche dédiée à chaque nouveau ticket.

## b) Les merges et rebase

⚠️ **ATTENTION** : la règle d'or du rebase → NE JAMAIS au grand JAMAIS faire de rebase sur une branche commune (**master**, **develop** et **release**).

On peut faire des rebase depuis ces branches, mais JAMAIS l'inverse. C'est un coup à avoir des conflits à l'infini, devoir supprimer des branches locale et autre bidouilles infâmes. Pour ces branches, il faudra uniquement merger les branches contenant les commits à intégrer. Après, un rebase, pour pusher la branche rebasé, il faut utiliser la commande `git push --force`

Bon maintenant que cette règle est établie, on peut continuer !

Tout merge devra être fait après avoir fait une pull request (PR) sur GitHub, quelle que soit la personne qui a développé le code.

**master** : cette branche ne devra être mergé avec AUCUNE autre branche. Ce sont uniquement d'autres branches qui seront mergé avec celle-ci.

**release** : cette branche ne devra être mergé QU'AVEC **master** et **develop** et uniquement tiré QUE de **develop**.

**develop** : cette branche ne devra être mergé avec AUCUNE branche. Ce sont uniquement d'autres branches qui devront être mergé avec celle-ci.

⚠️ Les branches suivantes devront être rebasé depuis la branche de destination avant d'être mergé.

**bugfix** : cette branche ne devra être mergé QU'AVEC **release** et uniquement tiré de **release**.

**hotfix** : cette branche ne devra être mergé QU'AVEC **develop** et **master** et uniquement tiré de **master**.

**type-scope-nom** : cette branche ne devra être mergé QU'AVEC **develop** et uniquement tiré de **develop**.

## c) L’environnement

La **prod** ne sera mis à jour que par un⋅e développeur⋅euse interne (sauf cas de congé) une fois à la fin d'un sprint sauf correction de bug critique. La mise en prod se fera manuellement le soir à 20:00 pour ne pas gêner les chargées de développement locaux dans leur travail. Seule la branche **master** sera sur cet environnement.

La **preprod** servira à stabiliser la branche **release** avant de la merger avec **master**. Cet environnement devra être un copier-coller de la **prod**. Seule la branche **release** sera sur cet environnement. Elle devra être mise à jour avec la CI, mais en attendant ce celle-ci soit implémenté, il faudra qu'un⋅e développeur⋅euse interne mette manuellement.

L'environnement de **test** servira à tester toutes les fonctionnalités mergé avec **develop** qui sera l'unique branche sur cet environnement. Elle devra être mise à jour avec la CI, mais en attendant ce celle-ci soit implémenté, il faudra qu'un⋅e développeur⋅euse interne mette manuellement.

## d) Les commits

Les commits seront standardisés en suivant la spécification Conventional Commits.

```xml
<type>([optional scope]): <description>

[optional body]

[optional footer(s)]
```

Le **type** peut être l’un des suivants :

- **build** : Modifications qui s'appliquent sur le système de build ou sur des dépendances externes.
- **ci** : Modifications des fichiers de configuration et les scripts de l'Intégration Continue.
- **docs** : Modifications de la documentation uniquement.
- **feat** : Une nouvelle fonctionnalité.
- **fix** : Une correction de bugs.
- **perf** : Du code qui améliore les performances du produit.
- **refactor** : Du code qui n'ajoute aucune fonctionnalité ni ne corrige un bug.
- **style** : Modifications qui n'affectent pas le sens du code (espace, formatage, points-virgules manquants,...)
- **test** : Ajoute des tests manquants ou améliore les tests présents

Le **scope** est optionnel SAUF dans le cas d'une fonctionnalité. Dans ce cas, le scope devra être le type de contenu que touche la fonctionnalité.

La **description** explique rapidement ce que fait ce commit. On doit pouvoir comprendre le contenu du commit juste avec la description si on le recherche plus tard.

Le **body** précise le titre si besoin. Une personne qui n'a pas développé le contenu du commit doit pouvoir comprendre ce qui a été fait.

Le(s) **footer(s)** doivent être l'un de :

- _BREAKING CHANGE_ : les changements qui sont survenues
- _Reviewed-by_: pseudo-github
- _~~Refs_ #numeroDIssueGithub~~

## e) Les labels

Les labels sont gérés automatiquement par _lerna_ lors d'un merge avec **master** en fonction des commits fait lors de ce merge (d'où l'importance de rédiger des commits _propres_ et _standardisés_).

# C. Collaborer au projet

## a) Pour les developpeur⋅euses internes

Ils/elles ont les droits pour ajouter des personnes sur le repo et d'ajouter de nouveaux projets.

Ils/elles ont des droits d'accès _admin_ sur GitHub au niveau d'un ou plusieurs repo.

Ils/elles s'occupent de faire les merge et les reviews.

Ils/elles peuvent être responsables d'une fonctionnalité

## c) Pour les developpeur⋅euses externes

Ils/elles ont des droits d'accès _triage_ pour contribuer à un ou plusieurs repo.

Ils/elles doivent créer des forks du projet pour pouvoir y contribuer.

## d) Pour le/la Product Owner

Il/elle a des droits d'accès _read_ sur GitHub.

# D. Petit guide pratique

⚠️ Petit disclaimer : Le processus de développement part TOUJOURS d'un ticket. Donc si on remarque un bug, ou si le ticket doit être splitté pendant le sprint, il faudra créer un ticket avant de développer cette fonctionnalité.

### Je suis un⋅e développeur⋅euse externe et je veux forker un projet.

1. Avant de commencer à développer, je me rends sur le repo github du projet sur lequel je suis assigné⋅e.
2. Je clique sur fork en haut à droite pour ajouter le projet à la liste de mes projets.
3. Je clone le projet forké sur mon ordinateur avec le protocole HTTPS.
4. Je relie mon projet forké au projet de Solinum avec la commande : `git remote add upstream [https://github.com/PROJECT_USERNAME/PROJECT.git](https://github.com/PROJECT_USERNAME/PROJECT.git)`. Vous pourrez trouver le lien du projet en allant sur la page du projet et en cliquant sur le bouton vert `Code`. En tapant la commande `git remote -v` vous devriez avoir un résultat avec _origin_ et _upstream_.
5. C'est bon, je peux commencer à coder !

### Je suis développeur et je veux contribuer

1. Je vérifie que je suis assigné à un ticket.
2. Si c'est mon premier ticket, je récupère la branche **develop** localement avec la commande `git fetch && git checkout --track origin/develop`.
3. Je crée ma branche **feature** avec le bon standard de nommage avec la commande `git checkout -b ma-branch develop`.
4. Je passe mon ticket de la colonne _Sprint Backlog_ à _En cours_ sur Airtable.
5. Je développe le contenu du ticket.
6. Une fois cela fait, j'envoie mon code sur le repo distant avec les commandes : `git fetch`, `git rebase origin/develop` pour mettre à jour ma branche avec les derniers commits puis `git push --set-upstream origin ma-branch` ou `git push origin ma-branch --force` si j'ai déjà pushé sur le repo distant avant, pour mettre mes commits sur le repo distant
7. Je crée ma PR avec l'outil adapté sur GitHub en ciblant la branche **develop**.
8. Je passe mon ticket de la colonne _En cours_ à _Review_.
9. Si j'ai des retours de la part du responsable de fonctionnalité, je corrige ces retours.
10. Une fois la PR accepté, je supprime ma branche locale et la branche distante avec les commandes `git branch -D ma-branch` et `git push origin --delete ma-branch`.

### Je suis développeur⋅euse interne et je veux faire une review de code en PR.

1. Je regarde le code sur l'outil de GitHub pour les PR
2. Je teste le code en local avec la commande `git fetch && git checkout --track origin/sa-branche`.
3. Si des retours sont à faire, je les fais sur GitHub
4. Sinon, je merge la PR
5. Dans tous les cas, je mets à jour Airtable en déplaçant le ticket de _Review_ à _Anomalie / Retours_ ou _~~A tester / préprod~~_ _Mergé et à déployer_ respectivement.
6. Je mets à jour l'environnement de **test**.

### Je suis développeur⋅euse interne et je veux stabiliser une release avant de mettre en prod de fonctionnalité

1. Je crée ma branche de **release** avec le nom standardisé avec la commande `git checkout -b release develop`.
2. Je crée une PR vers master sur Github.
3. Je teste le site à fond pour vérifier qu'il n'y a pas de régression.
4. S'il y a des bugs, je crée une branche **bugfix** avec le nom approprié avec la commande `git checkout -b bugfix release`.
5. Je suis les mêmes étapes que si c'était une branche de feature mais avec **release** comme destination et la **preprod** comme environnement.
6. Une fois stable, je merge la PR de **release** vers **master**.
7. Je mets à jour le Airtable en passant les tickets dans la colonne _A déployer en production_ puis je mets à jour la **prod** pendant les horaires appropriés.

### Je suis PO et je reçois une notification pour tester une fonctionnalité

1. Je vérifie sur Airtable quels sont les tickets à tester
2. Je vais sur l'environnement de **test**.
3. Je teste le ticket.
4. En fonction de si le ticket est fonctionnel ou pas, je mets à jour Airtable en déplaçant le ticket de _Review_ à _Anomalie / Retours_ ou ~~P*réprod*~~ _A déployer en production_ respectivement.

# Contributions spécifiques

La **traduction** se fait avec l'outil BabelEdit disponible [ici](https://www.codeandweb.com/babeledit)

# Autres infos

## Version des outils

**mongo** : v4.4.2

**node** : v14.4.0

**angular** : v11.1.4

---

Sources :

Conventionals commit : [https://www.conventionalcommits.org/fr/v1.0.0/](https://www.conventionalcommits.org/fr/v1.0.0/)

Git en méthode agile : [https://www.atlassian.com/fr/agile/software-development/git](https://www.atlassian.com/fr/agile/software-development/git)

Git workflow selon Atlassian : [https://www.atlassian.com/git/tutorials/comparing-workflows#centralized-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#centralized-workflow)

Git workflow selon OVH UX : [https://medium.com/@OVHUXLabs/la-puissance-des-workflows-git-12e195cafe44](https://medium.com/@OVHUXLabs/la-puissance-des-workflows-git-12e195cafe44)

Gestion des merge et des rebases : [https://delicious-insights.com/fr/articles/bien-utiliser-git-merge-et-rebase/#nettoyer-son-historique-local-avant-envoi](https://delicious-insights.com/fr/articles/bien-utiliser-git-merge-et-rebase/#nettoyer-son-historique-local-avant-envoi)

Contribuer à un projet Open source : [http://blog.davidecoppola.com/2016/11/howto-contribute-to-open-source-project-on-github/](http://blog.davidecoppola.com/2016/11/howto-contribute-to-open-source-project-on-github/)

Les utilisations de `git rebase` et `git merge` : [https://www.atlassian.com/git/tutorials/merging-vs-rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
