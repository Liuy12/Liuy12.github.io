---
title: 'Javascript'
comments: true
layout: post
tags:
    - web design
---

- comments for javascript is `\\`

- basic syntax for javascript: `"cake".length`

- javascript
    - websites that responde to user interaction, example: `confirm("...")`, `promot("...")`
    - build apps and games
    - access information on the internet (query?)
    - data visualization 

- strings with quote, numbers without quote

- `console.log()`, print out whatever is in the parentheses.

- '===' equals to; '!==' not equal to 

- if statement syntax 

~~~ javascript
if ( "Leo".length<4) {
    console.log("You have a short name!" );
}
else {
    console.log("You have a short name!");  
}
~~~

- substring: `"wonderfulday".substring(3,7)`

- declare a variable: `var myAge = 20`;

- declaration of a function:

~~~ 
var greeting = function (name) {
    console.log("Great to see you," + " " + name);
};
~~~

- concatenate strings using '+'

- put semicolon at end of each line and each function

- return a value rather than print to screen;

~~~
var timesTwo = function(number) {
    return number * 2;
};
~~~

- `var` use only the varibale that is in current scope (global or local)

- `math.random()`, generate a random number between 0 and 1;

- for loop syntax in javascript:

~~~
for (var counter = 1; counter < 6; counter++) {
    console.log(counter);
}
~~~

- `i++, i--, +=, -=`

- array in javascript: `var arrayName = [data, data, data];`
can be accessed via `[index]`

- wrap long text using '\' in the end of sentence.

- `push` is a method for array, to push the thing in '()' to the array.

- while statememt:

~~~
while(coinFace === 0){
    console.log("Heads! Flipping again...");
    var coinFace = Math.floor(Math.random() * 2);
}
~~~ 

- do/while loop, execute at least once;

~~~
var loopCondition = false;

do {
    console.log("I'm gonna stop looping 'cause my condition is " + loopCondition + "!");    
} while (loopCondition);
~~~

- isNaN: is not a number; 

- switch case 

~~~
var lunch = prompt("What do you want for lunch?","Type your lunch choice here");

switch(lunch){
  case 'sandwich':
    console.log("Sure thing! One sandwich, coming up.");
    break;
  case 'soup':
    console.log("Got it! Tomato's my favorite.");
    break;
  case 'salad':
    console.log("Sounds good! How about a caesar salad?");
    break;
  case 'pie':
    console.log("Pie's not a meal!");
    break;
  default:
    console.log("Huh! I'm not sure what " + lunch + " is. How does a sandwich sound?");
}
~~~

- logical operators: (&&), (||), (!)

- toUpperCase() and toLowerCase()

- heterogenous arrays and arrays inside of arrays; if the dimension is not the same, then it's jagged arrays;

- creat one object:

~~~
var myObject = {
    key: value,
    key: value,
    key: value
};
~~~

example

~~~
var phonebookEntry = {};

phonebookEntry.name = 'Oxnard Montalvo';
phonebookEntry.number = '(555) 555-5555';
phonebookEntry.phone = function() {
  console.log('Calling ' + this.name + ' at ' + this.number + '...');
};
~~~

can also be custructed via

~~~
var myObj = new Object();
myObj["name"] = "Charlie";
myObj.name = "Charlie";
~~~