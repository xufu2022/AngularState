# NgRx Fundamentals Introduction

## Types of State?

-   Persisted state
-   URL state 
-   Client state 
-   Local UI state

## Purpose of NgRx

Provide a framework for building reactive applications in Angular
- Managing global and local state
- Isolation of side effects
- Entity collection management
- Integration with the Angular Router
- Developer tooling

## State Lifecycle – Toggle Show Product Code

1.  Component
2.  Action
3.  Reducer
4.  Store

```ts
{type: '[Products Page] Toggle Show Product Code'}
```

## NgRx Store Implements The Redux Pattern

-   There is only one single source of truth for application state called the store
-   State is read-only and the only way to change state is to dispatch an action
-   Changes to the store are made using pure functions called reducers

## Use NgRx To Manage State When...

-   Your application has a lot of user interactions
-   Your application has a lot of different data sources
-   Managing state in services is no longer sufficient

## The SHARI Principle

1. Shared: state that is accessed by many components and services.
1. Hydrated: state that is persisted and rehydrated from external storage.
1. Available: state needs to be available when re-entering routes.
1. Retrieved: state that must be retrieved with a side effect.
1. Impacted: state that is impacted by actions from other sources.

## Key Concepts of NgRx

- Type safety 
- Immutability and performance 
- Encapsulation
- Serializable 
- Testable
