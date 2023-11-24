# Angular State Management with NgRx

- NgRx core concepts 
- Basic NgRx implementation
- Asynchronous handling with NgRx effects
- Advanced NgRx patterns

## NgRx Core Concepts

Actions : Actions are payloads of information that send data from the application to the Redux store.
Reducers : Reducers are functions that take the current state and an action as arguments and return a new state
Effects : Effects are responsible for interacting with external parts of your system

Redux Store : The Redux Store is the object that brings together the state, actions, and reducers in a Redux or NgRx application

## Basic NgRx Implementation


Asynchronous Handling with NgRx Effects

```ts
//action
export const loadProducts = createAction("[Product] Load products");
export const loadProductsSuccess = createAction("[Product] Load products success", props<{products:any[]}>());
export const loadProductsFailure = createAction("[Product] Load products failure", props<{error:any}>());

// reducer
import * as ProductActions from '../actions/product.actions';
export const initialState: any[] = [];

export const productReducer = createReducer(
    initialState,
    on(ProductActions.loadProductsSuccess, (state, {products}) => products),
    on(ProductActions.loadProductsFailure, (state, {error})=> state)
)

// effects
export class ProductEffects {

    loadData$ = createEffect(() => 
    this.actions$.pipe(
        ofType(ProductActions.loadProducts),
        mergeMap(() => 
            this.productService.getProducts().pipe(
                map(products => ProductActions.loadProductsSuccess({products})),
                catchError(error => [ProductActions.loadProductsFailure({error})])
            )
        )
    ))

    constructor(private actions$: Actions,
        private productService: ProductService){}
}

//register
StoreModule.forRoot({home: homeReducer, products: productReducer}),
EffectsModule.forRoot([ProductEffects])

export class ProductsComponent {
    products$: Observable<any> | undefined;
    constructor(
        // private productService: ProductService, 
        private store: Store<{products:any}>,
        private router: Router) {
        this.products$ = store.select('products');
        }

    ngOnInit(): void {

        this.store.dispatch(ProductActions.loadProducts())

        // this.productService.getProducts().subscribe(data => {
        //   this.products = data;
        //   console.log('Products: ', this.products);
        // });
    }
```

[Notes]: 
Flow example:
-   A user clicks a button in a component to load data, triggering the component to dispatch a LoadData action.
-   An effect listens for the LoadData action. Upon receiving it, the effect makes an HTTP request to fetch data
-   Once the data is fetched, the effect dispatches a new action, such as LoadDataSuccess, with the fetched data as payload.
-   A reducer listens for the LoadDataSuccess action and updates the state with the new data

 actions and effects work together to handle state changes and side effects in a structured and predictable way. Actions are the mechanisms that trigger state changes, and effects handle the side effects of those actions, often resulting in new actions being dispatched to update the state.

Advanced Techniques in NgRx
    Meta-reducers
    Feature creators
    Actions groups

```ts
//Meta-reducers
export function logger(reducer: ActionReducer<any>): ActionReducer<any> {
    return (state, action: Action) => {
        const newState = reducer(state, action);

        console.group('Logger Meta-Reducer');
        console.log('Previous state: ', state);
        console.log('Action: ', action);
        console.log('New state: ', newState);
        console.groupEnd();

        return newState;
    }
}

export const metaReducers: MetaReducer<any>[] = [logger];
StoreModule.forRoot({home: homeReducer, products: productReducer}, {metaReducers}),
```

The introduction of feature creators in NgRx represents a significant step towards more functional and less boilerplate-heavy state management in Angular applications. By leveraging these creators, developers can write more concise, readable, and maintainable code for managing the application state.
```ts
//action
export const loadItems = createAction('[Items] Load Items');
export const loadItemsSuccess = createAction('[Items] Load Items Success', props<{ items: Item[] }>());
//reducer
export const itemsReducer = createReducer(
  initialState,
  on(loadItems, state => ({ ...state, loading: true })),
  on(loadItemsSuccess, (state, { items }) => ({ ...state, items, loading: false }))
);
//effects
loadItems$ = createEffect(() =>
  this.actions$.pipe(
    ofType(loadItems),
    mergeMap(() => this.itemsService.getItems().pipe(
      map(items => loadItemsSuccess({ items }))
    ))
  )
);
//selector
export const selectItemsState = createFeatureSelector<ItemsState>('items');
export const selectAllItems = createSelector(selectItemsState, (state) => state.items);

```