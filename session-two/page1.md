# Typescript

What is it?
> A typed superset of Javascript that compiles to plain Javascript.

> TypeScript is a compile-to-JavaScript language that brings compile-time type checks, 
> classical object-oriented programming patterns, and powerful typing features to JavaScript.

https://www.typescriptlang.org/
 


There are actually plenty of languages out there that are supsets of Javascript.  There has got to be something that is missing?

Transpiled Languages Add Features

> One substantial advantage to transpiled languages is that they can add features. 
> This is one of TypeScript's main selling points, as it adds interfaces, abstract classes, algebraic data types, 
> and other features to JavaScript, which we wouldn't be able to use otherwise.

> The transpiled language implements a feature in raw JavaScript, 
> and lets you work with the interface without having to worry about the details. 
> Truth is, we do this even when we write normal JavaScript: We still use ES6-to-ES5 
> transpilers to write to use JavaScript's newest features, since the relatively small number of 
> people using browsers with reliable ES6 support is . . . Well, people like us.

https://scotch.io/tutorials/why-you-shouldnt-be-scared-of-typescript

So we get stronger type checking, which leads to less bugs, and more confidence in our refactorings.

Other Links:

*  https://vsavkin.com/writing-angular-2-in-typescript-1fa77c78d8e8#.ie24yt6w4

So why does angular2 use Typescript?

Google was going to have its own Language "AtScript" to include the features they wanted on top of vanilla js.
But they teamed with Microsoft and got what they wanted in the language.  

A good link for background is here:

https://www.infoq.com/articles/Angular2-TypeScript-High-Level-Overview

 
# Types

From personal experience, I found the lack of type checking in Javascript a bit unsettling at first.  Java has it, and the previous
language I used at Nortel (Protel) was so strongly typed that if you gave three engineers the same task...they would probibly write
the exact same code. It really does help prevent bugs in your code. Both by catching it as you are writing it in you IDE but also just
in general readability.

But, the nice thing is it is like the cub scouts.  
You can dress the full part, but you can also go to meetings in just the shirt and hat, or just the hat.

## Basic types

You can now declare types when you define your variables.

```typescript
var name: string
```

we can use them in functions as well

```typescript
function sayHello(name: string): string {
 return "Hello " + name;
}
```

If you use the results of this function as, say, a number. Typescript has the means to let you know otherwise.

## using TSC
Create a directory.  Lets just make sure that we have what we need.

```bash
$ tsc -v
Version 2.1.5
```

Cooking with gas.

Create the file bad1.ts

```typescript
function sayHello(name: string): string {
 return "Hello " + name;
}

var result:number = sayHello("Cat");
```
As you can see, the result is expected to be a number, but the function returns a string.

Now compile it.
```bash
$ tsc *.ts
bad1.ts(5,5): error TS2322: Type 'string' is not assignable to type 'number'.
```

You can also open the folder in visual code and see the errors from your IDE.


## Built in Types

### String
Your basic String.
  
```typescript
var name: string = 'Rob';
```

### Number

Any type of numeric value. In TypeScript, all numbers are represented as floating point.

```typescript
var answer: number = 52;
```

### Boolean

```typescript
var married: boolean = true;
```

### Array

You can use either  Array<type> or type[]

 
```typescript
var music: Array<string> = ['Country', 'Western'];
var team: string[] = ['Cowboys', 'Rams', 'Eagles'];
var scores:  number[] = [18, 21];
```

### Enums
Enums are kinda more like ordered hashmaps. But for the most part, are numbers.


```typescript
enum Color {Red, Blue, Green, Orange, White, Purple};
var aColor: Color = Color.Green;

```

for example...lets run:
http://jsbin.com/tazufo/edit?js,console,output

```typescript
enum Color {Red, Blue, Green, Orange, White, Purple};

var aColor : Color = Color.Green;

console.log("you see the color:"+aColor);

var bColor: Color = Color.Red;

if (aColor == bColor){
  console.log("They match");
} else {
  console.log("They do not match");
}
```

```bash
"you see the color:2"
"They do not match"
```

Lets try another example.

http://jsbin.com/nusowut/edit?js,console,output


```typescript
enum Color {Red =1 , Blue = 3, Green = 5, Orange = 7, White = 8, Purple};

for (i = 0; i < 11; i++) {
    console.log(i+" is:"+Color[i]);
}
```

gets

```bash
"0 is:undefined"
"1 is:Red"
"2 is:undefined"
"3 is:Blue"
"4 is:undefined"
"5 is:Green"
"6 is:undefined"
"7 is:Orange"
"8 is:White"
"9 is:Purple"
"10 is:undefined"
```


 
### Any
Can be any value. Default if not specified.

```typescript
 var stuff: any = 33;
 stuff = 'Bananas';
``` 

### Void
Using void means there�s no type expected. This is usually in functions with no return value:

```typescript
 function doIt(name: string): void {
   console.log("Doing nothing");
  }
```

# let and const

Typescript has two new varible types. _Let_ and _const_.

Let gives us a block scoped variable. Javascript never had this. You might not have even known this unless you ran jslint on a large piece of code.

