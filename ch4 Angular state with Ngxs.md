# Core Concept

- Store
- Actions : Actions in NGXS are simple classes with a type property. They are dispatched from components or services to signal state changes. Actions in NGXS carry the information needed to make a state change, such as payload data
- State
- Selects

## Setting up NGXS in Angular

```ts
npm install @ngxs/store --save

//actions
export class Show {
    static readonly type = '[Home] Show';
}
export class Hide {
    static readonly type = '[Home] Hide';
}

// state
@State<boolean>({
    name:'home',
    defaults: false
})

export class HomeState{

    @Action(Show)
    show(ctx: StateContext<boolean>){
        ctx.setState(true);
    }

    @Action(Hide)
    hide(ctx: StateContext<boolean>){
        ctx.setState(false);
    }

}

export class HomeComponent {

  // isShowMore: boolean = false;

  isShowMore$ = this.store.select(state => state.home);

  constructor(private store: Store){}
  
  showMore(){
    // this.isShowMore = true;
    this.store.dispatch(new Show());
  }

  showLess(){
    // this.isShowMore = false;
    this.store.dispatch(new Hide());
  }

}
```
## NGXS State Management with Services in Angular

```ts
//action
export class LoadProducts{
    static readonly type = '[Product] Load';
}

export class LoadProductsSuccess {
    static readonly type = '[Product] Load Success';
    constructor(public payload: any[]){}
}

export class LoadProductsFailure {
    static readonly type = '[Product] Load Failure';
    constructor(public payload: any){}
}
//state
export interface ProductStateModel{
    products: any[],
    error: any,
    loading: boolean
}

@State<ProductStateModel>({
    name: 'products',
    defaults: {
        products: [],
        loading: false,
        error: null
    }
})

@Injectable()
export class ProductState{
    constructor(private productService: ProductService){}

    @Action(LoadProducts)
    loadProducts(ctx: StateContext<ProductStateModel>){
        ctx.patchState({loading: true});

        return this.productService.getProducts().pipe(
            tap((result) => {
                ctx.dispatch(new LoadProductsSuccess(result))
            }),
            catchError((error) => {
                ctx.dispatch(new LoadProductsFailure(error));
                return of(error);
            })
        )
    }

    @Action(LoadProductsSuccess)
    loadProductsSuccess(ctx: StateContext<ProductStateModel>, action: LoadProductsSuccess){
        ctx.patchState({
            products: action.payload,
            loading: false
        })
    }

    @Action(LoadProductsFailure)
    loadProductsFailure(ctx: StateContext<ProductStateModel>, action: LoadProductsFailure){
        ctx.patchState({
            error: action.payload,
            loading: false
        })
    }


}
import { NgxsModule } from '@ngxs/store';
import { HomeState } from './state-ngxs/state/home.state';
import { ProductState } from './state-ngxs/state/product.state';
NgxsModule.forRoot([HomeState, ProductState])
//products$: Observable<any>;
  constructor(
    private productService: ProductService, 
    private store: Store,
    private router: Router) {
      this.products$ = this.store.select(state => state.products.products);
    }
  ngOnInit(): void {
    // this.productService.getProducts().subscribe(data => {
    //   this.products = data;
    //   console.log('Products: ', this.products);
    // });
    this.store.dispatch(new LoadProducts());
  }
```
The tap operator in RxJS is a utility operator that is primarily used for performing side effects or debugging within an Observable pipeline. It's important to understand that tap does not transform the data passing through the Observable stream; instead, it allows you to "tap into" the stream to perform actions or side effects, such as logging or modifying external state, without affecting the data flow itself.

Here are some of the main uses of the tap operator:

1. Debugging:

    1. One of the most common uses of tap is to debug an Observable pipeline. You can insert tap at any point in an Observable chain to inspect the values that are being emitted without altering them.
Example: observable.pipe(tap(value => console.log(value)))
2.  Triggering Side Effects:

    1. tap can be used to perform actions that should not alter the emitted values but are necessary as a part of the overall logic. This can include updating a user interface, sending a notification, logging, etc.
Example: observable.pipe(tap(value => this.logService.logValue(value)))

3.  Interacting with External State:

    1. While RxJS promotes a functional reactive programming style where state is managed within the Observable stream, sometimes you might need to interact with external state. tap allows you to do this without disrupting the Observable's data flow.
Example: observable.pipe(tap(value => this.externalState.update(value)))
Performing Actions Conditionally:

4.  tap can also be used to perform actions conditionally based on the emitted value, without affecting the value itself.
Example: observable.pipe(tap(value => { if (value.condition) this.performAction(); }))
Performance Monitoring:


You can use tap to measure performance or track the timing of certain operations without affecting the operation itself.

Example: observable.pipe(tap(() => console.time("operation")), /* some operations */, tap(() => console.timeEnd("operation")))

It's important to note that tap should be used judiciously, especially when interacting with external state or performing actions that have side effects. Overuse of tap for state management or critical logic can lead to code that's hard to follow and maintain. In general, tap is most appropriately used for debugging, logging, and other non-critical side activities that do not form the core of your application's logic.


## NGXS Plugins
In NGXS, plugins are used to add extra functionalities or extend the behavior of the NGXS store

- NGXS Plugins :
    - CLI
    - Logger
    - DevTools
    - Storage
    - Form
    - Web Socket
    - Router
    - HMR

## Logger plugin

```ts
npm install @ngxs/logger-plugin --save

    NgxsModule.forRoot([HomeState, ProductState]),
    NgxsLoggerPluginModule.forRoot()

```