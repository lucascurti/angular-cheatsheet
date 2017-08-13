## Template-driven Forms

Template-driven forms allow you to build the form on the HTML template. 

You place HTML form controls (such as `<input>` and `<select>`) in the component template and bind them to data model properties in the component, using directives like `ngModel`.

You don't create Angular form control objects. Angular directives create them for you, using the information in your data bindings. You don't push and pull data values. Angular handles that for you with `ngModel`. Angular updates the mutable data model with user changes as they happen.

An Angular form has two parts: an HTML-based template and a component class to handle data and user interactions programmatically. 

<div class="filename">src/app/hero-form.component.ts</div>

```ts
import { Component } from '@angular/core';
import { Hero }    from './hero';

@Component({
  selector: 'hero-form',
  templateUrl: './hero-form.component.html'
})
export class HeroFormComponent {

  powers = ['Really Smart', 'Super Flexible',
            'Super Hot', 'Weather Changer'];

  model = new Hero(18, 'Dr IQ', this.powers[0], 'Chuck Overstreet');

  submitted = false;

  onSubmit(formValues) { 
    this.submitted = true;
    console.log(formValues);
  }

  // TODO: Remove this when we're done
  get diagnostic() { return JSON.stringify(this.model); }
}
```

Because template-driven forms are in their own module, you need to add the `FormsModule` to the array of `imports` for the application module before you can use forms.

<div class="filename">src/app/app.module.ts</div>

```ts
...
import { FormsModule } from '@angular/forms';
import { HeroFormComponent } from './hero-form.component';

@NgModule({
  imports: [
    ...
    FormsModule
  ],
  declarations: [
    AppComponent,
    HeroFormComponent
  ]
  ...
})
export class AppModule { }
```

<div class="filename">hero-form.component.html</div>

```html hl_lines="3 8"
<div class="container">
  <h1>Hero Form</h1>
  <form #heroForm="ngForm" (ngSubmit)="onSubmit(heroForm.value)">    
    <div class="form-group">
      <label for="name">Name</label>
      <input type="text" class="form-control" id="name"
            required
            [(ngModel)]="model.name" name="name">
    </div>

    <div class="form-group">
      <label for="alterEgo">Alter Ego</label>
      <input type="text"  class="form-control" id="alterEgo"
            [(ngModel)]="model.alterEgo" name="alterEgo">
    </div>

    <div class="form-group">
      <label for="power">Hero Power</label>
      <select class="form-control"  id="power"
              required
              [(ngModel)]="model.power" name="power">
        <option *ngFor="let pow of powers" [value]="pow">{{pow}}</option>
      </select>
    </div>
  
    <button type="submit" class="btn btn-success"
      [disabled]="!heroForm.form.valid">Submit</button>
  </form>
  {{diagnostic}}
</div>
```
In template driven forms, if you've imported `FormsModule`, you don't have to do anything to the `<form>` tag in order to make use of `FormsModule`. Continue on to see how this works.

The variable `heroForm` is now a reference to the `NgForm` directive that governs the form as a whole.

!!! note "The NgForm directive"
    The NgForm directive supplements the form element with additional features. It holds the controls you created for the elements with an `ngModel` directive and `name` attribute, and monitors their properties, including their validity. It also has its own valid property which is true only if every contained control is valid.

Notice that you also added a `name` attribute to the `<input>` tag and set it to "name", which makes sense for the hero's name. Any unique value will do, but using a descriptive name is helpful. Defining a `name` attribute is a requirement when using `[(ngModel)]` in combination with a form.

!!! note ""
    Internally, Angular creates `FormControl` instances and registers them with an `NgForm` directive that Angular attached to the `<form>` tag. Each `FormControl` is registered under the name you assigned to the `name` attribute.

### ngValue

If your option values are simple strings, you can bind to the normal `value` property
on the option.  If your option values happen to be objects (and you'd like to save the
selection in your form as an object), use `ngValue` instead:

### ngModelGroup

Creates and binds a `FormGroup` instance to a DOM element.

This directive can only be used as a child of `NgForm` (or in other words,
within `<form>` tags).

Use this directive if you'd like to create a sub-group within a form. This can
come in handy if you want to validate a sub-group of your form separately from
the rest of your form, or if some values in your domain model make more sense to
consume together in a nested object.

Pass in the name you'd like this sub-group to have and it will become the key
for the sub-group in the form's full value. You can also export the directive into
a local template variable using `ngModelGroup` (ex: `#myGroup="ngModelGroup"`).

