After creating a component/directive by calling its constructor, Angular calls the lifecycle hook methods in the following sequence at specific moments:

### ngOnChanges()

Respond when Angular (re)sets data-bound input properties. The method receives a `SimpleChanges` object of current and previous property values.

Called before `ngOnInit()` and whenever one or more data-bound input properties change.

### ngOnInit()

Initialize the directive/component after Angular first displays the data-bound properties and sets the directive/component's input properties.

Called _once_, after the _first_ `ngOnChanges()`.

### ngDoCheck()

Detect and act upon changes that Angular can't or won't detect on its own. Called during every change detection run, immediately after `ngOnChanges()` and `ngOnInit()`.

### ngAfterContentInit()

Respond after Angular projects external content into the component's view.

Called _once_ after the first `ngDoCheck()`.

_A component-only hook_.

### ngAfterContentChecked()

Respond after Angular checks the content projected into the component.

Called after the `ngAfterContentInit()` and every subsequent `ngDoCheck()`.

_A component-only hook_.

### ngAfterViewInit()

Respond after Angular initializes the component's views and child views.

Called _once_ after the first `ngAfterContentChecked()`.

_A component-only hook_.

### ngAfterViewChecked()

Respond after Angular checks the component's views and child views.

Called after the `ngAfterViewInit` and every subsequent `ngAfterContentChecked()`.

_A component-only hook_.

### ngOnDestroy()

Cleanup just before Angular destroys the directive/component. Unsubscribe Observables and detach event handlers to avoid memory leaks.

Called _just before_ Angular destroys the directive/component.
