# Application Load Process

When a the frontend is loaded it is given a basic configuration object provided from the Laravel Middleman, this object is assigned to the window and is also exported as a global module. 

```js
    let globals = {};
    globals = window.Globals;

    export default globals;
```

This means data such as the customer's cart, login state and current page payload can be acessed within Javascript.

Some of the data we do not want to persist within navigation so we assign some of it to `Vuex` state data on when the store is generated and then that data is then nullified so it can be populated with other data moving to another page or to just have that data serve as a means of telling the application what page we are on. As we rely on Magento's URL Resolver for that information.


## Binding current Payload
Whenever we visit a page we do not know what type of page we are visiting so we do not know what component to render as all URL's are looked up on the server, the frontend does not now until that data and type of returned from the server.

This triggers `vue router` to perform a `beforeRouteCheck` which is defined in `middleman/resources/js/shiba/router/route-check.js` 

This is stored on the `globals.payload`.
Which contains two properties one containg the type of resource we are expecting. Such as a `product`,`category`,`page` or `not_found`.

This is then stored on the state while the component responsible for handling the content type is passed back to router.

Which is just a simple switch statement contained in `middleman/resources/js/shiba/router/component-map.js`
```js
export default (route, type, payload) => {
    switch (type) {
        case 'product':
            return productPage;

        case 'category':
            return categoryPage;

        case 'page':
            return handlePage(payload);

        case '404':
            return page404;

        default:
            break;
    }
};
```

## Resolver 
Whenever the user navigates to a page that isn't defined in a hardcoded route such as `/simple-product` or `/turmeric-yellow-sweater`

This is passed into the resolver which returns a payload of `type` & `content`. 
The resolver call on the backend will make a call to Magento first as `catalog` pages take priority. If nothing can be found then it will then use Wordpress as a fallback. 

The use of a resolver means that we don't have to worry seeing if the URL is a product or a category on the frontend we can let the server do the look up for us.

The resolver is also called on initial pageload based on the users URL. Which also returns the same payload object. 

But in this instance it will be bound to the `Globals` object. 