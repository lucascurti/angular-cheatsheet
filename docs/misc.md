## Barrels

A way to _roll up exports_ from several ES2015 modules into a single convenient ES2015 module. The barrel itself is an ES2015 module file that re-exports _selected_ exports of other ES2015 modules.

```ts
// movies/movie.component.ts
export class MovieComponent {}

// movies/movie.model.ts
export class Movie {}

// movies/movie.service.ts
export class MovieService {}
```

Without a barrel, a consumer needs three import statements:

```ts
import { MovieComponent } from '../movies/movie.component.ts';
import { Movie }          from '../movies/movie.model.ts';
import { MovieService }   from '../movies/movie.service.ts';
```

You can add a barrel to the `movies` folder (called `index`, by convention) that exports all of these items:

<div class="filename">movies/index.ts</div>

```ts
export * from './movie.model.ts';   // re-export all of its exports
export * from './movie.service.ts'; // re-export all of its exports
export { MovieComponent } from './movie.component.ts'; // re-export the named thing
```

Now a consumer can import what it needs from the barrel.

```ts
import { Movie, MovieService } from '../movies'; // index is implied
```
