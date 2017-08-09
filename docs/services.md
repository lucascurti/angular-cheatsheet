```ts
import { Injectable } from '@angular/core';

@Injectable()
export class MovieService {
  getMovies(): IMovie[] {
    // movies array
  };
}
```

A Service is a class with a focus purpose. Used for features that:

* Are independent from any particular component
* Provide shared data or logic across components
* Encapsulate external interactions
The service should be imported and added to the `providers` property of a @Component or in a @ngModule. The service will be available for all childs.

**Important:** If a service is registered twice in 2 different components, we will no longer have a singleton and we will end up with 2 instances of the Service.

```ts
import { MovieService } from './movie.service.ts';

@Component({
  providers: [ MovieService ]
})
export class RootComponent {
  // properties and methods
}
```

**Dependecy Injection** is a coding pattern in which a class receives the instances of objects it needs (called dependencies) from an external source rather than creating them itself.

The Service should be injected in the constructor.

```ts
import { MovieService } from './movie.service.ts';

@Component({
  selector: 'movies',
  templateUrl: 'movies-list.component.html'
})
export class MoviesComponent implements onInit {
  // ...
  constructor (private _movieService: MovieService) {
  }
  ngOnInit(): void {
    this.movies = this._movieService.getMovies();
  }
}
```

