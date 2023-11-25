# Selecting State with Selectors

- Why use selectors
- Creating selectors
- Updating the demo to use selectors

Selectors are pure functions used for obtaining slices of store state

> Benefits of Selectors

- Portability 
- Memoization 
- Composition
- Testability 
- Type Safety

```ts
// Two Create Selector Helper Functions
mport { createSelector} from ‘@ngrx/store’;
createSelector();
createFeatureSelector();

// Store State
{
    products: {
        showProductCode: boolean;
        loading: boolean;
        products: Product[];
    },
    auth: {
        userId: number;
        favouriteProducts: number[];
    }
}

// Selecting Typed State

export interface ProductsState {
    showProductCode: boolean;
    loading: boolean;
    products: Product[];
}
export interface AuthState {
    userId: number;
    favouriteProducts: number;
}
export interface AppState {
    products: ProductState;
    auth: AuthState;
}
export const selectProductsState = (state: AppState) => state.products;

// Selecting Feature State
export const selectProductsState = createFeatureSelector<ProductsState>(‘products’);

// Composing Selectors
export const selectProducts = createSelector(
    selectProductsState,
    (productsState) => productsState.products;
);

// Aggregating Data
export const selectProductsTotal = createSelector(
    selectProducts,
    (products) => sumProducts(products); //sumProducts
);

// Composing 2 Feature Slices of State

export const selectProducts = createSelector(
    selectProductsState,
    (state) => state.products;
);
export const selectFavouriteProducts = createSelector(
    selectAuthState,
    (state) => state.favouriteProducts;
);
export const selectFavouriteProducts = createSelector(
    selectProducts,
    selectFavouriteProducts,
    (products, favouriteProducts) => products.filter(
        (product) => favouriteProducts.includes(product.id)
    );
);

// Using Selectors in Components
products$ = this.store.select(selectProducts);

```
QA: why FeatureSelector? 


