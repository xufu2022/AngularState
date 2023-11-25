# Manage Local State with Component Store

- Why and when use component-store?
    - Simpler single file
    ComponentStore is a standalone library that helps to manage local/component state

- Add @ngrx/component-store 
- @ngrx/component-store vs @ngrx/store
    - Independent states
    - Independent state
    - Similar NgRx API


> Key Concept

- Local state must be initialized
- Local state is typically tied to a single components lifecycle
- Update state via setState or an updater 
- Read state with selectors
- Manage side effects with effects

```ts
    @Injectable()
    export class ProductsStore extends ComponentStore<ProductsState> {
        constructor(private productsService: ProductsService) {
        super({ products: [] });
    }
    addProducts = this.updater((state, products: Product[]) => ({
        ...state,
        products,
    }));

    getProducts = this.effect<void>((trigger$) =>
        trigger$.pipe(exhaustMap(() => this.productsService.getAll().pipe(
            tap({ next: (products) => this.addProducts(products)})
    ))));

    // store 
    products$ = this.select((state) => state.products);

    // Component Store
    @Component({
    selector: ‘app-products-page’    ,
    templateUrl: ‘./products-page.component.html’    ,
    styleUrls: [‘./products-page.component.css’],
    providers: [ProductsStore],
    })
    export class ProductsPageComponent {
    products$ = this.productsStore.products$;
    constructor(private productsStore: ProductsStore) {}
    ngOnInit() {
        this.productsStore.getProducts();
    }
    }
```