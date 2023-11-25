# Managing Collections with Entity Adapter

Why use entity adapter?
- Reduce boilerplate 
- Performant CRUD operations 
- Type-safe selectors

Entity State adapter are for managing record collections

Add @ngrx/entity 

Use entity adapter:
- Interface and collection methods
- Selectors
- Update types in actions

```ts
// What is an Entity?
export interface Product {
id: number;
name: string;
price: number;
} 
const products: Product[] = [
{ id: 1, price: 3, name: ‘Hammer’ },
{ id: 2, price: 2, name: ‘Nails’ },
{ id: 3, price: 4, name: ‘Spanner’ }
];
// Products State
export interface ProductsState {
    showProductCode: boolean;
    loading: boolean;
    errorMessage: string;
    products: Product[];
}

// Selectors
export const selectProductById = createSelector(
    selectProducts,
    selectRouteParams,
    (products, { id }) => products.find((product) => product.id === parseInt(id))
);
// Reducer
on(ProductsAPIActions.productUpdatedSuccess, (state, { product }) => ({
    ...state,
    loading: false,
    products: state.products.map((existingProduct) => existingProduct.id === product.id ? product : existingProduct
),
})),

// Products Dictionary vs Array
export interface ProductsState {
    showProductCode: boolean;
    loading: boolean;
    errorMessage: string;
    products: Product[];
}

export interface ProductsState {
    showProductCode: boolean;
    loading: boolean;
    errorMessage: string;
    ids: string[] | number[];
    entities: Dictionary<Product>;
}

// Entity Interfaces - EntityState<T>
export interface ProductsState extends EntityState<Product> {
    showProductCode: boolean;
    loading: boolean;
    errorMessage: string;
    // ids: string[] | number[];
    // entities: Dictionary<Product>;
}

// Entity Adapter – createEntityAdapter
import { createEntityAdapter, EntityAdapter } from ‘@ngrx/entity’;
export const adapter: EntityAdapter<Product> = createEntityAdapter<Product>({});

export const adapter: EntityAdapter<Product> = createEntityAdapter<Product>({
    selectId,
    sortByName
});
export function selectId(product: Product): string {
    return product.productId;
};
export function sortByName(a: Product, b: Product): number {
    return a.name.localeCompare(b.name);
};

// Entity Adapter – getInitalState
const intitialState: ProductsState = adapter.getInitialState({
    showProductCode: true,
    loading: false,
    errorMessage: ‘’
});

// With EntityAdapter - Reducers

on(ProductsAPIActions.productUpdatedSuccess, (state, { update }) => ({
    adapter.updateOne(update, {
        ...state,
        loading: false,
}))
```

EntityAdapter - Collection Methods

| Adapter Method | Description |
|----------|----------|
| addOne | Add one entity to the collection.|
| addMany | Add multiple entities to the collection.|
| setAll | Replace current collection with provided collection.|
| setOne | Add or Replace one entity in the collection.|
| setMany | Add or Replace multiple entities in the collection.|
| removeOne | Remove one entity from the collection.|
| removeMany | Remove multiple entities from the collection, by id or by predicate.|
| removeAll | Clear entity collection.|
| updateOne | Update one entity in the collection. Supports partial updates.|
| updateMany | Update multiple entities in the collection. Supports partial updates.|
| upsertOne | Add or Update one entity in the collection.|
| upsertMany | Add or Update multiple entities in the collection.|
| mapOne | Update one entity in the collection by defining a map function.|
| map | Update multiple entities in the collection by defining a map function.|

```ts
// Selectors
export const selectProductById = createSelector(
    selectRouteParams,
    selectProducts,
    (products, { id }) => products.find((product) => product.id === parseInt(id))
);

const { 
selectAll,
selectEntities,
selectIds,
selectTotal
} = adapter.getSelectors();

export const selectAllProducts = selectAll;
export const selectProductEntities = selectEntities;
export const selectProductIds = selectIds;
export const selectProductTotal = selectTotal;

// With EntityAdapter - Selectors

export const selectProductById = createSelector(
    selectRouteParams,
    selectProductEntities,
    ({ id }, entities) => entities[id])
);

// Actions
export const ProductsAPIActions = createActionGroup({
    source: ‘Products API’,
    events: {
    ‘Product Updated Success’: props<{ product: Product }>(),
},
});

export const ProductsAPIActions = createActionGroup({
    source: ‘Products API’,
    events: {
    ‘Product Updated Success’: props<{ update: Update<Product> }>(),
},
});

```

