# Working with Effects

> Why use effects?

- Add @ngrx/effects
- Define an effect
- Register an effect
- Use an effect
- Exception handling in effects

Effects are an RxJS powered side effect model for NgRx Store.

## Effects Keep Components Pure

```ts
export const productsReducer = createReducer(
    initialState,
    on(createAction(ProductsPageActions.loadProducts), (state) => ({
        ...state,
        loading: true,
    }))
);
```

## Benefits of Effects

- Keep components pure 
- Isolate side effects 
- Easier to test

```ts
// Defining an Effect
@Injectable()
export class ProductEffects {
    constructor(private actions$: Actions, private productsService: ProductsService) {}
    loadProducts$ = createEffect(() =>
        this.actions$.pipe(
            ofType(ProductsPageActions.loadProducts),
            concatMap(() => this.productsService.getAll().pipe(
                map((products) => ProductsAPIActions.productsLoadedSuccess({ products })),
                catchError(
                    (error) => of(ProductsAPIActions.productsLoadedFail({ message: error}))
            ))));

// Exception Handling in Effects
    catchError(
        (error) => of(ProductsAPIActions.productsLoadedFail({ message: error})
    )

// Exception Handling in Reducer
export interface ProductsState {
    showProductCode: boolean;
    loading: boolean;
    products: Product[];
    errorMessage: string;
}
const initalState: ProductsState = {
    showProductCode: true,
    loading: false,
    products: [],
    errorMessage: ‘’
};

export const productsReducer = createReducer(
    initalState,
        on(ProductsAPIActions.productsLoadedFail, (state, { message }) => ({
            errorMessage: message,
})));

```

Warning – Possible Race Conditions

> concatMap : Runs subscriptions/requests in order and is less performant
            Use for get, post and put requests when order is important

> mergeMap : Runs subscriptions/requests in parallel 
            Use for get, put, post and delete methods when order is not important

> switchMap : Cancels the current subscription/request
            Use for get requests or cancelable requests like searches

> exhaustMap : Ignores all subsequent subscriptions/requests until complete
             Use when you do not want more requests until the initial one completes