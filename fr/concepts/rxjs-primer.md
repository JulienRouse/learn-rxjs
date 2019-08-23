# Concepts essentiels de RxJS

Vous débutez avec RxJS? Dans cet article nous allons passer en revu tous les concepts importants dont vous aurez besoin pour commencer à comprendre, et être productif avec RxJS. Accrochez vous et commençons tout de suite!

## Qu'est-ce qu'est un Observable?

Un Observable représente un flux (*stream*), ou un source de donnée qui peut arriver au fil du temps. Vous pouvez créer un Observable à partir de presque tout, mais le cas le plus courant dans RxJS est de le faire à partir d'événements (*event*). Cela peut être aussi bien un mouvement de la souris, des cliques de la souris, le remplissage d'un champs de texte, ou encore un changement de route. La manière la plus simple de créer un Observable est d'utiliser les fonctions prédéfinies de création des Observables. Par exemple, nous pouvons utiliser la fonction [`fromEvent`](../operators/creation/fromEvent.md) qui crée un Observable à partir des événements générés par des cliques de souris: 


```js
// on importe l'opérateur fromEvent
import { fromEvent } from 'rxjs';

// on récupére la référence sur le bouton qui a l'id 'myButton'
const button = document.getElementById('myButton');

// création de l'Observable sur les cliques de la souris
const myObservable = fromEvent(button, 'click');
```

Après avoir fait cela, nous avons un Observable mais il ne fait rien pour le moment. **C'est parce que les Observables sont 'froid'(*cold*): ils n'activent pas un producteur(*producer*) (comme par exemple si on créait un ??(*event listener*)), jusqu'a ce qu'il y ait une...**

## Inscription (*Subscription*) ?plutot souscription?

Les inscriptions (*subscriptions*) sont ce qui met la machine en route. Prenons l'exemple d'un robinet: il y a un flux d'eau prêt à couler dans votre évier (observable), il faut juste que quelqu'un tourne la poignée du robinet. Dans le cas des observables, ce rôle appartient au `????`(*subscriber*).

Pour créer une inscription, vous appelez la méthode `subscribe`(*que l'on peut traduire par 's'inscrire'*), en passant en paramétre une fonction (ou un objet) - aussi appelé l'`observateur` (*observer*). C'est a cet endroit que vous pouvez décider comment **réagir** (*react*, comme dans *react*-ive programming soit programmation réactive) à chaque événement(*event*). Regardons maintenant ce qu'il se passe dans le scénario précédent quand une inscription est créé:

```js
// on importe l'opérateur fromEvent
import { fromEvent } from 'rxjs';

// on récupére la référence du bouton d'id 'myButton'
const button = document.getElementById('myButton');

// création de l'Observable sur les cliques de la souris
const myObservable = fromEvent(button, 'click');

// pour l'instant, nous allons juste afficher l'événement déclenché par chaque clique
const subscription = myObservable.subscribe(event => console.log(event));
```

Dans l'exemple ci-dessus, quand on appelle `myObservable.subscribe()`cela va:

