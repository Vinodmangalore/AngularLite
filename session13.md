### *Session 13*

# Display a selection list
In this page, you'll expand the Tour of Heroes application to display a list of heroes, and allow users to select a hero and display the hero's details.

## Create mock heroes
You'll need some heroes to display.

Eventually you'll get them from a remote data server. For now, you'll create some mock heroes and pretend they came from the server.

Create a file called `mock-heroes.ts` in the `src/app/` folder. Define a `HEROES` constant as an array of ten heroes and export it. The file should look like this.

`src/app/mock-heroes.ts`
```typescript
import { Hero } from './hero';

export const HEROES: Hero[] = [
  { id: 11, name: 'Dr Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```
## Displaying heroes
Open the `HeroesComponent` class file and import the mock `HEROES`

`src/app/heroes/heroes.component.ts (import HEROES)`
```typescript
import { HEROES } from '../mock-heroes';
```
In the same file (`HeroesComponent` class), define a component property called heroes to expose the `HEROES` array for binding

`src/app/heroes/heroes.component.ts`
```typescript
export class HeroesComponent implements OnInit {

  heroes = HEROES;
}
```

## List heroes with ***ngFor**
Open the HeroesComponent template file and make the following changes:
* Add an `<h2>` at the top,
* Below it add an HTML unordered list (`<ul>`)
* Insert an <`li`> within the <`ul`> that displays properties of a `hero`.
* Sprinkle some CSS classes for styling (you'll add the CSS styles shortly).

Make it look like this:
`heroes.component.html (heroes template)`
```html
<h2>My Heroes</h2>
<ul class="heroes">
  <li>
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```
That displays an error since the property 'hero' does not exist. To have access to each individual hero and list them all, add an *`ngFor` to the `<li>` to iterate through the list of heroes:

```html
<li *ngFor="let hero of heroes">
```
The *`ngFor` is Angular's repeater directive. It repeats the host element for each element in a list.

The syntax in this example is as follows:
* `<li>` is the host element.
* `heroes` holds the mock heroes list from the `HeroesComponent` class, the mock heroes list.
* `hero` holds the current hero object for each iteration through the list.

>Don't forget the asterisk (*) in front of ngFor. It's a critical part of the syntax.

After the browser refreshes, the list of heroes appears.

## Style the heroes
The heroes list should be attractive and should respond visually when users hover over and select a hero from the list.

In the `first tutorial`, you set the basic styles for the entire application in `styles.css`. That stylesheet didn't include styles for this list of heroes.

You could add more styles to `styles.css` and keep growing that stylesheet as you add components.

You may prefer instead to define private styles for a specific component and keep everything a component needs— the code, the HTML, and the CSS —together in one place.

This approach makes it easier to re-use the component somewhere else and deliver the component's intended appearance even if the global styles are different.

You define private styles either inline in the `@Component.styles` array or as stylesheet file(s) identified in the `@Component.styleUrls` array.

When the CLI generated the `HeroesComponent`, it created an empty `heroes.component.css` stylesheet for the `HeroesComponent` and pointed to it in `@Component.styleUrls` like this.

`src/app/heroes/heroes.component.ts (@Component)`
```typescript
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
```
Open the `heroes.component.css` file and paste in the private CSS styles for the `HeroesComponent`. You'll find them in the `final code review` at the bottom of this guide.

>Styles and stylesheets identified in @Component metadata are scoped to that specific component. The heroes.component.css styles apply only to the HeroesComponent and don't affect the outer HTML or the HTML in any other component.

## Viewing details
When the user clicks a hero in the list, the component should display the selected hero's details at the bottom of the page.

In this section, you'll listen for the hero item click event and update the hero detail.

### Add a click event binding
Add a click event binding to the `<li>` like this:

`heroes.component.html (template excerpt`
```html
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```
This is an example of Angular's `event binding` syntax.

The parentheses around `click` tell Angular to listen for the `<li>` element's `click` event. When the user clicks in the `<li>`, Angular executes the `onSelect(hero)` expression.

In the next section, define an `onSelect()` method in `HeroesComponent` to display the hero that was defined in the `*ngFor `expression.

### Add the click event handler
Rename the component's `hero` property to `selectedHero` but don't assign it. There is no selected hero when the application starts.

Add the following `onSelect`() method, which assigns the clicked hero from the template to the component's `selectedHero`.

`src/app/heroes/heroes.component.ts (onSelect)`
```typescript
selectedHero?: Hero;
onSelect(hero: Hero): void {
  this.selectedHero = hero;
}
```
### Add a details section
 Currently, you have a list in the component template. To click on a hero on the list and reveal details about that hero, you need a section for the details to render in the template. Add the following to `heroes.component.html` beneath the list section:

 `heroes.component.html (selected hero details)`
```html
<h2>{{selectedHero.name | uppercase}} Details</h2>
<div><span>id: </span>{{selectedHero.id}}</div>
<div>
  <label for="hero-name">Hero name: </label>
  <input id="hero-name" [(ngModel)]="selectedHero.name" placeholder="name">
</div>
```
After the browser refreshes, the application is broken.

Open the browser developer tools and look in the console for an error message like this:

```
HeroesComponent.html:3 ERROR TypeError: Cannot read property 'name' of undefined
```
### What happened?

When the application starts, the `selectedHero` is `undefined` by design.

Binding expressions in the template that refer to properties of `selectedHero`—expressions like `{{selectedHero.name}}`—must fail because there is no selected hero.

### The fix - hide empty details with **ngIf*
The component should only display the selected hero details if the `selectedHero` exists.

Wrap the hero detail HTML in a `<div>`. Add Angular's *ngIf directive to the `<div>` and set it to selectedHero.

>Don't forget the asterisk (*) in front of ngIf. It's a critical part of the syntax.

`src/app/heroes/heroes.component.html (*ngIf)`
```html
<div *ngIf="selectedHero">

  <h2>{{selectedHero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{selectedHero.id}}</div>
  <div>
    <label for="hero-name">Hero name: </label>
    <input id="hero-name" [(ngModel)]="selectedHero.name" placeholder="name">
  </div>

</div>
```
After the browser refreshes, the list of names reappears. The details area is blank. Click a hero in the list of heroes and its details appear. The application seems to be working again. The heroes appear in a list and details about the clicked hero appear at the bottom of the page.

### Why it works
When `selectedHero` is undefined, the `ngIf` removes the hero detail from the DOM. There are no `selectedHero` bindings to consider.

When the user picks a hero, `selectedHero` has a value and `ngIf` puts the hero detail into the DOM.

###  Style the selected hero
To help identify the selected hero, you can use the `.selected` CSS class in the `styles you added earlier`. To apply the `.selected` class to the `<li>` when the user clicks it, use class binding.
![Demo app](./img/heroes-list-selected.png)

Angular's `class binding` can add and remove a CSS class conditionally. Add `[class.some-css-class]="some-condition" `to the element you want to style.

Add the following [class.selected] binding to the `<li>` in the HeroesComponent template:

`heroes.component.html (toggle the 'selected' CSS class)`
```html
[class.selected]="hero === selectedHero"
```
When the current row hero is the same as the selectedHero, Angular adds the selected CSS class. When the two heroes are different, Angular removes the class.

The finished `<li>` looks like this:

`heroes.component.html (list item hero)`
```html
<li *ngFor="let hero of heroes"
  [class.selected]="hero === selectedHero"
  (click)="onSelect(hero)">
  <span class="badge">{{hero.id}}</span> {{hero.name}}
</li>
```
# Summary
* The Tour of Heroes application displays a list of heroes with a detail view.
* The user can select a hero and see that hero's details.
* You used `*ngFor` to display a list.
* You used` *ngIf `to conditionally include or exclude a block of HTML.
* You can toggle a CSS style class with a `class` binding.

# Create a feature component
At the moment, the `HeroesComponent` displays both the list of heroes and the selected hero's details.

Keeping all features in one component as the application grows will not be maintainable. You'll want to split up large components into smaller sub-components, each focused on a specific task or workflow.

In this page, you'll take the first step in that direction by moving the hero details into a separate, reusable `HeroDetailComponent`.

The `HeroesComponent` will only present the list of heroes. The `HeroDetailComponent` will present details of a selected hero.

## Make the HeroDetailComponent
Use the Angular CLI to generate a new component named hero-detail.
```
ng generate component hero-detail
```

The command scaffolds the following:

* Creates a directory `src/app/hero-detail`.

Inside that directory four files are generated:

* A CSS file for the component styles.
* An HTML file for the component template.
* A TypeScript file with a component class named `HeroDetailComponent`.
* A test file for the `HeroDetailComponent` class.

The command also adds the `HeroDetailComponent` as a declaration in the `@NgModule` decorator of the `src/app/app.module.ts` file.

## Write the template
Cut the HTML for the hero detail from the bottom of the `HeroesComponent` template and paste it over the generated boilerplate in the `HeroDetailComponent` template.

The pasted HTML refers to a `selectedHero`. The new `HeroDetailComponent` can present any hero, not just a selected hero. So replace "selectedHero" with "hero" everywhere in the template.

When you're done, the `HeroDetailComponent` template should look like this:
`src/app/hero-detail/hero-detail.component.html`
```html
<div *ngIf="hero">

  <h2>{{hero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{hero.id}}</div>
  <div>
    <label for="hero-name">Hero name: </label>
    <input id="hero-name" [(ngModel)]="hero.name" placeholder="name">
  </div>

</div>
```

## Add the @Input() hero property
The `HeroDetailComponent` template binds to the component's `hero` property which is of type `Hero`.

Open the `HeroDetailComponent` class file and import the `Hero` symbol.

`src/app/hero-detail/hero-detail.component.ts (import Hero)`
```javascript
import { Hero } from '../hero';
```

The `hero` property _`must be an Input property`_, annotated with the `@Input()` decorator, because the _external_ `HeroesComponent will bind to it` like this.
```html
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```
Amend the `@angular/core` import statement to include the `Input` symbol.

`src/app/hero-detail/hero-detail.component.ts (import Input)`
```typescript
import { Component, OnInit, Input } from '@angular/core';
```
Add a `hero` property, preceded by the `@Input()` decorator.

`src/app/hero-detail/hero-detail.component.ts`
```typescript
@Input() hero?: Hero;
```

That's the only change you should make to the `HeroDetailComponent` class. There are no more properties. There's no presentation logic. This component only receives a hero object through its `hero` property and displays it.

## Show the HeroDetailComponent

The `HeroesComponent` used to display the hero details on its own, before you removed that portion of the template. This section guides you through delegating logic to the `HeroDetailComponent`.

The two components will have a parent/child relationship. The parent `HeroesComponent` will control the child `HeroDetailComponent` by sending it a new hero to display whenever the user selects a hero from the list.

You won't change the `HeroesComponent` _class_ but you will change its _template_.

## Update the HeroesComponent template

The `HeroDetailComponent` selector is `'app-hero-detail'`. Add an `<app-hero-detail>` element near the bottom of the `HeroesComponent` template, where the hero detail view used to be.

Bind the `HeroesComponent.selectedHero` to the element's `hero` property like this.

`heroes.component.html (HeroDetail binding)`
```html
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```

`[hero]="selectedHero"` is an Angular `property binding`.

It's a _one way_ data binding from the `selectedHero` property of the `HeroesComponent` to the `hero` property of the target element, which maps to the `hero` property of the `HeroDetailComponent`.

Now when the user clicks a hero in the list, the `selectedHero` changes. When the `selectedHero` changes, the _property binding_ updates `hero` and the `HeroDetailComponent` displays the new hero.

The revised `HeroesComponent` template should look like this:

`heroes.component.html`
```html
<h2>My Heroes</h2>

<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>

<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```
The browser refreshes and the application starts working again as it did before.

## What changed?
As before, whenever a user clicks on a hero name, the hero detail appears below the hero list. Now the `HeroDetailComponent` is presenting those details instead of the `HeroesComponent`.

Refactoring the original `HeroesComponent` into two components yields benefits, both now and in the future:

1. You reduced the `HeroesComponent` responsibilities.

2.  You can evolve the `HeroDetailComponent` into a rich hero editor without touching the parent `HeroesComponent`.

3. You can evolve the `HeroesComponent` without touching the hero detail view.

4. You can re-use the `HeroDetailComponent` in the template of some future component.

## Summary
* You created a separate, reusable HeroDetailComponent.
* You used a property binding to give the parent HeroesComponent control over the child HeroDetailComponent.
* You used the @Input decorator to make the hero property available for binding by the external HeroesComponent.

### *End of session 13*