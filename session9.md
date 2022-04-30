### *Session 9*

# Routing

In a single-page app, you change what the user sees by showing or hiding portions of the display that correspond to particular components, rather than going out to the server to get a new page. As users perform application tasks, they need to move between the different views that you have defined.

To handle the navigation from one view to the next, you use the Angular `Router`. The `Router` enables navigation by interpreting a browser URL as an instruction to change the view.

### Generate an application with routing enabled

The following command uses the Angular CLI to generate a basic Angular application with an application routing module, called `AppRoutingModule`, which is an NgModule where you can configure your routes. The application name in the following example is `routing-app`.

```
ng new routing-app --routing --defaults
```

### Adding components for routing

To use the Angular router, an application needs to have at least two components so that it can navigate from one to the other. To create a component using the CLI, enter the following at the command line where `first` is the name of your component:

```
ng generate component first
```

Repeat this step for a second component but give it a different name. Here, the new name is `second`.

```
ng generate component second
```

The CLI automatically appends Component, so if you were to write `first-component`, your component would be `FirstComponentComponent`.`

### Importing your new components

To use your new components, import them into `AppRoutingModule` at the top of the file, as follows:

```typescript
import { FirstComponent } from './first/first.component';
import { SecondComponent } from './second/second.component';
```

### Defining a basic route

There are three fundamental building blocks to creating a route.

Import the `AppRoutingModule` into `AppModule` and add it to the `imports` array.

The Angular CLI performs this step for you. However, if you are creating an application manually or working with an existing, non-CLI application, verify that the imports and configuration are correct. The following is the default `AppModule` using the CLI with the `--routing` flag.

`AppModule.ts`

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppRoutingModule } from './app-routing.module'; // CLI imports AppRoutingModule
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule // CLI adds AppRoutingModule to the AppModule's imports array
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

1. Import `RouterModule` and `Routes` into your routing module.

    The Angular CLI performs this step automatically. The CLI also sets up a `Routes` array for your routes and configures the `imports` and `exports` arrays for `@NgModule()`.

    `AppRoutingModule.ts`

    ```typescript
    import { NgModule } from '@angular/core';
    import { Routes, RouterModule } from '@angular/router'; // CLI imports router

    const routes: Routes = []; // sets up routes constant where you define your routes

    // configures NgModule imports and exports
    @NgModule({
        imports: [RouterModule.forRoot(routes)],
        exports: [RouterModule]
    })
    export class AppRoutingModule { }
    ```

2. Define your routes in your `Routes` array.

    Each route in this array is a JavaScript object that contains two properties. The first property, `path`, defines the URL path for the route. The second property, `component`, defines the component Angular should use for the corresponding path.

    `AppRoutingModule.ts`

    ```typescript
    const routes: Routes = [
    { 
        path: 'first-component', component: FirstComponent },
    { 
        path: 'second-component', component: SecondComponent },
    ];
    ```

3. Add your routes to your application.

    Now that you have defined your routes, add them to your application. First, add links to the two components. Assign the anchor tag that you want to add the route to the `routerLink` attribute. Set the value of the attribute to the component to show when a user clicks on each link. Next, update your component template to include `<router-outlet>`. This element informs Angular to update the application view with the component for the selected route.

    `Template with routerLink and router-outlet`

    ```html
    <h1>Angular Router App</h1>
    <!-- This nav gives you links to click, which tells the router which route to use (defined in the routes constant in  AppRoutingModule) -->
    <nav>
        <ul>
            <li><a routerLink="/first-component" routerLinkActive="active">First Component</a></li>
            <li><a routerLink="/second-component" routerLinkActive="active">Second Component</a></li>
        </ul>
    </nav>
    <!-- The routed views render in the <router-outlet>-->
    <router-outlet></router-outlet>
    ```

## Getting route information

Often, as a user navigates your application, you want to pass information from one component to another. For example, consider an application that displays a shopping list of grocery items. Each item in the list has a unique `id`. To edit an item, users click an Edit button, which opens an `EditGroceryItem` component. You want that component to retrieve the id for the grocery item so it can display the right information to the user.

Use a route to pass this type of information to your application components. To do so, you use the `ActivatedRoute` interface.

To get information from a route:

1. Import `ActivatedRoute` and `ParamMap` to your component.

    `In the component class`
    ```typescript
    import { Router, ActivatedRoute, ParamMap } from '@angular/router'
    ```

    These `import` statements add several important elements that your component needs.

2. Inject an instance of `ActivatedRoute` by adding it to your application's constructor:`

    `In the component class`
    ```typescript
    constructor(
        private route: ActivatedRoute,
    ) {}
    ```

3. Update the `ngOnInit()` method to access the `ActivatedRoute` and track the name parameter:

    `In the component `
    ```typescript
    ngOnInit() {
        this.route.queryParams.subscribe(params => {
            this.name = params['name'];
        });
    }
    ```

# Lifecycle hooks

A component instance has a lifecycle that starts when Angular instantiates the component class and renders the component view along with its child views. The lifecycle continues with change detection, as Angular checks to see when data-bound properties change, and updates both the view and the component instance as needed. The lifecycle ends when Angular destroys the component instance and removes its rendered template from the DOM. Directives have a similar lifecycle, as Angular creates, updates, and destroys instances in the course of execution.

### Lifecycle event sequence
After your application instantiates a component or directive by calling its constructor, Angular calls the hook methods you have implemented at the appropriate point in the lifecycle of that instance.

Angular executes hook methods in the following sequence. Use them to perform the following kinds of operations.

* `ngOnChanges()`
    Respond when Angular sets or resets data-bound input properties. The method receives a SimpleChanges object of current and previous property values.

    Note that this happens very frequently, so any operation you perform here impacts performance significantly.

    Called before ngOnInit() (if the component has bound inputs) and whenever one or more data-bound input properties change.

    Note that if your component has no inputs or you use it without providing any inputs, the framework will not call ngOnChanges().

* `ngOnInit()`
    Initialize the directive or component after Angular first displays the data-bound properties and sets the directive or component's input properties.

    Called once, after the first ngOnChanges(). ngOnInit() is still called even when ngOnChanges() is not (which is the case when there are no template-bound inputs).

* `ngAfterContentInit()`
    Respond after Angular projects external content into the component's view, or into the view that a directive is in.


* `ngAfterViewInit()`
    Respond after Angular initializes the component's views and child views, or the view that contains the directive.


* `ngOnDestroy()`
    Cleanup just before Angular destroys the directive or component. Unsubscribe Observables and detach event handlers to avoid memory leaks.

    Called immediately before Angular destroys the directive or component.


Examples:

```typescript
import { Component, OnInit, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})

export class HomeComponent implements OnInit, OnDestroy {
  constructor() { }

  ngOnInit() {
    console.log("component has been initialized!")
  }

  ngOnChanges(changes: SimpleChanges) {
    console.log(changes)
  }

  ngOnDestroy() { 
	  console.log("component has been destroyed!")
  }
}
```

### *End of session 9*