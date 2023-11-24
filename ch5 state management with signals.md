# Angular State Management with Signals

## Introduction to Signals in Angular

Signal: A signal is just a special type of variable that holds a value which also provides notification when the variable value changes

Why Signals?
- Reactive 
- Thread-safe 
- Efficient

Where Can We Use Signals?

- Components
- Directives
- Services
- Templates
- Anywhere else

## Creating and Reading Signals
```ts
import { Component, signal } from '@angular/core';
  isShowMoreWithSignal = signal(false);
    showMore(){
    // this.isShowMore = true;
    this.isShowMoreWithSignal.set(true);
  }

  showLess(){
    // this.isShowMore = false;
    this.isShowMoreWithSignal.set(false);
  }
```

## Computed Signals
```ts
  clickCount = signal(0);
  clickCountComputed = computed(() => this.clickCount()*2);

  showMore(){
    // this.isShowMore = true;
    this.isShowMoreWithSignal.set(true);

    this.clickCount.update((value) => value + 1);
  }

  showLess(){
    // this.isShowMore = false;
    this.isShowMoreWithSignal.set(false);
  }
```

## Using Services with Signals

```ts
// services
allproducts= signal<any[]>([]);
 this.http.get<any>(`${this._baseUrl}/products`).(response=>{this.allproducts.set(response);},error=>{console.log("error")});

```

## summary

-   Introduction to signals
-   Creating and reading signals
    - signal({…})
-   Computed signals
    - computed()
-   Using services with signals