```ts
import {Component} from '@angular/core';
import {NgForm} from '@angular/forms';

@Component({
  selector: 'example-app',
  template: `
    <form #f="ngForm" (ngSubmit)="onSubmit(f)">
      <p *ngIf="nameCtrl.invalid">Name is invalid.</p>
    
      <div ngModelGroup="name" #nameCtrl="ngModelGroup">
        <input name="first" [ngModel]="name.first" minlength="2">
        <input name="last" [ngModel]="name.last" required>
      </div>
      
      <input name="email" ngModel> 
      <button>Submit</button>
    </form>
    
    <button (click)="setValue()">Set value</button>
  `,
})
export class NgModelGroupComp {
  name = {first: 'Nancy', last: 'Drew'};

  onSubmit(f: NgForm) {
    console.log(f.value);  // {name: {first: 'Nancy', last: 'Drew'}, email: ''}
    console.log(f.valid);  // true
  }

  setValue() { this.name = {first: 'Bess', last: 'Marvin'}; }
}
```

### Template-driven validation

To add validation to a template-driven form, you add the same validation attributes as you
would with [native HTML form validation](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Constraint_validation).
Angular uses directives to match these attributes with validator functions in the framework.

You can then inspect the control's state by exporting `ngModel` to a local template variable.
The following example exports `NgModel` into a variable called `name`:

<div class="filename">template/hero-form-template.component.html (name)</div>

```html hl_lines="3"
<input id="name" name="name" class="form-control"
       required minlength="4" forbiddenName="bob"
       [(ngModel)]="hero.name" #name="ngModel" >

<div *ngIf="name.invalid && (name.dirty || name.touched)"
     class="alert alert-danger">
  <div *ngIf="name.errors.required">
    Name is required.
  </div>
  <div *ngIf="name.errors.minlength">
    Name must be at least 4 characters long.
  </div>
  <div *ngIf="name.errors.forbiddenName">
    Name cannot be Bob.
  </div>
</div>
```

!!! note "Why check _dirty_ and _touched_?"
    You may not want your application to display errors before the user has a chance to edit the form.
    The checks for `dirty` and `touched` prevent errors from showing until the user
    does one of two things: changes the value,
    turning the control dirty; or blurs the form control element, setting the control to touched.

## Reactive Forms

Angular _reactive_ forms facilitate a _reactive style_ of programming
that favors explicit management of the data flowing between
a non-UI _data model_ (typically retrieved from a server) and a
UI-oriented _form model_ that retains the states
and values of the HTML controls on screen. Reactive forms offer the ease
of using reactive patterns, testing, and validation.

With _reactive_ forms, you create a tree of Angular form control objects
in the component class and bind them to native form control elements in the
component template, using techniques described below.

<div class="filename">src/app/hero-detail.component.ts</div>

```ts hl_lines="9 10"
import { Component }   from '@angular/core';
import { FormControl, FormGroup } from '@angular/forms';

@Component({
  selector: 'hero-detail',
  templateUrl: './hero-detail.component.html'
})
export class HeroDetailComponent {
  heroForm = new FormGroup ({
    name: new FormControl('', Validators.required)
  });
}
```

A `FormControl` constructor accepts three, optional arguments: the initial data value, an array of validators, and an array of async validators.


<div class="filename">src/app/hero-detail.component.html</div>

```html hl_lines="3 6"
<h2>Hero Detail</h2>
<h3><i>FormControl in a FormGroup</i></h3>
<form [formGroup]="heroForm" novalidate>
  <div class="form-group">
    <label class="center-block">Name:
      <input class="form-control" formControlName="name">
    </label>
  </div>
</form>
```

<div class="filename">src/app/app.module.ts</div>

```ts hl_lines="2 10"
...
import { ReactiveFormsModule } from '@angular/forms'; 

import { AppComponent }        from './app.component';
import { HeroDetailComponent } from './hero-detail.component';

@NgModule({
  imports: [
    BrowserModule,
    ReactiveFormsModule
  ],
  declarations: [
    AppComponent,
    HeroDetailComponent,
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

### Essential form classes

* **_AbstractControl_** is the abstract base class for the three concrete form control classes: `FormControl`, `FormGroup`, and `FormArray`. It provides their common behaviors and properties, some of which are _observable_.

* **_FormControl_** tracks the value and validity status of an _individual_ form control. It corresponds to an HTML form control such as an input box or selector.

* **_FormGroup_** tracks the value and validity state of a _group_ of `AbstractControl` instances. The group's properties include its child controls.
    The top-level form in your component is a `FormGroup`.

* **_FormArray_** tracks the value and validity state of a numerically indexed _array_ of `AbstractControl` instances.

### Introduction to _FormBuilder_

The `FormBuilder` class helps reduce repetition and clutter by handling details of control creation for you.

<div class="filename">src/app/hero-detail.component.ts</div>

```ts hl_lines="11 16"
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'hero-detail',
  templateUrl: './hero-detail.component.html'
})
export class HeroDetailComponent3 {
  heroForm: FormGroup;

