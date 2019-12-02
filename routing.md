# Routing
Routing is handled by `vue-router` the router is stored in: `middleman/resources/js/shiba/router/index.js`

While routes are defined in `middleman/resources/js/shiba/router/routes.js`

## Lazy loading pages
Some of the routes are using code splitting in place to have `.vue` files load on demand for some of the pages which have their routes harded coded in the `routes.js` file. 

 
```js 
/** Customer Flow */
let register = () => import(/* webpackChunkName: "register" */ '@shiba/pages/account/register.vue');
let dashboard = () => import(/* webpackChunkName: "dashboard" */ '@shiba/pages/account/dashboard.vue');
```

The example shows the register and the dashboard page being set as webpack chunks which result in `register.js` and `dashboard.js` which is automatically loaded in when imported but is seperated from the main `theme.js` payload.

Lazy loading pages should be done where the route of the page will not be dynamic such as:

* Account URLS
* Checkout Flow

## Dynamic Pages
While for catalog pages / CMS Pages we use an api call upon navigation to call our `resolver` from the `vuex` store which will return a relevant payload. 

This happens when there is no match found within the `routes.js` which will fall back a match for `*` which will then call a `routeScan` within `middleman/resources/js/shiba/router/route-check.js`

The result of which will be an object containing `type` & `payload` for example: 

```json
{
    payload: {
        id: 1,
        sku: "simple01",
        name: "Simple Product",
        type_id: "simple"
    },
    type: "product"
}
```

Based on the type this will be passed into a switch statement so that we have stored the current payload on the server for later rendering the relevant page.

```js
store.dispatch('resolve', route.path).then((response) => {
    if(response.type == 'product') {
        resolve(componentMap(route, 'product', response.payload));
        return;
    }

    if (response.type == 'category') {
        resolve(componentMap(route, 'category', response.payload));
        return;
    }

    if(response.type == 'error' || response.type == 'not_found'){
        resolve(componentMap(route, '404' , response.payload));
        return;
    }
}).catch((err) => {
    resolve(componentMap(route, '404' ,{}));
});
```

This would then call a function called `componentMap` which will check the payload content and type and then provide the relevant vue component which in itself is just another `switch` statement however more comparison of the data can be done.  The data is also added to the `vuex` within the `resolve` action. This can be found in `middleman/resources/js/shiba/store/index.js`.

Since the `routeScan` function is a promise we are calling `resolve` to then return the relevant component and then assigning it the `routeCheck` function we have bound to `vue-router`

```js
routeScan(to).then((component) => {
    to.matched[0].components.default = component;
    /** Clear global payload */
    globals.payload.type = null;
    globals.payload.payload = null;
    next();
});
```

This is the core logic that helps us have dynamic URLs within Magento and Wordpress which forms the core of Navigation within Shiba.

## Checkout flow
The checkout flow only contains 3 routes:
* `/checkout/cart` - Cart Page
* `/checkout` - Checkout Page
* `/checkout/success` - Order Success Page

These are hardcoded in the `routes.js` file. The navigation between `cart -> checkout` is handled by the user, while `checkout -> order success` is handled by the response of placing an order. 

These 3 pages are having their page components imported via code splitting meaning their logic will not come with the initial page load. Which will result in a lighter page load.