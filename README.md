# store-tutorial
Tutorial
The following tutorial shows you how to manage the state of a counter, and how to select and display it within an Angular component.

1.	Create a new file named counter.actions.ts to describe the counter actions to increment, decrement, and reset its value.
src/app/counter.actions.ts
import { createAction } from '@ngrx/store';

export const increment = createAction('[Counter Component] Increment');
export const decrement = createAction('[Counter Component] Decrement');
export const reset = createAction('[Counter Component] Reset');
2.	Define a reducer function to handle changes in the counter value based on the provided actions.
src/app/counter.reducer.ts
import { createReducer, on } from '@ngrx/store';
import { increment, decrement, reset } from './counter.actions';

export const initialState = 0;

export const counterReducer = createReducer(
  initialState,
  on(increment, (state) => state + 1),
  on(decrement, (state) => state - 1),
  on(reset, (state) => 0)
);
3.	Import the StoreModule from @ngrx/store and the counter.reducer file.
src/app/app.module.ts (imports)
import { StoreModule } from '@ngrx/store';
import { counterReducer } from './counter.reducer';
4.	Add the StoreModule.forRoot function in the imports array of your AppModule with an object containing the count and the counterReducer that manages the state of the counter. The StoreModule.forRoot() method registers the global providers needed to access the Store throughout your application.
src/app/app.module.ts (StoreModule)
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
 
import { AppComponent } from './app.component';
 
import { StoreModule } from '@ngrx/store';
import { counterReducer } from './counter.reducer';
 
@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, StoreModule.forRoot({ count: counterReducer })],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
5.	Create a new file called my-counter.component.ts in a folder named my-counter within the app folder that will define a new component called MyCounterComponent. This component will render buttons that allow the user to change the count state. Also, create the my-counter.component.html file within this same folder.
src/app/my-counter/my-counter.component.ts
import { Component } from '@angular/core';
import { Observable } from 'rxjs';
 
@Component({
  selector: 'app-my-counter',
  templateUrl: './my-counter.component.html',
})
export class MyCounterComponent {
  count$: Observable<number>
 
  constructor() {
    // TODO: Connect `this.count$` stream to the current store `count` state
  }
 
  increment() {
    // TODO: Dispatch an increment action
  }
 
  decrement() {
    // TODO: Dispatch a decrement action
  }
 
  reset() {
    // TODO: Dispatch a reset action
  }
}
src/app/my-counter/my-counter.component.html
<button (click)="increment()">Increment</button>

<div>Current Count: {{ count$ | async }}</div>

<button (click)="decrement()">Decrement</button>

<button (click)="reset()">Reset Counter</button>
6.	Add the new component to your AppModule's declarations and declare it in the template:
src/app/app.component.html
<app-my-counter></app-my-counter>
src/app/app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
 
import { AppComponent } from './app.component';
 
import { StoreModule } from '@ngrx/store';
import { counterReducer } from './counter.reducer';
import { MyCounterComponent } from './my-counter/my-counter.component';
 
@NgModule({
  declarations: [AppComponent, MyCounterComponent],
  imports: [BrowserModule, StoreModule.forRoot({ count: counterReducer })],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
7.	Inject the store into MyCounterComponent and connect the count$ stream to the store's count state. Implement the increment, decrement, and reset methods by dispatching actions to the store.
src/app/my-counter/my-counter.component.ts
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { Observable } from 'rxjs';
import { increment, decrement, reset } from '../counter.actions';
 
@Component({
  selector: 'app-my-counter',
  templateUrl: './my-counter.component.html',
})
export class MyCounterComponent {
  count$: Observable<number>;
 
  constructor(private store: Store<{ count: number }>) {
    this.count$ = store.select('count');
  }
 
  increment() {
    this.store.dispatch(increment());
  }
 
  decrement() {
    this.store.dispatch(decrement());
  }
 
  reset() {
    this.store.dispatch(reset());
  }
}
And that's it! Click the increment, decrement, and reset buttons to change the state of the counter.
Let's cover what you did:
•	Defined actions to express events.
•	Defined a reducer function to manage the state of the counter.
•	Registered the global state container that is available throughout your application.
•	Injected the Store service to dispatch actions and select the current state of the counter.
