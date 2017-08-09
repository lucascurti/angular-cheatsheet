## Built-in structural directives

### *ngIf

```html
<hero-detail *ngIf="isActive else elseBlock"></hero-detail>
<ng-template #elseBlock>
  Alternate text (primary text is hidden)
</ng-template>
```

When the `isActive` expression returns a truthy value, `NgIf` adds the `HeroDetailComponent` to the DOM. When the expression is falsy, `NgIf` removes the `HeroDetailComponent` from the DOM, destroying that component and all of its sub-components.

If it is necessary to display a template when the `expression` is falsy use the `else` template binding as shown. Note that the `else` binding points to a `<ng-template>` labeled `#elseBlock`. The template can be defined anywhere in the component view but is typically placed right after `ngIf` for readability.

### *ngFor
```html
<div *ngFor="let hero of heroes; let i=index">
  {{ hero.name }} has an index of {{ i }}
</div>
```
`NgFor` is a _repeater_ directive — a way to present a list of items. You define a block of HTML that defines how a single item should be displayed. You tell Angular to use that block as a template for rendering each item in the list.

### NgSwitch
```html
<div [ngSwitch]="currentHero.emotion">
  <happy-hero    *ngSwitchCase="'happy'"></happy-hero>
  <sad-hero      *ngSwitchCase="'sad'"></sad-hero>
  <confused-hero *ngSwitchCase="'confused'"></confused-hero>
  <unknown-hero  *ngSwitchDefault></unknown-hero>
</div>
```
NgSwitch is like the JavaScript `switch` statement. It can display _one_ element from among several possible elements, based on a _switch condition_. Angular puts only the selected element into the DOM.


## Built-in attribute directives

### NgClass
```html
<div [ngClass]="currentClasses">
  This div is initially saveable, unchanged, and special
</div>
```
```ts
currentClasses =  {
  'saveable': this.canSave,
  'modified': !this.isUnchanged,
  'special':  this.isSpecial
};
```
Adds and removes CSS classes on an HTML element.

### NgStyle
```html
<div [ngStyle]="currentStyles">
  This div is initially italic, normal weight.
</div>
```
```ts
currentStyles = {
  'font-style': this.canSave ? 'italic' : 'normal',
  'font-weight': !this.isUnchanged ? 'bold' : 'normal',
};
```
Updates an HTML element styles.

### [(NgModel)] - Two-way binding
```html
<input [(ngModel)]="currentHero.name">
```
When developing data entry forms, you often both display a data property and update that property when the user makes changes. Two-way data binding with the `NgModel` directive makes that easy.

Before using the `ngModel` directive in a two-way data binding, you must import the `FormsModule` and add it to the Angular module's `imports`.