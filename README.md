<img align="right" src="https://angular.io/assets/images/logos/angular/angular.png">

# Ander's Angular Adivce 


## Note
I am making this guide as I am learning Angular from [Maximilian Schwarzmüller's Angular Tutorial](https://www.udemy.com/the-complete-guide-to-angular-2/) as well as googling stuff online. Because I am writing this as I learn, there may be parts where I don't do things in the perfect manner but I think it's helpful to read through the eyes of someone who is still learning. Sometimes experts are so knowledgeable and deep into a subject to the point where they attempt to explain something and in their eyes it makes perfect sense, but to someone who is just starting out it is confusing due to the lack of a comprehensive understanding. I try my best to use words that anybody knows so that as many people as possible can understand what saying. If anything is in quotes, it means I copy and pasted something that was said by someone else. Some of the wording may come from Angulars documentation but I try to write everything myself as sometimes documentation isn't worded well. I am writing writing this tutorial using [stackedit](https://stackedit.io/) which has no spell checking so I'm sure I've misspelled a lot of things while writing this.

## Table of Contents
- Will do once finished

## Basics
- Angular (currently on Angular 8) is a framework that allows for dynamic, reactive web applications as well as native mobile applications
	- Not sure if I recommend for making native mobile apps yet
- It is a combination of TypeScript (a subset of Javascript) and HTML
> *Note: please view this page in Chrome and install the [Github + Mermaid](https://github.com/BackMarket/github-mermaid-extension) Chrome extension to view the graphs properly*

## Components
- Components are similar, one could even say analogous, to classes in OOP (Object Oriented Programming)
- Similar to objects, components are made to be reused in and it helps keep the LOC (lines of code) lower in the main file, `app.component.ts`
- The main component is `app`. Thinks of this like the mother of all components. Without this component, the app would have no functionality. Although the `app` component is **not** analogous to the `main` function in C or Java, imagine a program without a `main`. It has no functionality. That's how your Angular project would be with the `app` component.
- Components are usually made up of 3 files with an additional file used for testing
	1. `mycomponentname.component.ts`
		- This file controls the logic of the component. This is where your `TypeScript` code goes which controls what your component does.
	2. `mycomponentname.component.html`
		- This file controls how your component is structured on the page
	3. `mycomponentname.component.css`
		- This file controls how your component is styled on the page
	4. `mycomponentname.component.spec.ts`
		- This file is used for testing but I never use it so I'd just delete it

## Data Binding
 ### String Interpolation
- Allows you to communicate between your TypeScript code and HTML template in your component by using `[` and `]`
	```html
	<p>Hello, my name is {{ name }}!</p>
	```
- Where `name` is a variable in the corresponding TypeScript code
- The variable being interpolated into a string can be a `string`, `number`, `boolean`, etc. as long as it can be represented as a `string` in the end
- [Example](https://stackblitz.com/edit/angular-cau2tt)
	> Notice the `name` variable in `app.component.ts` and the `{{ name }}` in `app.component.html` 

### Property Binding
- Bind HTML attributes to TypeScript variables using `[` and `]`
	```html
	<button [disabled]="buttonDisabled">Click me</button>
	```
- Where `buttonDisabled` is a variable in a component file
- If `buttonDisabled` is `true`, then the button is disabled, if it is `false`, then the button is enabled
- In this case, the variable we bind is a `boolean`, but what if we bind a `string` or `number`?
	- For a `string` variable, if it is empty, then it is the equivalent of `false`, else it is `true`
	- For a `number` variable, if it is `0` then it is the equivalent of `false`, else it is `true`
- [Example](https://stackblitz.com/edit/angular-2knkts)
	> Notice the `buttonDisabled` variable in `app.component.ts` and the `[disabled]="buttonDisabled"` in `app.component.html` . Try changing the value of `buttonDisabled` from `false` to `true`

### Event Binding
- Bind HTML DOM events such as `click()` to a TypeScript method using `(` and `)`
	```html
	<button (click)="onButtonClick()">Click me</button>
	```
- Where `onButtonClick()` is a method in a component file
- So whenever `click()` fires, it triggers `onButtonClick()`
- [Example](https://stackblitz.com/edit/angular-2knkts)
	> Notice the `onButtonClick()` method in `app.component.ts` and the `(click)="onButtonClick()"` in `app.component.html` 
- *Note from [Maximilian Schwarzmüller](https://twitter.com/maxedapps):*
	> How do you know to which Properties or Events of HTML Elements you may bind? You can basically bind to all Properties and Events - a good idea is to `console.log()` the element you're interested in to see which properties and events it offers
	> **Important**: For events, you don't bind to onclick but only to click (=> (click))
	> The MDN (Mozilla Developer Network) offers nice lists of all properties and events of the element you're interested in. Googling for `YOUR_ELEMENT properties` or `YOUR_ELEMENT events` should yield nice results
- Data can be passed with event by using the reserved `$event` keyword
- `$event` can be passed to the component method through the parameter in the HTML template
	 ```html
	 <input type="text" class="form-control" (input)="inputUpdated($event)"/>
	 ```
- In our TypeScript code, we can reference this via 
	```typescript
	inputUpdated(event: Event) {
		console.log(event.target.value);
	}
	```
	> TypeScript might complain about this syntax because it does not explicitly know that the event has properties we are assuming it to have. All TypeScript knows is that it is an event, not necessarily what kind

![TypeScript complaining about variable's vagueness](https://github.com/andermoran/AngularAdvice/blob/master/images/Screen%20Shot%202019-09-03%20at%2011.24.51%20AM.png)
> Notice how `value` is red underlined. To fix this, we can be more explicit and type
```typescript
inputUpdated(event: Event) {
	console.log((<HTMLInputElement>event.target).value);
}
```
> Now TypeScript knows that we know that the type of the `HTMLElement` of the event is `HTMLInputElement`

![TypeScript no longer complaining with variable and is now happy](https://github.com/andermoran/AngularAdvice/blob/master/images/Screen%20Shot%202019-09-03%20at%2011.24.39%20AM.png)
> Notice how `value` is no longer red underlined
- [Example](https://stackblitz.com/edit/angular-9waen7)
> Try typing into the input field and looking at the console:

![Console showing "Hello" being logged](https://github.com/andermoran/AngularAdvice/blob/master/images/Screen%20Shot%202019-09-03%20at%2011.24.21%20AM.png)
> Notice how `(input)="inputUpdated($event)"` takes `$event` as a parameter, allowing `app.component.ts` to get information from `app.component.html`. Here we are passing information stored in `$event` in one direction (from `app.component.html` to `app.component.ts`) and logging it in `app.component.ts` via the `inputUpdated` method
```mermaid
graph LR
html["input field value (app.component.html)"] -- $event --> ts["handler method (app.component.ts)"]
```

### Two-Way Data Binding
- The two-way data binding enables "changes in the application state have been automagically reflected into the view and vise-versa"

> *Note from [Maximilian Schwarzmüller](https://twitter.com/maxedapps):*
> #### Important: FormsModule is Required for Two-Way-Binding!
> Important: For Two-Way-Binding to work, you need to enable the `ngModel` directive. This is done by adding the `FormsModule` to the `imports[]` array in the AppModule. 
> 
> You then also need to add the import from `@angular/forms` in the app.module.ts file:
> 
> `import { FormsModule } from '@angular/forms';`
- With two-way data binding, information is sent in *both* directions instead of only one
- [Example](https://stackblitz.com/edit/angular-pkgmpx)
> *Note: ngModel is a directive but you don't need to know what that is right now*

> Notice the `userInput` variable in `app.component.ts` and the `[(ngModel)]="userInput"` in `app.component.html`. In the previous example, if the value `userInput` is changed in the typescript code, then the value in the input field does not change. Why? Remember, the previous example was a *one* direction data binding so the value of the input field changed the value of `userInput`; however, if the value of `userInput` was changed outside of the input field, then the input field would not update. This is because the input field's value can only be changed if the user types keystrokes into it (like how it would normally work). In our *two* directional example, the value of the input field is tied to the value of `userInput`. So if `userInput` changes, then the value inside the text field changes and vice versa. Try changing the value of the textfield by typing into it, this changed the value of `userInput`. Now click the `Set to "Hello, World!" button` and see how the value of the input field changed to "Hello, World!". When this button is clicked, TypeScript code is run that sets the value of `userInput` to "Hello, World!". Since `userInput` is bound to the input field value, the value of the input field is then set to "Hello, World!".
```mermaid
graph LR
html["input field value (app.component.html)"] --> ts["variable (app.component.ts)"]
ts["variable (app.component.ts)"] --> html["input field value (app.component.html)"]
```
- Looking at the diagram above, you can see that a change to either the input field value *or* the variable associated with `ngModel` will change the other.

### Binding to Custom Properties
> To use enable custom property bindings for a component, you need to import `Input` from `@angular/core`. Your import statement should look like `import { Input } from '@angular/core';`
- Enables us to bind to properties, similar to how we did before, but for our own custom components
- To add a bindable property to our own components we do so by creating a variable like we normally do but add `@Input()` in front of it
- An example of this declaration would be `@Input() highlighted: boolean`
- Say my component is named `person` and I want to bind to the highlighted property, then I can simply do
	```html
	<app-person [highlighted]="true"></app-person>
	```
- [Example](https://stackblitz.com/edit/angular-xg2lee)
> This example shows how we pass a `boolean` to the `highlighted` property of the person component. This `highlighted` property is then handled by `person.component.html` where it uses `ngClass` to determine whethere to apply the css class or not
- You can also specify another name for the property if you do not want to use the variable name for binding
	- This other name acts as an alias for binding
- What if instead we want to refer to the property as `hili` when we bind to it because we're lazy and do not want to type highlighted?
- Simply insert `hili` in between the parentheses after `Input` like so `@Input("hili") highlighted: boolean`
- Now we can refer bind to the `highlighted` property under the alias `hili`
	```html
	<app-person [hili]="true"></app-person>
	```
- [Example](https://stackblitz.com/edit/angular-8i8qtm)
> Note: Once you create an alias for a custom property, you can no longer bind to the original property name so `<app-person [highlighted]="true"></app-person>` would not work anymore because angular recognizes `hili` in place of `highlighted` when binding


### Binding to Custom Events
> To use enable custom event bindings for a component, you need to import `Output` and `EventEmitter` from `@angular/core`. Your import statement should look like `import { Output, EventEmitter } from '@angular/core';`
- Custom events are used by the child class to send a message to the parent class whenever something happens
- In the child class, you must create an `EventEmitter` and put `Output` in front of it
	- `@Output() created = new EventEmitter<>();`
- If you want to pass data from the child to parent class, put parameters between `<` and `>`
	- `@Output() created = new EventEmitter<{name: string}>();`
	- This passes a `string` variable called `name`
- Next step is to let the parent class know when this event happens, so if our `EventEmitter` variable is called `created` and we want our parent class to know when the child class component calls `ngOnInit` we do
	```typescript
	ngOnInit() {
		this.created.emit();
	}
	```
- To pass data with this event we do
	```typescript
	ngOnInit() {
		this.created.emit({name: this.name});
	}
	```
	> This assumes the child class has a variable called `name` which it wishes to pass to the parent class
- The event will be emitted but the parent class is not listening for the event so this effectively does nothing
```mermaid
graph LR
child --EventEmitter--> nothing[ ]
style nothing fill:#FFFFFF, stroke:#FFFFF;
```
- To make the parent class listen for the event, we have to bind an event to our custom component like so
	```typescript
	<app-person (created)="showAlert($event)"></app-person>
	```
	```mermaid
	graph LR
	child --EventEmitter--> parent
	```
	> Right here we are binding the `created` event we made in our child component to the method `showAlert(personData: {name: string})` in our parent component. We pass `$event`. With this binding is in place, whenever `this.created.emit({name: this.name});` runs in the child component, an event is emitted to the parent component causing `showAlert(personData: {name: string})` to run as well.
Think about how we binded to click earlier. Whenever the person clicks the child component, the child lets the parent component that it was clicked and triggers an action in the parent component.
- [Example](https://stackblitz.com/edit/angular-xmgq8g)

## Directives
- "Directives are instructions in the DOM"
- Example directive:
	```html
	<p appTurnGreen>Receives a green background!</p>
	```
### Structural Directives
- "Structural directives are responsible for HTML layout. They shape or reshape the DOM's *structure*, typically by adding, removing, or manipulating elements."
- Must be prefixed with `*`
#### ngIf
- Used to decide whether to display an element or not
	- This is known as **conditional rendering**
- As long as whatever is in between the `"` and `"` is `true` (e.g. variable, method, expression), then the element will be displayed
```html
<p *ngIf="password.length >= 5">{{ password }} is a valid password</p>
```
> This paragraph tag will only display if `password.length >= 5` evaluates to `true`. `password` is a TypeScript variable.
- [Example](https://stackblitz.com/edit/angular-tnzfsf)
- What if we want to display one thing if something is `true` and another thing if something is `false`? Is there an `ngElse`?
	- Kind of
- You have two options
	1. Writing the same line of code but reverse the boolean
		```html
		<p *ngIf="password.length >= 5">{{ password }} is a valid password</p>
		<p *ngIf="password.length < 5">{{ password }} is too short</p>
		```
	2. Using `ng-template`
		```html
		<p *ngIf="password.length >= 5; else passwordTooShort">{{ password }} is a valid password</p>
		<ng-template #passwordTooShort>
		<p #passwordTooShort>password too short</p>
		</ng-template>
		```
	3. There are a few more ways to achieve conditional rendering [here](https://ultimatecourses.com/blog/angular-ngif-else-then#ngIf_and_Else)
- [Example](https://stackblitz.com/edit/angular-l7ueby)
	> Notice how if the password is less than 5 character you see `password too short` but once it is at least five characters it says `(whatever you typed) is a valid password`
#### ngFor
- Loops through an array to display multiple elements
```html
<p *ngFor="let person of people">{{ person }}</p>
```
> Where `people` is an array in the component
- [Example](https://stackblitz.com/edit/angular-2v9dzs)
	> Notice the `people` array. `*ngFor="let person of people"` means loop through each element in the array `people` and at each index assign the value of the array at that index to `person`
- To get the index of the element in the array with ngFor assign the index to a variable
```html
<p *ngFor="let person of people; let i = index">person #{{ i }} {{ person }}</p>
```
### Attribute Directives
- These directives do not add or remove elements, only change the element it is used on
#### ngStyle
- Used to dynamically apply CSS styles to elements
- In between `"` and `"` you should have a javascript object
- You can use this in a multiple ways as long as there is a javascript object between the quotes. Here are a couple examples
	1. Using a function that returns a javascript object
		```typescript
		getColor() {
			return {backgroundColor: 'green', color: 'white'};
		}
		```
		```html
		<p [ngStyle]="getColor()">Hello, world!<p>
		```
	2. Directly passing the javascript object
		```html
		<p [ngStyle]="{backgroundColor: 'green', color: 'white'}">Hello, world!<p>
		```
- [Example](https://stackblitz.com/edit/angular-p6hawb)
#### ngClass
- Dynamically add or remove CSS classes to elements
- You can have it apply/remove the class conditionally or unconditionally
	1. Conditionally
		```html
		<p [ngClass]="{valid: password.length >= 5}">Password status<p>
		```
		> If the css class name has a `-` (e.g. `valid-password`) then you need to place single quotes around the class name `<p [ngClass]="{'valid-password': password.length >= 5}">Password status<p>`
	2. Uncondtionally
		```html
		<p [ngClass]="'valid'">Password status<p>
		```
	> where `valid` is a css class defined in the .css file associated with a component.
- **Make sure to notice the nested quotes in the 2nd example**
- [Example](https://stackblitz.com/edit/angular-mnkems)

#### [Example using ngIf, ngFor, ngClass, and ngStyle](https://stackblitz.com/edit/angular-nxsnso)

## View Encapsulation
- CSS styles for a given component only effect the host component, not the children it creates
- Let's say the `App` component contains a `Person` component in `app.component.html`. If `app.component.css` changes the style of `p` tags and the `Person` component contains `p` tags, the `p` tags in `Person` will not be affected. Only `p` tags directly in `app.component.html` will be altered.
- [Example](https://stackblitz.com/edit/angular-g3bfc4)
- Different view encapsulation behaviors
	1. Emulated
		- This is the default view behavior. Only styles directly associated with a component will affect said component. A componenet with be affected by a style **if and only if** that component directly imports the style sheet into its TypeScript file.
	2. ShadowDom
		- Uses Shadow DOM to encapsulate styles
	3. None
		- "Don't provide any template or style encapsulation." This means if a component contains another component, then each component's style sheets will bleed into the other component; however, if either component has already defined a style for a particular tag, it particular style will **not** be overwritten by the other component's style
	4. Native (deprecated)
		- Use ShadowDom instead

- [Example](https://stackblitz.com/edit/angular-mwt3mm)
- In the example above, try commenting and uncommenting lines 6, 7, and 8 in `app.component.ts`. View the differences with each view encapsulation option.
	1. Emulated
		- Notice how "Hello, my name is John Doe" (which is an `h1` tag) is black even though in `app.component.css` we see
			```css
			h1 {
				color: red;
			}
			```
			Why is this? Because we have chosen the `Emulated` option, all of the styles are contained with their respective component. Since "Hello, my name is John Doe" is a part of the `Person` component and **not** the `App` component it does not respond because we have specified in `app.component.ts` that we want to encapsulate our view via the line `encapsulation: ViewEncapsulation.Emulated`
	2. ShadowDom
		- The difference between the results using `ShadowDom` and `None` is that with `ShadowDom` you see the black box border around the elements that is specified in `app.component.css` with
			 ```css
			:host {
				display: block;
				border: 1px solid black;
			}
			```
			This is because `None` ignores the shadow DOM. I'm not too familiar with the shadow DOM (I literally just learned about it while learning about view encapsulation) but I will update this part once I can show a better example
	3. None
		- "Hello, my name is John Doe" is now red because we decided **not** to encapsulate the `App` component styles. So the styles are applied to all the components that `App` uses is `app.component.html`. Since there is an `h1` style in `app.component.css`, it is passed to the `Person` component
		- *But wait...* there is also
			 ```css
			p {
				color: blue;
			}
			```
			In `app.component.css` so why isn't "I have a pet named Fido" blue? This is because in `person.component.css` we have
			 ```css
			p {
				color: green;
			}
			```
			This style sheet takes precedence over the other component's style sheet so "I have a pet named Fido" is green
- To use view encapsulation, you need to import `ViewEncapsulation` with 
	```typescript
	import { ViewEncapsulation } from '@angular/core'
	```
- The encapsulation tags go inside `@Component({ })` like so
	```typescript
	@Component({
		selector: 'my-app',
		templateUrl: './app.component.html',
		styleUrls: ['./app.component.css'],
		encapsulation: ViewEncapsulation.None
	})
	```

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTU4MDIzNjg1LDU1ODAyMzY4NSwtMTY5Mj
A0ODAxNSwtMTM3ODg1NDM3NSwtOTA5NDk0NzA3LDg1ODUzMDQ4
MCwtMTE5NjM5OTk2LC00ODc5OTg2MDUsNzU5MzM5NzAzLC0xNT
UxOTIxMDc1LC00NzY5MTA3NDcsLTEwNzA3MTA5MzMsMTE0NzMw
OTI3NCwtMTkwNDI5NTA1MSwxOTMzOTEyOTc0LC0xNDQ0MDMyMT
AsODE2MjAxMTc4LC04MzI4OTUxODMsMTQyMTg5NjI5MCw4NzEy
MzQyNjJdfQ==
-->