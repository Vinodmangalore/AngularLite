### *Session 6*

# Managing data

At this stage of development, the store application has a product catalog with two views: a product list and product details. Users can click on a product name from the list to see details in a new view, with a distinct URL, or route.

This step guides you through creating a shopping cart in the following phases:

* Update the product details view to include a **Buy** button, which adds the current product to a list of products that a cart service manages.

* Add a cart component, which displays the items in the cart.

* Add a shipping component, which retrieves shipping prices for the items in the cart by using Angular's `HttpClient` to retrieve shipping data from a `.json` file.

<br>

# Create the shopping cart service

In Angular, a service is an instance of a class that you can make available to any part of your application using Angular's dependency injection system.

Currently, users can view product information, and the application can simulate sharing and notifications about product changes.

The next step is to build a way for users to add products to a cart. This section walks you through adding a Buy button and setting up a cart service to store information about products in the cart.

### Define a cart service
This section walks you through creating the `CartService` that tracks products added to shopping cart.

1. In the terminal generate a new `cart` service by running the following command:

    ```
    ng generate service cart
    ```

2. Import the `Product` interface from `./products.ts` into the `cart.service.ts` file, and in the `CartService` class, define an `items` property to store the array of the current products in the cart.

    `src/app/cart.service.ts`
    ```typescript
    import { Product } from './products';
    /* . . . */
    export class CartService {
        items: Product[] = [];
    /* . . . */
    }
    ```
3. Define methods to add items to the cart, return cart items, and clear the cart items.

    `src/app/cart.service.ts`
    ```typescript
    export class CartService {
    items: Product[] = [];
    /* . . . */

    addToCart(product: Product) {
        this.items.push(product);
    }

    getItems() {
        return this.items;
    }

    clearCart() {
        this.items = [];
        return this.items;
    }
    /* . . . */
    }
    ```
    * The `addToCart()` method appends a product to an array of `items`.

    * The `getItems()` method collects the items users add to the cart and returns each item with its associated quantity.

    * The `clearCart()` method returns an empty array of items, which empties the cart.

<br>

### Use the cart service

This section walks you through using the `CartService` to add a product to the cart.

1. In `product-details.component.ts`, import the cart service.

    `src/app/product-details/product-details.component.ts`
    ```typescript
    import { Component, OnInit } from '@angular/core';
    import { ActivatedRoute } from '@angular/router';

    import { Product, products } from '../products';
    import { CartService } from '../cart.service';
    ```

2. Inject the cart service by adding it to the `constructor()`.

    `src/app/product-details/product-details.component.ts`
    ```typescript
    export class ProductDetailsComponent implements OnInit {

    constructor(
        private route: ActivatedRoute,
        private cartService: CartService
    ) { }
    }
    ```

3. Define the `addToCart()` method, which adds the current product to the cart.

    `src/app/product-details/product-details.component.ts`
    ```typescript
    export class ProductDetailsComponent implements OnInit {

        addToCart(product: Product) {
            this.cartService.addToCart(product);
            window.alert('Your product has been added to the cart!');
        }
    }
    ```

    The `addToCart()` method does the following:

    * Takes the current `product` as an argument.
    * Uses the `CartService` `addToCart()` method to add the product to the cart.
    * Displays a message that you've added a product to the cart.

4. In `product-details.component.html`, add a button with the label **Buy**, and bind the `click()` event to the `addToCart()` method. This code updates the product details template with a **Buy** button that adds the current product to the cart.

    `src/app/product-details/product-details.component.html`
    ```html
    <h2>Product Details</h2>

    <div *ngIf="product">
        <h3>{{ product.name }}</h3>
        <h4>{{ product.price | currency }}</h4>
        <p>{{ product.description }}</p>
        <button (click)="addToCart(product)">Buy</button>
    </div>
    ```

5. Verify that the new **Buy** button appears as expected by refreshing the application and clicking on a product's name to display its details.

6. Click the **Buy** button to add the product to the stored list of items in the cart and display a confirmation message.

<br>

### *End of session 6*