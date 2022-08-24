
### *Session 2*

# HTML and its relationship between Angular

The HyperText Markup Language or HTML is the standard markup language designed to be displayed in a web browser. The pages which are created using HTML can be static or dynamic (interactive) and the collection of these pages together forms a Website or Web application. The browser is built with an ability to parse these HTML pages and renders them on the screen so users can view them and interact with them, an User Interface (UI).

A page designed using HTML or Web technology is called as Web page. A web page can also have CSS for styling the content and Javacript for any UI behavior / functionality. A Web application may have some tasks like DOM manipulation, Ajax requests, Animations etc and can be implemented using Javascript.

Angular is a Javscript framework and has lot of built-in libraries that helps building a Web application fast and easy. It let us maintain the entire HTML page split into different sections and save in separate folders and also provides built-in options to bind or render the data to be displayed within every HTML content. So to keep it simple, Angular makes it very easy to implement DOM manipulation, Ajax requests or Animations etc compared to traditional system. But when finally it is rendered on the browser, the application output will be nothing but just HTML content.

# Angular applications: The essentials
The core ideas behind Angular, understanding these can help you design and build your applications more effectively.


## <u> Components: </u>
Components are the building blocks that compose an application. A component includes a TypeScript class with a `@Component()` decorator, an HTML template, and styles. The `@Component()` decorator specifies the following Angular-specific information:

* A CSS selector that defines how the component is used in a template. HTML elements in your template that match this selector become instances of the component.

* An HTML template that instructs Angular how to render the component.

* An optional set of CSS styles that define the appearance of the template's HTML elements.

The following is a minimal Angular component.

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'hello-world',
  template: `
    <h2>Hello World</h2>
    <p>This is my first component!</p>
  `
})
export class HelloWorldComponent {
  // The code in this class drives the component's behavior.
}
```
To use this component, you write the following in a template:

```html
<hello-world></hello-world>
```
When Angular renders this component, the resulting DOM looks like this:
```html
<hello-world>
    <h2>Hello World</h2>
    <p>This is my first component!</p>
</hello-world>
```

Angular's component model offers strong encapsulation and an intuitive application structure. Components also make your application painless to unit test and can improve the overall readability of your code.

<br>

## <u> Templates: </u>

Every component has an HTML template that declares how that component renders. You define this template either inline or by file path.

Angular extends HTML with additional syntax that lets you insert dynamic values from your component. Angular automatically updates the rendered DOM when your component’s state changes. One application of this feature is inserting dynamic text, as shown in the following example.

```html
<p>{{ message }}</p>
```
The value for message comes from the component class:

```typescript
import { Component } from '@angular/core';

@Component ({
  selector: 'hello-world-interpolation',
  templateUrl: './hello-world-interpolation.component.html'
})
export class HelloWorldInterpolationComponent {
    message = 'Hello, World!';
}
```
When the application loads the component and its template, the user sees the following:

```html
<p>Hello, World!</p>
```
> Notice the use of double curly braces--they instruct Angular to interpolate the contents within them.

Angular also supports property bindings, to help you set values for properties and attributes of HTML elements and pass values to your application's presentation logic.

```html
<p
  [id]="sayHelloId"
  [style.color]="fontColor">
  You can set my color in the component!
</p>
```

> Notice the use of the square brackets--that syntax indicates that you're binding the property or attribute to a value in the component class.

Declare event listeners to listen for and respond to user actions such as keystrokes, mouse movements, clicks, and touches. You declare an event listener by specifying the event name in parentheses:

``` html
<button
  [disabled]="canClick"
  (click)="sayMessage()">
  Trigger alert message
</button>
```
The preceding example calls a method, which is defined in the component class:

```typescript
sayMessage() {
  alert(this.message);
}
```

The following is a combined example of Interpolation, Property Binding and Event Binding within an Angular template:

`hello-world-bindings.component.ts`
```typescript
import { Component } from '@angular/core';

@Component ({
  selector: 'hello-world-bindings',
  templateUrl: './hello-world-bindings.component.html'
})
export class HelloWorldBindingsComponent {
  fontColor = 'blue';
  sayHelloId = 1;
  canClick = false;
  message = 'Hello, World';

  sayMessage() {
    alert(this.message);
  }

}
```
`hello-world-bindings.component.html`
```html
<button
  [disabled]="canClick"
  (click)="sayMessage()">
  Trigger alert message
</button>
<p
  [id]="sayHelloId"
  [style.color]="fontColor">
  You can set my color in the component!
</p>
<p>My color is {{ fontColor }}</p>
```

Add additional functionality to your templates through the use of directives. The most popular directives in Angular are `*ngIf` and `*ngFor`. Use directives to perform a variety of tasks, such as dynamically modifying the DOM structure. And create your own custom directives to create great user experiences.

The following code is an example of the `*ngIf` directive.

`hello-world-ngif.component.ts`
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'hello-world-ngif',
  templateUrl: './hello-world-ngif.component.html'
})
export class HelloWorldNgIfComponent {
  message = 'I\'m read only!';
  canEdit = false;

  onEditClick() {
    this.canEdit = !this.canEdit;
    if (this.canEdit) {
      this.message = 'You can edit me!';
    } else {
      this.message = 'I\'m read only!';
    }
  }
}
```

`hello-world-ngif.component.html`
```html
<h2>Hello World: ngIf!</h2>

<button (click)="onEditClick()">Make text editable!</button>

<div *ngIf="canEdit; else noEdit">
    <p>You can edit the following paragraph.</p>
</div>

<ng-template #noEdit>
    <p>The following paragraph is read only. Try clicking the button!</p>
</ng-template>

<p [contentEditable]="canEdit">{{ message }}</p>
```

Angular's declarative templates let you cleanly separate your application's logic from its presentation. Templates are based on standard HTML, for ease in building, maintaining, and updating.

<br>

### *End of session 2*

### [NEXT: Session3](session3.md)