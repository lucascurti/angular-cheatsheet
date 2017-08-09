### @Input() / @Output()

See [Class field decorators](class-field-decorators.md)

### Using a Template `#variable`

This is an easy and straight forward way to communicate between components.

In this example `RatingComponent` is a child of `MoviesListComponent`.

Note how the local variable `#ratingVar` points to `RatingComponent`. This variable is used by the parent to access the properties and methods of the child component.

<div class="filename">movies-list.component.html</div>

```html
<div *ngFor="let movie of movies">
  {{ movie.name }}
  <rating #ratingVar></rating>
  <p>Stars: {{ ratingVar.star }}</p>
  <button (click)="ratingVar.addStar()"></button>
</div>
```

<div class="filename">rating.component.ts</div>

```ts
@Component({
  selector: 'rating',
  templateUrl: 'rating.component.ts'
})
export class RatingComponent {
  @Input() stars: number;
  stars: number = 3;
  addStar() {
    console.log('Star added');
  }
}
```
