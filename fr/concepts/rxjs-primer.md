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

## Inscription (*Subscription*)

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

TODO