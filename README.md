# Shiba Commerce

## Introduction
Shiba is our headless approach for Magento 2 to make developer easier for a frontend as you not dealing with the native clunky frontend provided with Magento 2 instead you are given a frontend that is using [Vue.js](https://vuejs.org/)

It uses a set of other libraries ontop of Vue to provide a full Single Page Application approach where all data is sent through the frontend to Laravel acting as a middleman to:

* Handle authentication.
* Handle session management.
* Cache data such as catalog content or configuration.
* Format data **TO** and **FROM** Magento *Such as configurable option data*.

## The frontend 
The frontend is created using Vue and SASS which means we are using a modern frontend with such libraries/tools as:
* Routing via [Vue Router](https://router.vuejs.org/)
* State Management via [Vuex](https://vuex.vuejs.org/)
* JS/SCSS Compliation handled by [Laravel Mix](https://laravel-mix.com/) which is a wrapper around webpack.
* [Axios](https://github.com/axios/axios) for Promise based HTTP requests.
* Animation in JS using [Anime.js](https://animejs.com/).

## The backend
The backend is a combination of Laravel providing the middleman which interacts with Magento. This interacts with Magento via GrahpQL calls which allows data to be more flexible interms of what we request from Magento e.g. certain types of product data. 

Laravel acts as an API for the frontend so that `Vue` can communicate with it via `axios`. 

Laravel also acts as a caching layer which means it will store: 
* Store Configuration
* Catalog Data
* Any other data that. 

There are also cron jobs that will flush all of the stored cache on then refetch everything via the server.

## Useful tools
* GraphQL UI for testing [Download Here](https://electronjs.org/apps/graphiql)
* Magento 2 GraphQL [Documentation](https://devdocs.magento.com/guides/v2.3/graphql/)
