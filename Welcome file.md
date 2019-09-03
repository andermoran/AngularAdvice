
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
- Allows you to communicate between your TypeScript code and HTML template in your component
```
<p>Hello, my name is  {{ name }}!</p>
```
- Where `name` is a variable in the corresponding TypeScript code
- The variable being interpolated into a string can be a `string`, `number`, `boolean`, etc. as long as it can be represented as a `string` in the end
- [Example](https://stackblitz.com/edit/angular-cau2tt)
> Notice the `name` variable in `app.component.ts` and the `{{ name }}` in `app.component.html` 

### Property Binding
- Bind HTML attributes to variables
```
<button [disabled]="buttonDisabled">Click me</button>
```
- Where `buttonDisabled` is a variable in a component file
- If `buttonDisabled` is `true`, then the button is disabled, if it is `false`, then the button is enabled
- [Example](https://stackblitz.com/edit/angular-2knkts)
> Notice the `buttonDisabled` variable in `app.component.ts` and the `[disabled]="buttonDisabled"` in `app.component.html` . Try changing the value of `buttonDisabled` from `false` to `true`

### Event Binding
- Bind HTML DOM events such as `click()` to a method
```
<button (click)="onButtonClick()"/>
```
- Where `onButtonClick()` is a method in a component file
- So whenever `click()` fires, it triggers `onButtonClick()`
- Can pass data with event by using the reserved `$event` keyword
- `$event` can be passed to the component method through the parameter in the HTML template
 ```
 <input type="text" class="form-control" (input)=“inputUpdated($event)”/>
 ```
- In our TypeScript code, we can reference this via 
```
inputUpdated(event: Event) {
	this.myVar = event.target.value;
}
```
- TypeScript might complain about this syntax because it does not explicitly know that the event has properties we are assuming it to have. To fix this, we canbe more explicit and type
```
inputUpdated(event: Event) {
	this.myVar = (<HTMLInputElement>event.target).value;
}
```
> Now TypeSript knows that we know that the type of the `HTMLElement` of the event is `HTMLInputElement`
- 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzNzYwNDQ1OSwtNjU4NjQ2ODcyLC0xNz
ExODI4NDQxXX0=
-->