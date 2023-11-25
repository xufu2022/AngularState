# Listen to Router State with Router Store

Why use router store?
- Router actions 
- Router selectors 
- Reduce router logic in components

> Navigation Actions

-   ROUTER_REQUEST 
    - At the start of each navigation
-   ROUTER_NAVIGATION 
    - During navigation, before any guards or resolvers run
-   ROUTER_NAVIGATED
    - After successful navigation
-   ROUTER_CANCEL 
    - When navigation is cancelled
-   ROUTER_ERROR
    - When there is an error during navigation

```ts
export const {
    selectCurrentRoute, // select the current route
    selectFragment, // select the current route fragment
    selectQueryParams, // select the current route query params
    selectQueryParam, // factory function to select a query param
    selectRouteParams, // select the current route params
    selectRouteParam, // factory function to select a route param
    selectRouteData, // select the current route data
    selectRouteDataParam, // factory function to select a route data param
    selectUrl, // select the current url
    selectTitle, // select the title if available
} = getRouterSelectors();

@NgModule({
imports: [
    StoreModule.forRoot({router: routerReducer}),
    StoreRouterConnectingModule.forRoot()
],})
```
Add and use @ngrx/router-store
Initialize loading data
Navigating in effects

## ngrxOnInitEffects lifecycle hooks

Router Store adds bindings to connect the Angular Router with NgRx Store