> var Variables in JavaScript are function scoped. 
> This is different from many other languages (C# / Java etc.) where the variables are block scoped. 
> If you bring a block scoped mindset to JavaScript, you would expect the following to print 123, instead it will print 456:

```typescript
var foo = 123;
if (true) {
    var foo = 456;
}
console.log(foo); // 456
```


> This is because { does not create a new variable scope. The variable foo is the same inside the if block as it is outside the if block. This is a common source of errors in JavaScript programming. This is why TypeScript (and ES6) introduces the let keyword to allow you to define variables with true block scope. That is if you use let instead of var you get a true unique element disconnected from what you might have defined outside the scope. 
> The same example is demonstrated with let:

```typescript
let foo = 123;
if (true) {
    let foo = 456;
}
console.log(foo); // 123
```

Taken from 
https://basarat.gitbooks.io/typescript/content/docs/let.html

You will certainly see this in Angular2.


Const is similar to let in that its blocked.  It also prevents re-assignment to a variable.

```typescript
const x2 : number = 34;
x2 = 44;
```

```bash
bad2.ts(2,1): error TS2540: Cannot assign to 'x2' because it is a constant or a read-only property.
```



 
A Revealing Example (pulled from stack exchange)

Mozilla Developer Network gives an example where var does not work as intended. 
 
```typescript
var a = [];
(function () {
   'use strict';
   for (let i = 0; i < 5; ++i) { // *** `let` works as expected ***
     a.push( function() {return i;} );
   }
} ());
console.log(a.map( function(f) {return f();} ));
// prints [0, 1, 2, 3, 4]

// Start over, but change `let` to `var`.
// prints [5, 5, 5, 5, 5]
```

http://jsbin.com/gipadom/edit?js,console


var fails us because all loop iterations share the same function-scoped i variable, which has the value 5 after the loop finishes.
shareimprove this answer
	
 
Other Links:
* https://www.typescriptlang.org/docs/handbook/variable-declarations.html


# Functions


They can be declared as named or anonymous

```typescript
// Named function
function add(x, y) {
    return x + y;
}

// Anonymous function
let myAdd = function(x, y) { return x+y; };
```

function parameters can be annotated just like variables.  We saw a little of this earlier.
In this case we are specifying that the input is a string and that it returns a number.

```typescript
function  sayHello(name:string) :number{
    return 2
 }
```

You can specify void as well.
```typescript
function  dontSayHello(name:string) :void{
    //do nothing
 }
```

If the return type is not specified, it in interpolated

```typescript
function doit (name:string, age:number) {
    return age;
}
var name2:string = doit("fred",32)
```

Even though the return type is not specified, it is determined from the code.

```bash
$ tsc *.ts
bad2.ts(7,5): error TS2322: Type 'number' is not assignable to type 'string'.
```



You can additionally use further annotations

```typescript
function greet(name: string) {
   console.log("Hello :"+name);
}

greet("Bob");

function greet2(person: {name:string, age:number}) {
   console.log("Hello :"+person.name+" age:"+person.age);
}

greet2({name:"Bob", age:23});
```






## Optional and default parameters

Functions can have optional parameters. This is similar in a way to how javascript works.  In javascript, nothing really stopped you from adding
more of less parameters to a function call. You just had to deal with a lot of undefined values. Its basically similar, however, the 
signature is checked in the ide.  The optional parameters are indicated with a ? and should follow required parameters.



```typescript
enum Color { Red, Blue, Green };
function buildCar(make: string, color: Color, year: number, option1?: string, option2?: string): string {
    let str = "You have a " + year + " " + Color[color] + " " + make;
    if (option1) {
        str = str + " with " + option1;

    }
    if (option2) {
        str = str + " and " + option2;
    }
    return str;
}


var s1 = buildCar("Ford", Color.Red, 1994)
console.log(s1);

var s2 = buildCar("Ford", Color.Red, 1994, "leather seats")
console.log(s2);

var s3 = buildCar("Ford", Color.Red, 1994, "leather seats", "cup holder")
console.log(s3);
```

```bash
"You have a 1994 Red Ford"
"You have a 1994 Red Ford with leather seats"
"You have a 1994 Red Ford with leather seats and cup holder"
```
Play with it here: http://jsbin.com/zalobe/edit?js,console,output


Default parameters are optional as well in the signature.

 
```typescript
function orderCoffee(regular: boolean, withCream:boolean = false) {
	return 'I want a ' + (regular ? 'regular coffee ' : 'decaf coffee ') + (withCream ? 'with cream ' : 'no cream');
}
console.log(orderCoffee(true))
console.log(orderCoffee(true, true))
```

```bash
"I want a regular coffee no cream"
"I want a regular coffee with cream "
```

http://jsbin.com/neciwe/edit?js,console,output



Typescript has rest parameters as well. These are similar to how they are used in Java

You can pass as many of the same type as you would like, or none at all. 
The compiler will build an array of the arguments passed in with the name given after the ellipsis (...).

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}

let hisName = buildName("John", " Jacob", "Jingleheimer", "Schmidt");
```
Try parsing this with the playground
https://www.typescriptlang.org/play/

or using tsc to transpile it.

```typescript
function buildName(firstName) {
    var restOfName = [];
    for (var _i = 1; _i < arguments.length; _i++) {
        restOfName[_i - 1] = arguments[_i];
    }
    return firstName + " " + restOfName.join(" ");
}
var hisName = buildName("John", " Jacob", "Jingleheimer", "Schmidt");
```



Further reading:
 * https://www.typescriptlang.org/docs/handbook/functions.html
 
 

  