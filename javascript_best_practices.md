# Javascript Best Practices
### Your script is your art: make it structured, make it consistent, make it beautiful.


## Table of Contents

1. Syntax: be consistent.
2. Scope: be mindful.
3. Design: be modular. 
4. Optimization: be faster. 
5. Post-coding: be cleaner. 
6. References: be better.  


## Syntax: be consistent.

#### Use camelCase for variable and function names; provide meaningful names. 

Variable and function names should indicate the main purpose of the function. They should follow a consistent camelCase format throughout your script. 

Avoid the following reserved keywords for names: break, case, class, catch, const, continue, debugger, default, delete, do, else, export, extends, finally, for, function, if, import, in, instanceof, let, new, return, super, switch, this, throw, try, typeof, var, void, while, with, yield

**X** Incorrect: 
	
	// fails to use camelCase  
	var helloworld = "hello world"; 

	var helloworld = function() {
		console.log("hello world"); 
	}
	
	// fails to use meaningful name
	var abc123 = "hello world"; 
	
	var abc123 = function() {
		console.log("hello world");
	}	
	
	// uses keyword 
	var class = "hello world"; 
	
	var class = function() {
		console.log("hello world"); 
	}

**✔** Correct: 
	
	var helloWorld = "hello world"; 
	
	var helloWorld = function() {
		console.log("hello world");
	}

##### Use curly braces on the same line as functions and loops.
Use curly braces on at the end of functions and loops. Use one space before the opening of the bracket; end the bracket on a separate line.

**X** Incorrect: 

	var helloWorld = function()
	{
		console.log("hello world");
	}
	
	for (var i=0; i<5; i++) 
	{
		console.log(i);
	}
	
**✔** Correct:
	 
	var helloWorld = function() {
		console.log("hello world");
	}

	for (var i=0; i<5; i++) {
		console.log(i);
	}
	
##### Use double quotes, not single quote. 
This is a matter of preference. For the sake of consistency, we use double quotes. 

**X** Incorrect: 

	var helloWorld = function() {
	  	console.log('hello world');
	}

**✔** Correct: 

	var helloWorld = function() {
		console.log("hello world");
	}
	
	
##### Use four spaces for indentation.
This is a matter of preference. For the sake of consistency, we use four spaces using tab, not two spaces.

**X** Incorrect: 

	var helloWorld = function() {
      console.log("hello world");  
  	}
  
**✔** Correct: 

	var helloWorld = function() {
		console.log("hello world");
	}


##### Use semicolons sparingly.
This is a matter of preference. For the sake of consistency, don't use semicolons after curly brackets; they are unnecessary. 

**X** Incorrect: 

	
	// unnecessary semicolon at end of statement
	if (helloWorld === "hello world") {
		console.log(helloWorld);
	};

**✔** Correct:  
	
	if (helloWorld === "hello world") {
		console.log(helloWorld);
	}


##### === and == are different; use the appropriate one.
When using an equal operator for comparison, === means the values being compared are the same value and of the same data type, whereas == means only the values being compared have the same value (but not necessarily of the same data type). Be mindful of which case applies to your scenario.

	// returns true
	4 == "4"
	
	// returns false
	4 === "4"
 

## Scope: be mindful.

##### Use global variables and functions with caution.

Global variables are variables defined outside of your function or inside of your function without the use of var keyword. Global functions are functions that are independent of any particular object you have defined. Global variables and functions belong to the window object.  

They can easily collide with functions and variables with the same name when adding a third-party library or even your own functions and variables in the future if you are not careful about namespace. This is dangerous.

**X** Incorrect: 

	// global variable outside of a function
	var userName = Susy; 
	
	// global variable without var keyword
	var helloWorld = function() {
		userName = Susy; 
		console.log("hello" + userName); 
	}
	
	// global function without object literal
	function globalFoo() {
		console.log("I'm global foo.")
	}
	
	 
##### To avoid global pollution, use object literals.

Object literals encapsulate variables and functions, while avoiding polluting the global namespace. Object literals are used to store and access data.

	var fooObject = {
		var foo = 1;
		
		helloFoo: function() {
			console.log("hello foo");
		}
	};


##### Be aware of hoisting; declare your variables as the top within your scope.
Hoisting is the act of variable declarations being processed before any code is executed within a particular scope. Hoisting often causes confusing and unexpected results. 

Variables used within a particular scope get "hoisted" or brought to the top of the scope before they are initialized or evaluated.

	var x = 10; 
	var helloFoo = function() {
		console.log(x); // undefined
		x = 20; 
		console.log(x); // 20
	}
	helloFoo(); 


	// How interpreter orders the execution
	var x = 10; 
	var helloFoo = function() { 
		var x; // x is used in helloFoo scope, so it is "hoisted" to the top
		console.log(x); // undefined because we have not yet initialized the value of x
		x = 20; // x is now initialized with value 20
		console.log(x); // output is 20 based on initilization in previous line
	}


	
To avoid bugs resulting from hoisting, make sure to declare and initialize your variables at the top your scope with a var statement. 

**X** Incorrect: 

	var helloFoo = function(){
		console.log(x + y); //undefined 
		
		var x = 10; 
		var y = 50; 	
	}

**✔** Correct: 

	var helloFoo = function(){
		var x = 10; 
		var y = 50;
			 
		console.log(x + y); 
	}
	


Function expressions are hoisted to the top in similar fashion (though not function declarations). 

**X** Incorrect: 
	
	helloFoo(); // undefined 
	var helloFoo = function() {
		console.log("hello foo");
	}


