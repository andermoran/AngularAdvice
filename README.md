
# Ander's Angular Adivce

## Basics

- Angular (currently on Angular 8) is a framework that allows for dynamic, reactive web applications as well as native mobile applications
- It is a combination of TypeScript (a subset of Javascript) and HTML
- The "main" file is `app.component.ts` which is a component itself which is loaded into the home html page, index.html

## Components
- Components are similar, one could even say analogous. to classes in OOP (Object Oriented Programming)
- Similar to objects, components are made to be reused in and it helps keep the LOC (lines of code) lower in the main file, `app.component.ts`

## Data Binding
 ### String Interpolation
- Allows you to communicate between your TypeScript code and HTML template in your component by using `[` and `]`
```html
<p>Hello, my name is  {{ name }}!</p>
```
- Where `name` is a variable in the corresponding TypeScript code
- The variable being interpolated into a string can be a `string`, `number`, `boolean`, etc. as long as it can be represented as a `string` in the end
#### [Example](https://stackblitz.com/edit/angular-cau2tt)
> Notice the `name` variable in `app.component.ts` and the `{{ name }}` in `app.component.html` 

### Property Binding
- Bind HTML attributes to TypeScript variables using `[`  and `]`
```html
<button [disabled]="buttonDisabled">Click me</button>
```
- Where `buttonDisabled` is a variable in a component file
- If `buttonDisabled` is `true`, then the button is disabled, if it is `false`, then the button is enabled
#### [Example](https://stackblitz.com/edit/angular-2knkts)
> Notice the `buttonDisabled` variable in `app.component.ts` and the `[disabled]="buttonDisabled"` in `app.component.html` . Try changing the value of `buttonDisabled` from `false` to `true`

### Event Binding
- Bind HTML DOM events such as `click()` to a TypeSript method using `(` and `)`
```html
<button (click)="onButtonClick()">Click me</button>
```
- Where `onButtonClick()` is a method in a component file
- So whenever `click()` fires, it triggers `onButtonClick()`
#### [Example](https://stackblitz.com/edit/angular-2knkts)
> Notice the `onButtonClick()` method in `app.component.ts` and the `(click)="onButtonClick()"` in `app.component.html` 
- *Note from [Maximilian Schwarzmüller](https://twitter.com/maxedapps):*
>  How do you know to which Properties or Events of HTML Elements you may bind? You can basically bind to all Properties and Events - a good idea is to  `console.log()` the element you're interested in to see which properties and events it offers
> **Important**: For events, you don't bind to onclick but only to click (=> (click))
> The MDN (Mozilla Developer Network) offers nice lists of all properties and events of the element you're interested in. Googling for  `YOUR_ELEMENT properties` or  `YOUR_ELEMENT events` should yield nice results
- Data can be passed with event by using the reserved `$event` keyword
- `$event` can be passed to the component method through the parameter in the HTML template
 ```html
 <input type="text" class="form-control" (input)="inputUpdated($event)"/>
 ```
- In our TypeScript code, we can reference this via 
```typescript
inputUpdated(event: Event) {
	this.myVar = event.target.value;
}
```
> TypeScript might complain about this syntax because it does not explicitly know that the event has properties we are assuming it to have. All TypeScript knows is that it is an event, not necessarily what kind

![TypeScript complaining about variable's vagueness](https://github.com/andermoran/AngularAdvice/blob/master/images/event_error.png)
> Notice how `value` is red underlined
> To fix this, we can be more explicit and type
```typescript
inputUpdated(event: Event) {
	this.myVar = (<HTMLInputElement>event.target).value;
}
```
> Now TypeScript knows that we know that the type of the `HTMLElement` of the event is `HTMLInputElement`

![TypeScript no longer complaining with variable and is now happy](https://github.com/andermoran/AngularAdvice/blob/master/images/event_error_fixed_explicit.png)
> Notice how `value` is no longer red underlined
#### [Example](https://stackblitz.com/edit/angular-9waen7)
### Two-Way Databinding
- *Note from [Maximilian Schwarzmüller](https://twitter.com/maxedapps):*
> #### Important: FormsModule is Required for Two-Way-Binding!
> Important: For Two-Way-Binding to work, you need to enable the  `ngModel` directive. This is done by adding the  `FormsModule` to the  `imports[]` array in the AppModule. 
> 
> You then also need to add the import from  `@angular/forms` in the app.module.ts file:
> `import { FormsModule } from '@angular/forms';`
> 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2Njc2NjE4MDMsLTE2OTk2NTQyMTEsLT
E3Mzc0MDQxNzQsLTE2NDA5NjY4OTksOTYyNjAwODk4LDEzNjUz
Mzc0MjUsLTE1ODc2NDAzODYsLTExNDQ2NDY3MTldfQ==
-->