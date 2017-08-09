# @Component

A class becomes am Angular Component when we give it Component Metadata. We do so by attaching a Decorator (@). A Decorator is a function that adds metadata to a class, it's member, or its method arguments.

```ts
import { Component } from '@angular/core';

@Component({
  moduleId: module.id,
  selector: 'my-app',
  template: `
    <h1>{{PageTitle}}</h1>
    <p>This is a Component</p>
  `,
  styles: ['.primary {color: red}']
})
export class AppComponent {
  pageTitle: string = 'My First Component';
}
```

<div class="filename">app.module.ts</div>

```ts hl_lines="2"
@NgModule({
  declarations: [ MoviesComponent ]
})
export class AppModule { }
```

Use `PascalCasing` and append `Component` to the class name.

Class' properties and methods use camelCase.

### Metadata Properties

* `animations`: list of animations of this component.
* `changeDetection`: change detection strategy used by this component.
* `encapsulation`: style encapsulation strategy used by this component.
* `entryComponents`: list of components that are dynamically inserted into the view of this component.
* `exportAs`: name under which the component instance is exported in a template.
* `host`: map of class property to host element bindings for events, properties and attributes.
* `inputs`: list of class property names to data-bind as component inputs.
* `interpolation`: custom interpolation markers used in this component's template.
* `moduleId: module.id` If set, the `templateUrl` and `styleUrl` are resolved relative to the component.
* `outputs`: list of class property names that expose output events that others can subscribe to.
* `providers`: list of providers available to this component and its children.
* `queries`: configure queries that can be injected into the component.
* `selector:` Specifies a CSS selector that identifies this directive within a template. Supported selectors include `element`, `[attribute]`, `.class`, and `:not()`. Does not support parent-child relationship selectors.
* `styles: ['.primary {color: red}']` List of inline CSS styles for styling the component’s view.
* `styleUrls: ['my-component.css']` List of external stylesheet URLs for styling the component’s view.
* `template: 'Hello {{name}}'` Inline template URL of the component's view.
* `templateUrl: 'my-component.html' ` External template URL of the component's view.
* `viewProviders: [MyService, { provide: ... }]` List of dependency injection providers scoped to this component's view.
