## Routing & Navigation Basics

<div class="filename">src/index.html</div>

```html
<base href="/">
```
Most routing applications should add a `<base>` element to the `index.html` as the first child in the `<head>` tag to tell the router how to compose navigation URLs.

<div class="filename">app.module.ts</div>

```ts
import { RouterModule, Routes } from '@angular/router';
import { appRoutes } from './routes';

@NgModule({
  imports: [
    ...
    RouterModule.forRoot(appRoutes, { enableTracing: true })
  ]
})
export class AppModule { }
```

<div class="filename">routes.ts</div>

```ts
...
import { Routes } from '@angular/router';

export const appRoutes: Routes = [
  { path: 'movies', component: MovieListComponent },
  { path: 'movie/:id', component: MovieDetailComponent },
  { path: 'welcome', component: WelcomeComponent },
  { path: '', redirectTo: 'welcome' pathMatch: 'full' },
  { path: '**', component: PageNotFoundComponent },
];
```

The `appRoutes` array of _routes_ describes how to navigate. Pass it to the `RouterModule.forRoot` method in the module `imports` to configure the router.

Each `Route` maps a URL `path` to a component. There are _no leading slashes_ in the _path_. The router parses and builds the final URL for you, allowing you to use both relative and absolute paths when navigating between application views.

The `:id` in the second route is a token for a route parameter. In a URL such as `/movie/42`, "42" is the value of the `id` parameter. The corresponding `MovieDetailComponent` will use that value to find and present the movie whose `id` is 42\. You'll learn more about route parameters later in this guide.

The `data` property in the third route is a place to store arbitrary data associated with this specific route. The data property is accessible within each activated route. Use it to store items such as page titles, breadcrumb text, and other read-only, _static_ data. You'll use the resolve guard to retrieve _dynamic_ data later in the guide.

The **empty path** in the fourth route represents the default path for the application, the place to go when the path in the URL is empty, as it typically is at the start. This default route redirects to the route for the `/movies` URL and, therefore, will display the `MoviesListComponent`.

The `**` path in the last route is a **wildcard**. The router will select this route if the requested URL doesn't match any paths for routes defined earlier in the configuration. This is useful for displaying a "404 - Not Found" page or redirecting to another route.

**The order of the routes in the configuration matters** and this is by design. The router uses a **first-match wins** strategy when matching routes, so more specific routes should be placed above less specific routes. In the configuration above, routes with a static path are listed first, followed by an empty path route, that matches the default route. The wildcard route comes last because it matches _every URL_ and should be selected _only_ if no other routes are matched first.

If you need to see what events are happening during the navigation lifecycle, there is the **enableTracing** option as part of the router's default configuration. This outputs each router event that took place during each navigation lifecycle to the browser console. This should only be used for _debugging_ purposes. You set the `enableTracing: true` option in the object passed as the second argument to the `RouterModule.forRoot()` method.

### Router outlet

<div class="filename">src/app/app.component.html</div>

```html
<h1>Angular Router</h1>
<router-outlet></router-outlet>
```

`router-outlet` acts as a placeholder that Angular dynamically fills based on the current router state.

### Router&nbsp;Links

<div class="filename">src/app/app.component.html</div>
```html
<h1>Angular Router</h1>
<nav>
  <a routerLink="/movies" routerLinkActive="active">Movies</a>
  <a
    [routerLink]="['/movie', movie.id]"
    routerLinkActive="active"
    [routerLinkActiveOptions]={exact:true} >{{ movie.name }}</a>
</nav>
<router-outlet></router-outlet>
```

The `RouterLink` directives on the anchor tags give the router control over those elements. The navigation paths are fixed, so you can assign a string to the `routerLink` (a "one-time" binding).

Had the navigation path been more dynamic, you could have bound to a template expression that returned an array of route link parameters (the _link parameters array_). The router resolves that array into a complete URL.

The `RouterLinkActive` directive on each anchor tag helps visually distinguish the anchor for the currently selected "active" route. The router adds the `active` CSS class to the element when the associated _RouterLink_ becomes active. You can add this directive to the anchor or to its parent element.

### Reading Parameters: `snapshot`

<div class="filename">movie.component.ts</div>

```ts
import { ActivatedRoute } from '@angular/router';
...
export class MovieComponent implements OnInit {
  constructor(private _route: ActivatedRoute) { }

  ngOnInit(): void {
    let id = +this._route.snapshot.params['id'];
  }
}
```

When you know for certain that an instance of a `Component` will _never, never, ever_ be re-used, you can simplify the code with the _snapshot_.

The `route.snapshot` provides the initial value of the route parameter map. You can access the parameters directly without subscribing or adding observable operators. It's much simpler to write and read:

**Remember:** you only get the _initial_ value of the parameter map with this technique. Stick with the observable `paramMap` approach if there's even a chance that the parameter will change without leaving the page.

### Routing programmatically

<div class="filename">movie.component.ts</div>

```ts
import { Router } from '@angular/router';
  ...
export class MovieComponent {
  constructor(private _router: Router) { }

  onBack(): void {
    this._router.navigate(['movies']);
  }
}
```

## Guards

You can add _guards_ to the route configuration to handle scenarios such as:

*   Perhaps the user is not authorized to navigate to the target component.
*   Maybe the user must login (_authenticate_) first.
*   Maybe you should fetch some data before you display the target component.
*   You might want to save pending changes before leaving a component.
*   You might ask the user if it's OK to discard pending changes rather than save them.

A guard's return value controls the router's behavior:

