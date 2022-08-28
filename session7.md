### *Session 7*

# Create the cart view

For customers to see their cart, you can create the cart view in two steps:

* Create a cart component and configure routing to the new component.
* Display the cart items.

### Set up the cart component
To create the cart view, follow the same steps you did to create the `ProductDetailsComponent` and configure routing for the new component.

1. Generate a new component named `cart` in the terminal by running the following command:
    ```
    ng generate component cart
    ```
    This command will generate the `cart.component.ts` file and it associated template and styles files.

    `src/app/cart/cart.component.ts`
    ```typescript
    import { Component } from '@angular/core';

    @Component({
        selector: 'app-cart',
        templateUrl: './cart.component.html',
        styleUrls: ['./cart.component.css']
    })
    export class CartComponent {

    constructor() { }

    }
    ```
    StackBlitz also generates an `ngOnInit()` by default in components. You can ignore the `CartComponent` `ngOnInit()` for this tutorial.

2. Note that the newly created `CartComponent` is added to the module's `declarations` in `app-routing.module.ts`.

    `src/app/app-routing.module.ts`
    ```typescript
    import { CartComponent } from './cart/cart.component';
    ```
3. Still in `app-routing.module.ts`, add a route for the component `CartComponent`, with a path of cart.

    `src/app/app-routing.module.ts`
    ```typescript
    const routes: Routes = [
        { path: '', component: ProductListComponent },
        { path: 'products/:productId', component: ProductDetailsComponent },
        { path: 'cart', component: CartComponent } // add this line
    ];
    ```
4. Update the Checkout button so that it routes to the `/cart` URL. In `top-bar.component.html`, add a `routerLink` directive pointing to `/cart`.

    `src/app/top-bar/top-bar.component.html`
    ```html
    <a routerLink="/cart" class="button fancy-button">
        <i class="material-icons">shopping_cart</i>Checkout
    </a>
    ```

<br>

### Display the cart items
This section shows you how to use the cart service to display the products in the cart.

1. In `cart.component.ts`, import the `CartService` from the `cart.service.ts` file.

    `src/app/cart/cart.component.ts`
    ```typescript
    import { Component } from '@angular/core';
    import { CartService } from '../cart.service';
    ```

2. Inject the `CartService` so that the `CartComponent` can use it by adding it to the `constructor()`

    `src/app/cart/cart.component.ts`
    ```typescript
    export class CartComponent {


    constructor(
        private cartService: CartService
    ) { }
    }
    ```

3. Define the `items` property to store the products in the cart.
    `src/app/cart/cart.component.ts`
    ```typescript
    export class CartComponent {

    items = this.cartService.getItems();

    constructor(
        private cartService: CartService
    ) { }
    }
    ```

    This code sets the items using the `CartService` `getItems()` method. You defined this method when you created `cart.service.ts`.

4. Update the cart template with a header, and use a `<div>` with an `*ngFor` to display each of the cart items with its name and price. The resulting `CartComponent` template is as follows.

    `src/app/cart/cart.component.html`
    ```html
    <h3>Cart</h3>

    <div class="cart-item" *ngFor="let item of items">
        <span>{{ item.name }}</span>
        <span>{{ item.price | currency }}</span>
    </div>
    ```

5. Verify that your cart works as expected:
    * Click **My Store**
    * Click on a product name to display its details.
    * Click Buy to add the product to the cart.
    * Click Checkout to see the cart.

<br>

# Retrieve shipping prices

Servers often return data in the form of a stream. Streams are useful because they make it easy to transform the returned data and make modifications to the way you request that data. Angular `HttpClient` is a built-in way to fetch data from external APIs and provide them to your application as a stream.

This section shows you how to use `HttpClient` to retrieve shipping prices from an external file.

The application that StackBlitz generates for this guide comes with predefined shipping data in` assets/shipping.json`. Use this data to add shipping prices for items in the cart.

`src/assets/shipping.json`
```json
[
  {
    "type": "Overnight",
    "price": 25.99
  },
  {
    "type": "2-Day",
    "price": 9.99
  },
  {
    "type": "Postal",
    "price": 2.99
  }
]
```

## Configure **AppModule** to use `HttpClient`
To use Angular's `HttpClient`, you must configure your application to use `HttpClientModule`.

Angular's `HttpClientModule` registers the providers your application needs to use the `HttpClient` service throughout your application.

1. In `app.module.ts`, import `HttpClientModule` from the `@angular/common/http` package at the top of the file with the other imports. As there are a number of other imports, this code snippet omits them for brevity. Be sure to leave the existing imports in place.

    `src/app/app.module.ts`
    ```typescript
    import { HttpClientModule } from '@angular/common/http';
    ```

2. To register Angular's `HttpClient` providers globally, add `HttpClientModule` to the `AppModule` `@NgModule()` `imports` array.

    `src/app/app.module.ts`
    ```typescript
    @NgModule({
    imports: [
        BrowserModule,
        HttpClientModule,
        ReactiveFormsModule
    ],
    declarations: [
        AppComponent,
        TopBarComponent,
        ProductListComponent,
        ProductAlertsComponent,
        ProductDetailsComponent,
        CartComponent,
    ],
    bootstrap: [
        AppComponent
    ]
    })
    export class AppModule { }
    ```

## Configure **CartService** to use `HttpClient`

The next step is to inject the `HttpClient` service into your service so your application can fetch data and interact with external APIs and resources.

1. In `cart.service.ts`, import `HttpClient` from the `@angular/common/http` package.

    `src/app/cart.service.ts`
    ```typescript
    import { Injectable } from '@angular/core';
    import { HttpClient } from '@angular/common/http';
    import { Product } from './products';
    ```

2. Inject `HttpClient` into the `CartService` `constructor()`.
    `src/app/cart.service.ts`
    ```typescript
    export class CartService {
    items: Product[] = [];

    constructor(
        private http: HttpClient
    ) {}
    /* . . . */
    }
    ```

## Configure **CartService** to get shipping prices

To get shipping data, from `shipping.json`, You can use the `HttpClient` `get()` method.

1. In `cart.service.ts`, below the `clearCart()` method, define a new `getShippingPrices()` method that uses the `HttpClient` `get()` method.

    `src/app/cart.service.ts`
    ```typescript
    export class CartService {
        /* . . . */
        getShippingPrices() {
            return this.http.get<{type: string, price: number}[]>('/assets/shipping.json');
        }
    }
    ```

<br>

## ✅ Tasks to perform on `computer-store` project

✅ Create the cart view.

✅ Display all cart items using cart service

✅ Retrieve shipping prices

✅ Configure HTTP Client 

✅ Perofm all other steps followed in this session


### *End of session 7*

### [NEXT: Session8](session8.md)