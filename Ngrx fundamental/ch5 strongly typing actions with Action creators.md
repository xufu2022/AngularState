# strongly typing actions with Action creators

Actions express unique events that happen throughout your application

```ts
// example action
{       //event source and category          Action to happen
    type: ‘     [Products Page]          Toggle Show Product Code’,
                //Optional metadata
    products: [{ id: 1, name : ‘Hammer’ }]
}
```

## Rules to Writing Good Actions

- Write them upfront 
- Divide by event source 
- Many specific actions
- Capture events not commands
- Descriptive debug logs

## Two Action Creator Helper Functions

```ts
import { createAction} from ‘@ngrx/store’;
createAction();
createActionGroup();

export const loadProducts = createAction(‘[Products Page] Load Products’);

//Action Creator - createAction
export const productsLoadedSuccess = createAction(
    ‘[Products API] Products Loaded Success’
    props<{ products: Product[] }>()
);

export const productsLoadedFail = createAction(
    ‘[Products API] Products Loaded Fail’
    props<{ message: string }>
);

// Action Creator - createActionGroup
export const ProductsPageActions = createActionGroup({
    source: ‘Products Page’,
    events: { ‘Load Products’: emptyProps(),},
});

export const ProductsAPIActions = createActionGroup({
    source: ‘Products API’,
    events: {
        ‘Products Loaded Success’: props<{ products: Product[] }>(),
        ‘Products Loaded Fail’: props<{ message: string }>()
    },
});

// Using Action Creators
import { ProductPageActions } from ‘../state/products.actions’;

toggleShowProductCode() {
    this.store.dispatch(ProductPageActions.loadProducts());
}

```