**✔** Correct: 
	
	var helloFoo = function() {
		console.log("hello foo"); 
	}
	helloFoo(); // hello foo
	



## Design patterns: be modular.

##### Use the revealing module pattern. 

Javascript has no "right" way of structuring your code. We use the revealing module pattern; it encapsulates code in a structured way while also being understandable and maintainable. 

Public methods are "revealed" by being returned. Private methods are not returned. Public variables can be returned by themselves or used by a method which is returned. Private variables are not returned and are not used by a method which is returned. 

	
	var fooModule = (function() {
	
		// private variable 
		var privateHello = "Hello, I'm a private variable."; 
		
		// public variable 
		var publicHello = "Hello, I'm a public variable";  
		
		// private method 
		var privateGreet = function() {
			return privateHello;
		}

		// public method 
		var publicGreet = function() {
			return publicHello;
		}
		
		return {
			publicHello: publicHello, // public variable
			publicGreet: publicGreet // public method 
		}
	
	}()); 
	

##### Use revealing module pattern, even with jQuery.

We can use revealing module with third-party libraries like jQuery.

	var fooModule = (function($) {
	
		// private variable 
		var privateHello = "Hello, I'm a private variable."; 
		
		// public variable 
		var publicHello = "Hello, I'm a public variable";  
		
		// private method 
		var privateGreet = function() {
			return privateHello;
		}

		// public method 
		var publicGreet = function() {
			return publicHello;
		}
		
		return {
			publicHello: publicHello, // public variable
			publicGreet: publicGreet // public method 
		}
	
	}(jQuery)); 
	 
	
##### Use IIFE with the revealing module pattern. 
	
Use Immediately Invoked Function Expression (IIFE) with the revealing module pattern. An IIFE is an anyonomous function which is executed right after it is created: 
  
	(function() { ... }())
  
This is useful because an IIFE helps prevent global pollution and controls scope. Any variable or method declared inside the IIFE is encapsulated and not accessible globally. By naming your IIFE, your code is encapsulated within the fooModule namespace (though giving a name is not always necessary to use an IIFE). 
	

##### If you use Object-Oriented Javascript, make use of prototypes. 

Use OOP Javascript if you need make many instances of your variables and methods in large scale software projects. JS is not a traditional class based language; instead it uses prototypes as means to add properties and methods to all instances of an object. 



	function Animal(name) {
		// constructor function 
		this.name = name; 
	}

	// add a method to the constructor 
	Animal.prototype.greet = function() {
		console.log("Hello, " + this. name); 
	}
	
	// add another method
	Animal.prototype.walk = function() {
		console.log(this.name + "is walking."); 
	}
	
	// create an object; make instances of the class
	var monkey = new Animal("monkey"); 
	monkey.greet(); // Hello, monkey. 
	
	var dog = new Animal("dog"); 
	dog.greet(); //Hello, dog.
	
	
## Optimization: be faster. 
#### Minify your JS.
Minification reduces load time by removing unnecessary characters like white spaces and new line characters. Minified JS does not be decompressed; the browser will interpret a minified JS file the same way as a regular JS file. 

To minify, use a library like YUI Compressor or Google Closure Compiler. Use task runners like grunt or gulp to automate minification. 

#### Obfuscate with caution. 
Obfuscation is the process of modifying your script by changing the names of variables and functions in order to reduce size. It makes the script more difficult to understand. Use obfuscation with caution as it increases risk of introducing bugs.

#### Remove unnncessary scripts. 
Be disciplined about only including the scripts that are being used in your code; remove the ones that aren't at they unnecessarily add to load time. 

#### Place your script at end of body. 
Place your script at the end of the body of your HTML so that the page loads first and then the script is fetched and executed. If you place your script at the head, you will have to first fetch the script and then load your DOM; this will result in bad user experience. 

jQuery is an exception because it executes your code when the DOM is ready through the use of: 

	$(document).ready() 


## Post-coding: be cleaner.

#### Use strict mode. 
Use strict mode at the top of your code. Strict mode will throw errors where your code does not follow best practices which may pass otherwise. It will eliminate "silent" errors. Strict mode can be applied to an entire script or a particular function. Use it to better your code.   

#### Use JSHint to lint your code after you're done. 
Use JSHint to lint your code. Ideally, your grunt file will already have a plugin that allows you to automate linting. (http://jshint.com/)

If you want to be especially strict with your code, use JSLint. (http://www.jslint.com/)


## References: be better.

<strong>Websites</strong>: <br/>
Douglas Crockford's Coding Style Guide <br/>
<http://javascript.crockford.com/code.html>

Learning Javascript Design Patterns <br/>
<http://addyosmani.com/resources/essentialjsdesignpatterns/book/>

Javascript Best Practices (Part 1 & 2) <br/>
<https://www.thinkful.com/learn/javascript-best-practices-1/#Comment-as-Much-as-Needed-but-Not-More>
<https://www.thinkful.com/learn/javascript-best-practices-2/Comment-as-Much-as-Needed-but-Not-More>

Pragmatic Standards <br/>
<https://github.com/stevekwan/best-practices/blob/master/javascript/best-practices.md>

Eloquent Javascript </br> 
<http://eloquentjavascript.net/>

<strong>Books</strong>: <br/>
JavaScript: The Good Parts <br/>
<https://www.safaribooksonline.com/library/view/javascript-the-good/9780596517748/>

Javascript Patterns <br/> 
<http://shop.oreilly.com/product/9780596806767.do>

A Smarter Way to Learn Javascript </br/>
<http://www.amazon.com/Smarter-Way-Learn-JavaScript-technology/dp/1497408180/ref=asap_bc?ie=UTF8>


