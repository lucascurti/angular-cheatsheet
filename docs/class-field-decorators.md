# Class field decorators for directives/components

### **@Input()** myProperty;

Declares an input property that you can update via property binding (example: `<my-cmp [myProperty]="someExpression">`).

Alias can be used: `@Input(alias_string)`.

#### Example

In this example, the child component `RatingComponent` is nested in the parent component `MoviesComponent`. The `@Input()` decorator is used on the `stars` property of the child component. This allows the component to receive values from the parent component using property binding.

<div class="filename">rating.component.ts</div>
```ts
@Component({
  selector: 'rating',
  templateUrl: 'rating.component.ts'
})
export class RatingComponent {
  @Input() stars: number;
}
```

<div class="filename">movies-list.component.ts</div>

```ts
@Component({
  selector: 'movies',
  templateUrl: 'movies-list.component.ts'
})
export class MoviesComponent {
  movies: IMovie[];
}
```

<div class="filename">movies-list.component.html</div>

```html
<div *ngFor="let movie of movies">
  {{ movie.name }} <rating [stars]="movie.rating"></rating>
</div>

```

### **@Output()** myEvent = new EventEmitter();

Declares an output property that fires events that you can subscribe to with an event binding (example: `<my-cmp (myEvent)="doSomething()">`).

Alias can be used: `@Output(alias_string)`.

#### Example

The child component exposes an `EventEmitter` property with which it `emits` events when something happens. The parent binds to that event property and reacts to those events.

The events payload can be accessed by the parameter `$event` on the components output event handler.

The generic argument (`<payload-type>`) is used to define the event payload type.

<div class="filename">rating.component.ts</div>

```ts
@Component({
  selector: 'rating',
  templateUrl: 'rating.component.ts'
})
export class RatingComponent {
  @Input() stars: number;
  @Output() notify: EventEmitter<string> = new EventEmitter<string>();
  onClick() {
    this.notify.emit('Star was clicked!');
  }
}
```

<div class="filename">rating.component.html</div>

```html
<div (click)="onClick()">
  ★★★★✩
</div>
```

<div class="filename">movies-list.component.ts</div>

```ts
@Component({
  selector: 'movies',
  templateUrl: 'movies-list.component.ts'
})
export class MoviesComponent {
  movies: IMovie[];
  onNotify(message: string): {
    // message = 'Star was clicked!'
    console.log(`Message received: ${message}`);
  }
}
```

<div class="filename">movies-list.component.html</div>

```html
<div *ngFor="let movie of movies">
  {{ movie.name }}
  <rating [stars]="movie.rating" (notify)="onNotify($event)"></rating>
</div>

```

### @HostBinding('class.valid') isValid;

Binds a host element property (here, the CSS class `valid`) to a directive/component property (`isValid`).

### @HostListener('click', ['$event']) onClick(e) {...}

Subscribes to a host element event (`click`) with a directive/component method (`onClick`), optionally passing an argument (`$event`).

### @ContentChild(myPredicate) myChildComponent;

Binds the first result of the component content query (`myPredicate`) to a property (`myChildComponent`) of the class.

### @ContentChildren(myPredicate) myChildComponents;

Binds the results of the component content query (`myPredicate`) to a property (`myChildComponents`) of the class.

### @ViewChild(myPredicate) myChildComponent;

Binds the first result of the component view query (`myPredicate`) to a property (`myChildComponent`) of the class. Not available for directives.

### @ViewChildren(myPredicate) myChildComponents;

Binds the results of the component view query (`myPredicate`) to a property (`myChildComponents`) of the class. Not available for directives.