  constructor(private fb: FormBuilder) {
    this.createForm();
  }

  createForm() {
    this.heroForm = this.fb.group({
      name: ['', Validators.required ], 
    });
  }
}
```

#### Nested FormGroups

You can group some of the related `FormControls` into a nested `FormGroup`. The `street`, `city`, `state`, and `zip` are properties that would make a good _address_ `FormGroup`. Nesting groups and controls in this way allows you to
mirror the hierarchical structure of the data model and helps track validation and state for related sets of controls.

<div class="filename">src/app/hero-detail.component.ts</div>

```ts hl_lines=""
export class HeroDetailComponent5 {
  heroForm: FormGroup;
  states = states;

  constructor(private fb: FormBuilder) {
    this.createForm();
  }

  createForm() {
    this.heroForm = this.fb.group({ // <-- the parent FormGroup
      name: ['', Validators.required ],
      address: this.fb.group({ // <-- the child FormGroup
        street: '',
        city: '',
        state: '',
        zip: ''
      }),
      power: '',
      sidekick: ''
    });
  }
}
```

<div class="filename">src/app/hero-detail.component.html</div>

```html hl_lines="9"
<h2>Hero Detail</h2>
<h3><i>A FormGroup with multiple FormControls</i></h3>
<form [formGroup]="heroForm" novalidate>
  <div class="form-group">
    <label class="center-block">Name:
      <input class="form-control" formControlName="name">
    </label>
  </div>
  <div formGroupName="address" class="well well-lg">
    <h4>Secret Lair</h4>
    <div class="form-group">
      <label class="center-block">Street:
        <input class="form-control" formControlName="street">
      </label>
    </div>
    <div class="form-group">
      <label class="center-block">City:
        <input class="form-control" formControlName="city">
      </label>
    </div>
    <div class="form-group">
      <label class="center-block">State:
        <select class="form-control" formControlName="state">
          <option *ngFor="let state of states" [value]="state">{{state}}</option>
        </select>
      </label>
    </div>
    <div class="form-group">
      <label class="center-block">Zip Code:
        <input class="form-control" formControlName="zip">
      </label>
    </div>
  </div>
  <div class="form-group radio">
    <h4>Super power:</h4>
    <label class="center-block"><input type="radio" formControlName="power" value="flight">Flight</label>
    <label class="center-block"><input type="radio" formControlName="power" value="x-ray vision">X-ray vision</label>
    <label class="center-block"><input type="radio" formControlName="power" value="strength">Strength</label>
  </div>
  <div class="checkbox">
    <label class="center-block">
      <input type="checkbox" formControlName="sidekick">I have a sidekick.
    </label>
  </div>
</form>
```

### Populate the form model with setValue and patchValue

#### _setValue_

With **`setValue`**, you assign _every_ form control value _at once_ by passing in a data object whose properties exactly match the _form model_ behind the `FormGroup`.

<div class="filename">src/app/hero-detail.component.ts</div>

```ts hl_lines=""
this.heroForm.setValue({
  name:    this.hero.name,
  address: this.hero.addresses[0] || new Address()
});
```

The `setValue` method checks the data object thoroughly before assigning any form control values.

It will not accept a data object that doesn't match the FormGroup structure or is
missing values for any control in the group. This way, it can return helpful
error messages if you have a typo or if you've nested controls incorrectly.
`patchValue` will fail silently.

On the other hand,`setValue` will catch the error and report it clearly.

#### _patchValue_

With **`patchValue`**, you can assign values to specific controls in a `FormGroup`
by supplying an object of key/value pairs for just the controls of interest.

This example sets only the form's `name` control.

<div class="filename">src/app/hero-detail.component.ts</div>

```ts hl_lines=""
this.heroForm.patchValue({
  name: this.hero.name
});
```

With **`patchValue`** you have more flexibility to cope with wildly divergent data and form models. But unlike `setValue`,  `patchValue` cannot check for missing control values and does not throw helpful errors.
