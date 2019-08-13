# Apprendre RxJS

Des exemples clairs, des explications et des ressources pour RxJS.

_Par [@btroncone](https://twitter.com/BTroncone)_

_Traduit en français par Julien Rousé_

## Introduction

[RxJS](https://github.com/Reactivex/rxjs) est une des technologies les plus en vue dans le développement web aujourd'hui. RxJS offre une approche puissante et fonctionnel pour gérer les événements (_events_). Son intégration dans de nombreux *framework*, *bibliothéques* et *logiciels utilitaires* en font une librairies trés attirante? à apprendre. Ajoutez à ça la capacité d'utiliser cette connaissance dans [pratiquement tous les languages](http://reactivex.io/languages.html), avoir une compréhension solide de la programmation réactive et de ce qu'elle offre semble être une évidence.

**Mais...**

Apprendre RxJS et la programmation réactive est [difficile](https://twitter.com/hoss/status/742643506536153088). Il y a beaucoup de concepts, une surface d'API trés grande, et une changement fondamental dans la manière d'appréhender la programmation quand on passe de la programmation [impérative à déclarative](https://tylermcginnis.com/imperative-vs-declarative-programming/). Ce site essaye de rendre ces concepts clairs et approchables, de donner des examples clairs et facile à explorer, et fournit des liens vers les meilleurs ressources RxJS sur internet. Le but est d'offrir un complément d'information à la [documentation officiel](http://reactivex.io/rxjs/) et aux ressources pré-existantes, tout en offrant une perspective nouvelle sur le sujet afin de débloquer toutes les barrières dans l'apprentissage qu'il pourrait exister. Apprendre RxJS n'est pas aisé mais cela en vaut la peine!

### Débutant en RxJS?

Devenez familier avec tous les concepts importants pour être productif rapidement avec les [concepts essentiels RxJS](./concepts/rxjs-primer.md)!

## Contenu

### Opérateurs

Les opérateurs sont ?le moteur qui anime? les observables, permettant d'exprimer des solutions élégantes de manière déclarative pour résoudre des tâches asynchrones complexes. Cette section contient tous les [opérateurs RxJS](/operators/README.md), inclus avec des exemples clairs et exécutables. Des liens vers des ressources additionnels et des ?recettes? pour chaque opérateur sont également inclus si applicable.

#### Opérateurs par catégorie


- [Combinaison](/operators/combination/README.md)
- [Conditionnel](/operators/conditional/README.md)
- [Création](/operators/creation/README.md)
- [Gestion d'erreur](/operators/error_handling/README.md)
- [Multicasting](/operators/multicasting/README.md)
- [Filtre](/operators/filtering/README.md)
- [Transformation](/operators/transformation/README.md)
- [Utilitaire](/operators/utility/README.md)

**OU...**

[Liste complète des opérateurs par ordre alphabétique](/operators/complete.md)

#### Comprendre les Sujets (Subjects)

Un Sujet (Subject) est un type spécial d'Observable qui partage un seul chemin d'éxécution parmi les Observateurs (Observers).

- [Vue d'ensemble](/subjects/README.md)
- [AsyncSubject](/subjects/asyncsubject.md)
- [BehaviorSubject](/subjects/behaviorsubject.md)
- [ReplaySubject](/subjects/replaysubject.md)
- [Sujet (Subject)](/subjects/subject.md)

#### Concepts

Sans une base solide de connaissance sur la manière dont fonctionne les Observables en arrière plan, c'est facile de confondre RxJS pour de la 'magie'. Cette section aide à solidifier l'apprentissage des concepts cardinaux dont vous aurez besoin pour vous sentir à l'aise avec la programmation réactive et les Observables.

- [Concepts essentiels RxJS](/concepts/rxjs-primer.md)
- [Passer de RxJS v5 -> v6](/concepts/rxjs5-6.md)
- [Comparaison des opérateurs temporels](/concepts/time-based-operators-comparison.md)
- [Comprendre comment importer des Opérateurs](/concepts/operator-imports.md)

#### ?Recettes?

?Recettes? pour les utilisations courantes et des solutions intéressantes avec RxJS.

- [Alphabet Invasion Game](/recipes/alphabet-invasion-game.md)
- [Breakout Game](/recipes/breakout-game.md)
- [Car Racing Game](/recipes/car-racing-game.md)
- [Catch The Dot Game](/recipes/catch-the-dot-game.md)
- [Click Ninja Game](/recipes/click-ninja-game.md)
- [Flappy Bird Game](/recipes/flappy-bird-game.md)
- [Game Loop](/recipes/gameloop.md)
- [Horizontal Scroll Indicator](/recipes/horizontal-scroll-indicator.md)
- [HTTP Polling](/recipes/http-polling.md)
- [Lockscreen](/recipes/lockscreen.md)
- [Matrix Digital Rain](/recipes/matrix-digital-rain.md)
- [Memory Game](/recipes/memory-game.md)
- [Mine Sweeper Game](/recipes/mine-sweeper-game.md)
- [Platform Jumper Game](/recipes/platform-jumper-game.md)
- [Progress Bar](/recipes/progressbar.md)
- [Save Indicator](/recipes/save-indicator.md)
- [Smart Counter](/recipes/smartcounter.md)
- [Stop Watch](/recipes/stop-watch.md)
- [Space Invaders Game](/recipes/space-invaders-game.md)
- [Swipe To Refresh](/recipes/swipe-to-refresh.md)
- [Tank Battle Game](/recipes/tank-battle-game.md)
- [Tetris Game](/recipes/tetris-game.md)
- [Type Ahead](/recipes/type-ahead.md)

## Ressources complémentaires

Débutant en RxJS et en programmation réactive? En complément du contenu de ce site, les trés bonnes  ressources ci-dessous sont un excellent moyen de démarrer et parfaire votre apprentissage!

### Conférences

- [RxJS Live](https://www.rxjs.live/) - Conférence dédié à RxJS qui se tiendra le 5-6 septembre 2019 (Las Vegas)

#### Lectures

- [RxJS Introduction(Introduction à RxJS)](https://rxjs-dev.firebaseapp.com/guide/overview) -
  Documentation officiel (en)
- [The Introduction to Reactive Programming You've Been Missing (L'introduction à la programmation réactive que vous attendiez)](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754) -
  André Staltz
- [RxJS: Observables, Observers and Operators Introduction(RxJS: Introductions aux Observables, aux Observateurs(Observers) et aux Opérateurs)](https://ultimatecourses.com/blog/rxjs-observables-observers-operators) -
  Todd Motto

#### Videos

- [Asynchronous Programming: The End of The Loop(Programmation asynchrone: ?La fin de la boucle?)](https://egghead.io/courses/mastering-asynchronous-programming-the-end-of-the-loop) -
  Jafar Husain
- [What is RxJS?(Qu'est ce que RxJS?)](https://egghead.io/lessons/rxjs-what-is-rxjs) - Ben Lesh
- [Creating Observable from Scratch(Créer des Observables en partant de zéro)](https://egghead.io/lessons/rxjs-creating-observable-from-scratch) -
  Ben Lesh
- [Introduction to RxJS Marble Testing](https://egghead.io/lessons/rxjs-introduction-to-rxjs-marble-testing)
  :dollar: - Brian Troncone
- [Introduction to Reactive Programming(Introduction à la programmation réactive)](https://egghead.io/courses/introduction-to-reactive-programming)
  :dollar: - André Staltz
- [Reactive Programming using Observables(La programmation réactive en utilisant des Observables)](https://www.youtube.com/watch?v=HT7JiiqnYYc&feature=youtu.be) -
  Jeremy Lund

#### Exercices

- [Functional Programming in JavaScript(La programmation fonctionnel avec JavaScript)](http://reactivex.io/learnrx/) - Jafar
  Husain

#### Outils

- [Rx Marbles - Interactive diagrams of Rx Observables(Rx Marbles - Diagrammes intéractifs d'Observables ?réactifs?)](http://rxmarbles.com/) -
  André Staltz
- [Rx Visualizer - Animated playground for Rx Observables(Rx Visualizer - Espace d'expérimentation intéractif pour les Observables ?réactifs?)](https://rxviz.com) -
  Misha Moroshko
- [Reactive.how - Animated cards to learn Reactive Programming(Reactive how - Cartes intéractives pour apprendre la programmation réactive)](http://reactive.how) -
  Cédric Soulas
- [Rx Visualization - Visualizes programming with RxJS(Rx Visualization - Visualiser la programmation avec RxJS)](https://fingerpich.github.io/rx-visualization/) -
  Mojtaba Zarei

_Interessé par RxJS 4? Jeter un oeil à l'excellent [eBook](https://xgrommx.github.io/rx-book/) de [Denis Stoyanov](https://github.com/xgrommx)!_

## Traductions

- [Learn RxJS](/Readme.md) (Version originale)
- [简体中文](https://rxjs-cn.github.io/learn-rxjs-operators)
- [Apprendre RxJS](/fr/Readme.md)

### Précision sur les réferences

Toutes les références incluses dans ce GitBook sont des ressouces, autant gratuites que payantes, qui m'ont aidé énormement pendant que je me familiarisait avec RxJS. Si vous découvrez un article ou une vidéo qui pourrait être ajouté à cette liste, s'il vous plait utilisez le lien _edit this page_ dans le menu en haut, et soumettez un *pull request*. Toutes critiques ou commentaires sont les bienvenus! :)