1. Créer un ?? (*event listener*) sur notre bouton pour les événements liés aux cliques
2. Appeller la fonction que nous avons passé en paramétre de la méthode `subscribe` (l'observateur (*observer*)) à chaque fois qu'un événement de clique est déclenché
3. Retourner une inscription(*subscription*) sous la forme d'un objet avec une méthode `unsubscribe` (se traduisant par 'se désinscrire') qui contient de la logique pour du nettoyage, comme retirer les ??(*event listeners*) approprié

La méthode `subscribe` accepte également un objet map??? qui gére le cas des erreurs et de la terminaison???. Vous ne l'utiliserez surement pas trés réguliérement de cette façon mais c'est de savoir que ça existe si jamais le besoin venait à se faire sentir.

```js
// plutôt qu'une fonction, nous allons passer un objet avec les méthodes next, error et complete
const subscription = myObservable.subscribe({
  // quand une valeur est émise avec succés
  next : event => console.log(event),
  // quand il y a une erreur
  error: error => console.log(error),
  // une fonction à appeler une seule fois à la terminaison
  complete: () => console.log('complete!')
})
```

C'est important de noter que chaque inscription(*subscription*) va créer un nouveau contexte d'éxécution. Cela implique que lorsque l'on fait un appel à `subscribe` une deuxième fois, cela crée une nouveau ???(*event listener*):

```js
// on appelle addEventListener
const subscription = myObservable.subscribe(event => console.log(event));

// on appelle une deuxième fois addEventListener
const secondSubscription = myObservable.subscribe(event => console.log(event));

// on se désinscrit avec unsubscribe
subscription.unsubscribe();
secondSubscription.unsubscribe();
```

Par défaut, une inscription crée une conversation unidirectionnelle entre l'Observable et l'Observateur(*observer*). C'est comme si votre patron (l'Observable) criait (emet (*emitting*)) sur vous (l'observateur(*observer*)) pour avoir fait un merge sur un mauvais PR(pull request). C'est aussi appelé **unicasting**. Si vous préférez une conversation de type conférence - un observable et plusieurs observateurs - vous aurez besoin d'une autre approche qui incluera le **multicasting** avec `Subjects` (*sujets*) soit de manière explicite ou implicite. Plus d'informations à ce sujet dans un prochain article!

Cela vaut la peine de noter que lorsque l'on discute d'une source Observable qui émet des données à des observateurs(*observer*), c'est un modèle *push based* (que l'on peut traduire par modèle ?d'envoi des données?). La source d'envoi des données ne sait pas et ne s'intéresse pas à comment les ?souscripteurs?(*subscribers*) utilisent les données, son rôle est simplement d'envoyer ces données.

Bien que travailler sur un flux d'événement(*stream of events*) est intéressant, cela reste limité.
**Ce qui fait que RxJS est le "lodash pour les événements", ce sont ses...**

## Opérateurs (*Operator*)

Les opérateurs sont une manière de manipuler des valeurs émises par une source, de les transformer et de les retourner sous la forme d'un Observable. Plusieurs opérateurs de RxJS vous sembleront famillier si vous êtes habitué au méthodes de l'object `Array` de JavaScript. Par exemple si vous voulez transformer des valeurs émisent par une source observable, vous pouvez utiliser [`map`](../operators/transformation/map.md):

```js
// on importe l'opérateur map
import { of } from 'rxjs';
import { map } from 'rxjs/operators';
/*
 *  'of' permet de retourner une séquence de valeur
 *  Dans notre cas, il va emettre 1,2,3,4,5 dans l'ordre.
 */
const dataSource = of(1, 2, 3, 4, 5);

// on souscrit à notre observable
const subscription = dataSource
  .pipe(
    // ajoute 1 à chaque valeur émise
    map(value => value + 1)
  )
  // log: 2, 3, 4, 5, 6
  .subscribe(value => console.log(value));
```

Ou si vous voulez filtrer pour des valeurs en particulier, vous pouvez utiliser [`filter`](../operators/filtering/filter.md):

```js
// on importe l'opérateur filter
import { of } from 'rxjs';
import { filter } from 'rxjs/operators';

const dataSource = of(1, 2, 3, 4, 5);

// on souscrit à notre observable
const subscription = dataSource
  .pipe(
    // accepte seulement les valeurs égales ou supérieurs à 2
    filter(value => value >= 2)
  )
  // log: 2, 3, 4, 5
  .subscribe(value => console.log(value));
```

En pratique, si vous avez un probléme à résoudre, il y a de très grande chance **qu'il y ait un opérateur pour ça**. Et bien que le nombre d'opérateur puisse être impressionant au début de votre apprentissage de RxJS, vous pouvez commencer par vous concentrez sur quelques opérateurs très courant pour étre productif au plus vite. Au fur et à mesure de votre apprentissage, vous en viendrez à apprécier la flexibilité des opérateurs quand vous rencontrerez des problémes plus complexes.

**Une chose que vous avez peut être remarqué à propos de l'exemple ci-dessus: les opérateurs existent  à l'intérieur d'un...**

## Pipe ?Tuyau? ?Conduit?

La function ?pipe? est la ligne d'assemblage depuis la source de données observable et qui passe à travers les opérateurs. Tout comme des matières premières rentre dans l'usine puis enchaine des étapes intermédiaires avant de devenir un produit manufacturé fini, la source de données peut passer à travers un ?pipe?-line d'opérateurs avec lesquels vous pouvez manipuler, filtrer et transformer les données comme bon vous semble pour répondre au mieux à votre besoin. Ce n'est pas rare d'utiliser 5 (et même plus) opérateurs à l'intérieur de la fonction ?pipe? d'un observable.

Par exemple, une solution pour un système d'autocomplétion dans un champs de texte (le même genre que quand vous faites une recherche dans un moteur de recherche) qui serait faite avec des Observables pourrait utiliser un groupe d'opérateurs qui optimisent autant la requête que le processus d'affichage:


```js
// une source observable de valeurs d'un champs de texte, et des opérateurs qui s'enchainent dans un pipe
inputValue
  .pipe(
    // on attend 200ms pour une pause
    debounceTime(200),
    // si la valeur n'a pas changé, on ne fait rien
    distinctUntilChanged(),
    // if an updated value comes through while request is still active cancel previous request and 'switch' to new observable
    // si une valeur est mise à jour pendant que la requête est toujours active, on annule la requête précédente et on échange pour un nouvel observable 
    switchMap(searchTerm => typeaheadApi.search(term))
  )
  // on crée une souscription
  .subscribe(results => {
    // mise à jour du DOM
  });
```

**Mais comment savoir quel opérateur correspond à votre besoin immédiat? La bonne nouvelle c'est que...**

## Les opérateurs peuvent être regroupés par catégories

La première étape pour trouver le bon opérateur est de trouver sa catégorie. Avez-vous besoin de filtrer des données depuis une source? Regardez du côté des opérateurs de [`filtre`](../operators/filtering/README.md). Avez-vous plutôt besoin de corriger un bogue, où de déboguer les données qui passe par votre flux observable? Il existe les opérateurs [`utilitaires`](../operators/utility/README.md) qui devraient faire l'affaire.
**Les catégories d'opérateurs sont...**

### [Opérateurs de création](../operators/creation/README.md)

Ces opérateurs permettent la création d'un observable à partir de presque n'importe quelle source. Que ce soit pour un besoin générique où au contraire trés spécifique, vous serez libre de transformer n'importe quoi en flux(*stream*).

Par exemple, supposons que nous voulons créer une barre de progression pour ??un utilisateur qui scroll une page d'un article??. Nous pourrions créer un flux à partir des événements de scroll  en utilisant l'opérateur [`fromEvent`](../operators/creation/fromevent.md).

```js
fromEvent(scrollContainerElement, 'scroll')
  .pipe(
    // nous discuterons de stratégies de nettoyage dans un article futur
    takeUntil(userLeavesArticle)
  )
  .subscribe(event => {
    // mis à jour du DOM
  });
```

Les opérateurs de création les plus utilisés sont 
[`of`](../operators/creation/of.md), [`from`](../operators/creation/from.md),
et [`fromEvent`](../operators/creation/fromevent.md).

### [Opérateurs de combinaison](../operators/combination/README.md)

Les opérateurs de combinaison permettent de regrouper de l'information à partir de plusieurs Observables. L'ordre, la date date et la structure des valeurs émisent sont les principales variations parmi ces opérateurs.

Par exemple, on peut combiner ensemble des mises à jour de plusieurs sources de données pour faire des calculs:

```js
// on garde la dernière valeur émise de chaque source, à chaque fois qu'une source émet.
combineLatest(sourceOne, sourceTwo).subscribe(
  ([latestValueFromSourceOne, latestValueFromSourceTwo]) => {
    // ici on peut effecteur un calcul
  }
);
```

Les opérateurs de combinaisons sont les plus utilisés sont
[`combineLatest`](../operators/combination/combinelatest.md),
[`concat`](../operators/combination/concat.md),
[`merge`](../operators/combination/merge.md),
[`startWith`](../operators/combination/startwith.md), et
[`withLatestFrom`](../operators/combination/withlatestfrom.md).

### [Opérateurs de gestion d'erreurs](../operators/error_handling/README.md)

Les opérateurs de gestion d'erreurs permettent de gérer les erreurs de manière élégante et de faire des ?retries? au besoin:

Par exemple nous pouvons utiliser [`catchError`](../operators/error_handling/catcherror.md) pour se protéger des erreurs réseaux:

```js
source
  .pipe(
    mergeMap(value => {
      return makeRequest(value).pipe(
      	// on gére l'erreur si il y en à une en renvoyant un observable
        catchError(handleErrorByReturningObservable)
      );
    })
  )
  .subscribe(value => {
    // ici on défini une ou des actions
  });
```

L'opérateur de gestion des erreurs le plus utilisé est
[`catchError`](../operators/error_handling/catcherror.md).

### [Opérateurs de filtre](../operators/filtering/README.md)

Les opérateurs de filtre permettent d'accepter - ou de refuser - des valeurs d'une source de données observable and gérer le ?backpressure?, c'est à dire l'engorgement du flux par des données.

Par exemple, nous pouvons utiliser l'opérateur [`take`](../operators/filtering/take.md) pour capturer seulement lea premières `5` valeurs émisent par la source.


```js
source.pipe(take(5)).subscribe(value => {
  // faire quelque chose avec les valeurs 
});
```
Les opérateurs de filtre les plus utilisés sont:
[`debounceTime`](../operators/filtering/debouncetime.md),
[`distinctUntilChanged`](../operators/filtering/distinctuntilchanged.md),
[`filter`](../operators/filtering/filter.md),
[`take`](../operators/filtering/take.md), and
[`takeUntil`](../operators/filtering/takeuntil.md).

### [Opérateurs de multicasting](../operators/multicasting/README.md)

Les observateurs de RxJS sont "froid"(*cold*), où unicast (par défaut, une source de données par souscripteur(*subscriber*)). Ces opérateurs peuvent rendre un observable "chaud"(*hot*), où multicast, ce qui permet de partager des effets de bords(*side-effects*) entre plusieurs souscripteurs(*subscribers).

Par exemple, nous pourrions vouloir que des souscripteurs(*subscribers*) arrivant tard puissent recevoir et partager la dernière valeur d'une source de données active.

```js
const source = data.pipe(shareReplay());

const firstSubscriber = source.subscribe(value => {
  // ici on peut effectuer des actions
});

// puis plus tard...

// un deuxième souscripteur(*subscriber*) reçoit la dernière valeur émise au moment de sa souscription, et partage maintenant le contexte d'exécution avec 'firstSubscriber'
const secondSubscriber = source.subscribe(value => {
  // ici on peut effectuer des actions
});
```

L'opérateur de multicasting le plus utilisé est l'opérateur [`shareReplay`](../operators/multicasting/sharereplay.md).

### [Opérateurs de transformation](../operators/transformation/README.md)

Transformé des valeurs pendant qu'elles passent dans une succession d'opérateurs est une tâche courante. Ces opérateurs permettent d'utiliser des techniques de transformation de données qui couvrent tous les besoins que vous pourriez avoir.

Par exemple, nous pourrions vouloir accumuler un objet qui représente l'état d'une source de données au fil du temps, de manière similaire à  [Redux](https://redux.js.org/):

```js
source
  .pipe(
    scan((accumulatedState, currentState) => {
      return { ...accumulatedState, ...currentState };
    })
  )
  .subscribe();
```

Les opérateurs de transformation les plus utilisés sont:
[`concatMap`](../operators/transformation/concatmap.md),
[`map`](../operators/transformation/map.md),
[`mergeMap`](../operators/transformation/mergemap.md),
[`scan`](../operators/transformation/scan.md), and
[`switchMap`](../operators/transformation/switchmap.md).

## Les opérateurs ont des caractéristiques communes

Bien que les opérateurs peuvent être regroupés en groupes, les opérateurs à l'intérieur de ces groupes partage souvent des caractéristiques communes. En reconnaissant ces caractéristiques, on peut créer un arbre de décision pour ['choisir le meilleur opérateur'](https://rxjs-dev.firebaseapp.com/operator-decision-tree).

**Par exemple, une partie des opérateurs ont pour caractéristique d'être des...**

#### Opérateurs qui applatissent(*flatten*)

Ou, en d'autre mots, des opérateurs qui gérent la souscription d'un observable ?interne?, émettant ces valeurs dans une seule source de données observable. Une utilisation courante des opérateurs qui applatissent est de gérer des requêtes HTTP dans une API basée sur des observables ou des promesses(*promises*), mais ils aident à résoudre beaucoup d'autres problèmes.

```js
fromEvent(button, 'click')
  .pipe(
    mergeMap(value => {
      // cette souscription 'interne' est gérée par la fonction mergeMap, avec la valeur de la réponse émise aux observateurs(observer)
      return makeHttpRequest(value);
    })
  )
  .subscribe(response => {
    // faire quelque chose
  });
```

**Nous pouvons ensuite diviser les opérateurs qui applatissent dans des sous-catégories comme les...**

#### Opérateurs ?interrupteurs?(*switch*)

Les opérateurs ?interrupteurs?(*switch*) éteignent (désinscrivent(*unsubscribe)) l'observable courant et rendent actif un nouvel observable. Ces opérateurs sont utiles dans des situations ou vous n'avez pas besoin d'avoir (ou vous ne devez pas avoir) plus d'un observable actif à la fois.


```js
inputValueChanges
  // Seule la dernière valeur est importante, si une nouvelle valeur arrive, on annule la dernière requête / on change d'observable
  .pipe(
    // requête GET pour obtenir des données
    switchMap(requestObservable)
  )
  .subscribe();
```

Les opérateurs d'interruption incluent `switchAll`,
[`switchMap`](../operators/transformation/switchmap.md), and
[`switchMapTo`](../operators/transformation/switchmapto.md).

#### Opérateurs qui concatènent(*concat*)

Comparable à une file d'attente à un ATM, ou la prochaine transaction ne peut pas commencer jusqu'à ce que la précédente se termine. Pour en revenir aux observables, une seule souscription(*subscription*) se déroulera à la fois, dans l'ordre, déclenché par la terminaison de la précedente. Cela peut être utile dans des situations ou l'ordre d'exécution est important. 

```js
concat(
  firstObservable,
  // 'secondObservable' débutera quand 'firstObservable` sera terminé
  secondObservable,
  // 'thirdObservable' débutera quand 'secondObservable` sera terminé
  thirdObservable
).subscribe(values => {
  // faire quelque chose
});
```

Les opérateurs de concaténations incluent  [`concat`](../operators/combination/concat.md),
[`concatAll`](../operators/combination/concatall.md),
[`concatMap`](../operators/transformation/concatmap.md), et
[`concatMapTo`](../operators/transformation/concatmapto.md).

#### Opérateurs qui fusionnent(*merge*) ?de fusion?

De la même manière que plusieurs files d'attentes peuvent se transformer pour n'en devenir qu'une, les opérateurs de fusion permettent à plusieurs observables actifs de fusionner dans une seul file dans un ordre 'premier arrivé, premier servi' (*FIFO, First In First Out*). Les opérateurs de fusion sont utiles dans des situations ou vous voulez déclencher une actions quand n'importe qu'elle évenement parmi plusieurs source se déclenche.

```js
merge(firstObservable, secondObservable)
  // n'importe quelles valeurs émisent par 'firstObservable' ou 'secondObservable' sont traitées dans l'ordre ou elles arrivent
  .pipe(mergeMap(saveActivity))
  .subscribe();
```

Les opérateurs de fusion incluent [`merge`](../operators/combination/merge.md),
[`mergeMap`](../operators/transformation/mergemap.md), `mergeMapTo` et
[`mergeAll`](../operators/combination/mergeall.md).

## Autre similarités entre des opérateurs

Il y a aussi des opérateurs qui partagent un but commun mais qui offrent des possibilités différentes dans leurs déclenchements. Par exemple, pour se désinscrire d'un observable après qu'une condition spécifique se soit produite, nous pourrions utiliser:

1. [`take`](../operators/filtering/take.md) quand on veut que l'on veut prendre seulement `n`
   valeurs.
2. [`takeLast`](../operators/filtering/takelast.md) quand on veut juste les 
   `n` dernières valeurs.
3. [`takeWhile`](../operators/filtering/takewhile.md) quand on utiliser un prédicat.
4. [`takeUntil`](../operators/filtering/takeuntil.md) quand on veut que la source rest seulement active jusqu'a ce qu'une autre source émétte des valeurs.

Bien que le nombre d'opérateurs présent dans RxJS peut sembler ?intimidant??impressionant?, ces caractéristiques communes permettent de rapidement les assimiler relativement rapidement lors de l'apprentissage de RxJS.

## Quel est l'intéret pour moi? ?Qu'est ce que j'y gagne?

Au fur et à mesure que vous deviendrez plus familier avec ?push based programming? grâce aux Observables, vous pourrez modéliser tous les comportements asynchrones(*async behavior*) de votre application grâce aux flux Observables. Cela permet de développer des solutions simples et flexibles pour résoudre des problèmes avec des comportements complexes.

Par exemple, supposons que nous voulons faire une requête qui sauvegarde l'activité des utilisateurs quand ils répondent à un quiz. Notre implémentation initiale pourrait utiliser l'opérateur [`mergeMap`](../opearators/transformation/mergemap.md), qui déclenche une requête de sauvegarde à chaque événement:


```js
const formEvents = fromEvent(formField, 'click');
const subscription = formEvents
  .pipe(
    map(convertToAppropriateValue),
    mergeMap(saveRequest)
  )
  .subscribe();
```

Plus tard, il se peut qu'il faille s'assurer de l'ordre de ces sauvegardes. Armé de votre connaissance des caractéristiques des opérateurs comme vu ci-dessus, plutôt que d'implémenter un système de queue complexe, vous pouvez simplement remplacer  l'opérateur [`mergeMap`](../operators/transformation/mergemap.md) par
[`concatMap`](../operators/transformation/concatmap.md) :

```js
const formEvents = fromEvent(formField, 'click');
const subscription = formEvents
  .pipe(
    map(convertToAppropriateValue),
    // maintenant la prochaine requête ne va pas commencer avant que la précédente ne termine
    concatMap(saveRequest)
  )
  .subscribe();
```

En changeant simplement d'opérateur, vous avez créé une file qui contient les requêtes. Ce n'est qu'un exemple parmi tant d'autres, il y a beaucoup plus à découvrir!

## Ne vous arrêtez pas en si bon chemin!

Apprendre RxJS peut être intimidant, mais je promet que cela en vaut la peine. Si certains des concepts discutés ci-dessus vous semblent encore flous (ou vous n'y comprenez absolument rien), ne paniquez pas! Ca viendra bientôt. 

Commencez par regarder les opérateurs dans la partie gauche du site pour des exemples, de même que les [ressources d'introduction](../README.md#introductory-resources) nous avons collectés à travers le web. Bonne chance et bientôt vous serez un expert en programmation réactive!