*   If it returns `true`, the navigation process continues.
*   If it returns `false`, the navigation process stops and the user stays put.

The guard can also tell the router to navigate elsewhere, effectively canceling the current navigation.

The router supports multiple guard interfaces:

*   `CanActivate` to mediate navigation _to_ a route.

*   `CanActivateChild` to mediate navigation _to_ a child route.

*   `CanDeactivate` to mediate navigation _away_ from the current route.

*   `Resolve` to perform route data retrieval _before_ route activation.

*   `CanLoad` to mediate navigation _to_ a feature module loaded _asynchronously_.

For creating Route Guards, we can use a Service or a Function. On the following examples, a `CanActivate` Service is used to check if the id of a movie exists. A `CanDeactivate` function is used to prevent the user navigating away from the new movies form if the form is dirty.


<div class="filename">routes.ts</div>

```ts
...
import { MovieGuard } from './movies/movie-guard.service.ts';
import { checkDirtyState } from './movies/chech-dirty.ts';

export const appRoutes: Routes = [
  { path:
    'movie/new',
    component: MovieListComponent,
    canDeactivate: ['canDeactivateCreateMovieGuard']
  },
  { path: 'movies', component: MovieListComponent },
  { path:
    'movie/:id',
    component: MovieDetailComponent,
    canActivate: [ MovieGuard ] },
  { path: '404', component: Error404Component },
  { path: '', redirectTo: 'welcome' pathMatch: 'full' },
  { path: '**', redirectTo: '404' },
];
```

The `MovieGuard` Service and the `canDeactivateCreateMovieGuard` function should be added as providers.

<div class="filename">app.module.ts</div>

```ts
import { MovieGuard } from './movies/movie-guard.service.ts';

@NgModule({
  ...
  providers: [
    ...
    MovieGuard,
    {
      provide: 'canDeactivateCreateMovieGuard',
      useValue: checkDirtyState
    }
  ]
})
export class AppModule { }
```

### CanActivate

Service used as the CanActivate Guard.

<div class="filename">movie-guard.service.ts</div>

```ts
import { ActivatedRouteSnapshot, CanActivate, Router } from '@angular/router';
import { Injectable } from '@angular/core';
import { MovieService } from '../shared.movie.service';

@Injectable()
export class MovieGuard implements CanActivate {
  constructor (
    private _movieService:MovieService,
    private _router:Router
    ) {}

  canActivate(route:ActivatedRouteSnapshot) {
    const movieExists = !!this._movieService.getMovie(+route.params['id']);

    if (!movieExists) {
      this._router.navigate(['404']);
    }
    return movieExists;
  }
}
```

### CanDeactivate

Function used as the CanDeactivate Guard.

<div class="filename">chech-dirty.ts</div>

```ts
export function checkDirtyState(component:CreateMovieComponent) {
  if (component.isDirty) {
    return window.confirm('Really want to cancel?');
  }
  return true;
}
```

<div class="filename">create-movie.component.ts</div>

```ts
@Component({
  selector: 'create-movie',
  template: `...`
})
export class CreateMovieComponent {
  isDirty: boolean = true;

  constructor (private _router: Router) {}

  cancel() {
    this._router.navigate(['/movies']);
  }
}
```

## Resolve

If you were using a real world API, there might be some delay before the data to display is returned from the server. You don't want to display a blank component while waiting for the data.

It's preferable to pre-fetch data from the server so it's ready the moment the route is activated. This also allows you to handle errors before routing to the component. There's no point in navigating to a Movie detail for an `id` that doesn't have a record. It'd be better to send the user back to the `Movies List` that shows only valid Movies.

In summary, you want to delay rendering the routed component until all necessary data have been fetched.

You need a _resolver_.


<div class="filename">movie-resolver.service.ts</div>

```ts
import { Injectable } from '@angular/core';
import { ActivatedRoute, Router, Resolve } from '@angular/router';

@Injectable()
export class MovieResolver implements Resolve<IMovie> {
  constructor(
    private _movieService: MovieService,
    private _router: Router,
    private _route: ActivatedRoute) {}

  resolve() {
    let id = +this._route.snapshot.params['id'];
    let movie = this._movieService.getEvent(id);
    if (movie) {
      return movie;
    } else { // id not found
      this.router.navigate(['/movies']);
      return null;
    }
  }
}
```

<div class="filename">/movie.service.ts</div>

```ts
@Injectable()
export class MovieService {
  getMovie(id: number): IMovie {
    let subject = new Subject();
    let movie = MOVIES.find(movie => movie.id === id);
    setTimeout(() => { subject.next(movie); subject.complete() }, 2000);
    return subject;
  }
}
```

<div class="filename">routes.ts</div>

```ts
...
export const appRoutes: Routes = [
  ...
  { path: 'movies', component: MovieListComponent },
  { path:
    'movie/:id',
    component: MovieDetailComponent,
    resolve: { movieData: MovieResolver } },
  ...
];
```

Note that the returned data of `MovieResolver` is assigned to `movieData`. Then, on the Component, `movieData` is retrieved from the route snapshot array.

<div class="filename">movie.component.ts</div>

```ts
...
export class MoviesComponent implements OnInit {
  movie: IMovie;

  constructor(
    private movieService: MovieService,
    private _route: ActivatedRoute) {}

  ngOnInit() {
    this.movie = this._route.snapshot.data['movieData'];
  }
}
```

<div class="filename">app.module.ts</div>

```ts
@NgModule({
  ...
  providers: [
    ...
    MovieResolver,
  ]
})
export class AppModule { }
```