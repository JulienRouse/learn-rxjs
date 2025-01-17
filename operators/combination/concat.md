# concat

#### signature: `concat(observables: ...*): Observable`

## Subscribe to observables in order as previous completes

---

:bulb: You can think of concat like a line at a ATM, the next transaction
(subscription) cannot start until the previous completes!

:bulb: If throughput, not order, is a primary concern, try [merge](merge.md)
instead!

---

<div class="ua-ad"><a href="https://ultimatecourses.com/courses/rxjs"><img src="https://ultimatecourses.com/assets/img/banners/rxjs-banner-desktop.svg" style="width:100%;max-width:100%"></a></div>

### Examples

##### Example 1: Basic concat usage with three observables

(
[StackBlitz](https://stackblitz.com/edit/typescript-ks8chl?file=index.ts&devtoolsheight=100)
)

```js
// RxJS v6+
import { of, concat } from 'rxjs';

concat(
  of(1, 2, 3),
  // subscribed after first completes
  of(4, 5, 6),
  // subscribed after second completes
  of(7, 8, 9)
)
  // log: 1, 2, 3, 4, 5, 6, 7, 8, 9
  .subscribe(console.log);
```

##### Example 2: concat with delayed observable

(
[StackBlitz](https://stackblitz.com/edit/typescript-vsphry?file=index.ts&devtoolsheight=100)
)

```js
// RxJS v6+
import { of, concat } from 'rxjs';
import { delay } from 'rxjs/operators';

concat(
  of(1, 2, 3).pipe(delay(3000)),
  // after 3s, the first observable will complete and subsquent observable subscribed with values emitted
  of(4, 5, 6)
)
  // log: 1,2,3,4,5,6
  .subscribe(console.log);
```

##### Example 3: (Warning!) concat with source that does not complete

(
[StackBlitz](https://stackblitz.com/edit/typescript-njc2jw?file=index.ts&devtoolsheight=100)
)

```js
// RxJS v6+
import { interval, of, concat } from 'rxjs';

// when source never completes, any subsequent observables never run
concat(interval(1000), of('This', 'Never', 'Runs'))
  // log: 1,2,3,4.....
  .subscribe(console.log);
```

### Related Recipes

- [Save Indicator]('../../recipes/save-indicator.md)

### Additional Resources

- [concat](https://rxjs.dev/api/index/function/concat) :newspaper: - Official
  docs
- [Combination operator: concat, startWith](https://egghead.io/lessons/rxjs-combination-operators-concat-startwith?course=rxjs-beyond-the-basics-operators-in-depth)
  :video_camera: :dollar: - André Staltz

---

> :file_folder: Source Code:
> [https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/concat.ts](https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/concat.ts)
