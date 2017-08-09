### Interpolation (`{{...}}`)

```html
<h3>
  {{title}}
  <img src="{{heroImageUrl}}" style="height:30px">
</h3>
```

You use interpolation to weave calculated strings into the text between HTML element tags and within attribute assignments.

### Property binding (`[property]`)

```html
<img [src]="heroImageUrl">
<button [disabled]="isUnchanged">Cancel is disabled</button>
```

Write a template property binding to set a property of a view element. The binding sets the property to the value of a template expression.

### Attribute, class, and style bindings

```html
<tr><td [attr.colspan]="1 + 1">One-Two</td></tr>
```
The `<td>` element does not have a `colspan` property. It has the `"colspan"` attribute, but interpolation and property binding can set only properties, not attributes. You need attribute bindings to create and bind to such attributes.

```html
<div [class.special]="!isSpecial">
  This one is not so special
</div>
```

Class binding syntax resembles property binding. Instead of an element property between brackets, start with the prefix `class`, optionally followed by a dot (.) and the name of a CSS class: `[class.class-name]`.

```html
<button [style.color]="isSpecial ? 'red': 'green'">Red</button>
<button [style.background-color]="canSave ? 'cyan': 'grey'">
  Save
</button>
```

Style binding syntax resembles property binding. Instead of an element property between brackets, start with the prefix `style`, followed by a dot (.) and the name of a CSS style property: `[style.style-property]`.

### Event binding (`(event)`)

```html
<button (click)="onSave()">Save</button>
```

The only way to know about a user action is to listen for certain events such as keystrokes, mouse movements, clicks, and touches. You declare your interest in user actions through Angular event binding.

For a list of events see [https://developer.mozilla.org/en-US/docs/Web/Events](https://developer.mozilla.org/en-US/docs/Web/Events)

### The safe navigation operator (`?.`)

```html
<div>The current movie's name is {{movie?.name}}</div>
```

The Angular safe navigation operator (`?.`) is a fluent and convenient way to guard against nulls in property paths. The expression bails out when it hits the first null value. The display is blank, but the app keeps rolling without errors.

It works perfectly with long property paths such as `a?.b?.c?.d`.

