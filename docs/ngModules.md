NgModules help organize an application into cohesive blocks of functionality.

Modules are a great way to organize an application and extend it with capabilities from external libraries.

An NgModule is a class decorated with `@NgModule` metadata. The metadata do the following:

*   Declare which components, directives, and pipes belong to the module.
*   Make some of those classes public so that other component templates can use them.
*   Import other modules with the components, directives, and pipes needed by the components in _this_ module.
*   Provide services at the application level that any application component can use.

Every Angular app has at least one module class, the _root module_. You bootstrap that module to launch the application. The root module is all you need in a simple application with a few components. As the app grows, you refactor the root module into _feature modules_ that represent collections of related functionality. You then import these modules into the root module.

<div class="filename">src/app/app.module.ts</div>

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component.ts';

@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    RouterModule.forRoot(appRoutes, { enableTracing: true })
  ],
  declarations: [
    AppComponent,
    MoviesComponent,
    MovieDetailComponent,
    MovieFilterPipe
  ],
  providers: [ MovieDetailGuard ],
  exports: [ MovieDetailComponent ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

### Bootstrap Array

it defines the Component that is the starting point of the application. This Component is the _Application Component._

The bootstrap array should only be used in the AppModule.

### Declarations Array

It's used to define the `Components`, `Directives` and `Pipes` that belong to the Module.

Every component, directive, and pipe belong to **one and only one** Angular module. Also, they are private by default (they can be exported if needed).

### Exports Array

Allows us to share the `Components`, `Directives` and `Pipes` with modules that import this module.

### Imports Array

Allows us to import supporting modules that export `Components`, `Directives` or `Pipes` to be used on within components that are declared in this module.

Importing a module does NOT provide access to its imported modules. In other words, imports are not inherited.

### Providers Array

Allows us to register service providers at the module level.
