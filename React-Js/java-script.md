# Explain passed by value and passed by reference.?

 In JavaScript, **primitive data types** are passed by value and **non-primitive data types** are passed by reference. 
 
 **primitive data types**
 ```
 var y = #8454; // y pointing to address of the value 234

var z = y; 
        
var z = #5411; // z pointing to a completely new address of the value 234
        
// Changing the value of y
y = 23;
console.log(z);  // Returns 234, since z points to a new address in the memory so changes in y will not effect z
```
**NOTE:**  From the above example, we can see that primitive data types when passed to another variable, are passed by value. Instead of just assigning 
the same address to another variable, the value is passed and new space of memory is created. 

 Assign operator dealing with **non-primitive data types**: 
 
 ```
 var obj = { name: "Seenu", surname: "S" };

var obj2 = obj;
```

 In the above example, the assign operator, directly passes the location of the variable obj to the variable obj2. In other words, the reference of the 
 variable obj is passed to the variable obj2. 

```
var obj = #8711;  // obj pointing to address of { name: "Seenu", surname: "S" }

var obj2 = obj;
        
var obj2 = #8711; // obj2 pointing to the same address 
        
        
// changing the value of obj1
        
obj1.name = "Seeni";
        
console.log(obj2);
        
// Returns {name:"Seeni", surname:"S"} since both the variables are pointing to the same address.
```

 **From the above example, we can see that while passing non-primitive data types, the assign operator directly passes the address (reference). **
 
 # What is an Immediately Invoked Function in JavaScript? 
 
  An Immediately Invoked Function ( known as IIFE and pronounced as IIFY) is a function that runs as soon as it is defined.
  
  Syntax of IIFE : 
   
  ```
    (function(){ 
      // Do something;
    })();

  ```
   First set of parenthesis: 
    ```
      (function (){
        //Do something;
      })
    ```
   While executing javascript code, whenever the compiler sees the word “function”, it assumes that we are declaring a function in the code. Therefore, 
   if we do not use the first set of parentheses, the compiler throws an error because it thinks we are declaring a function, and by the syntax of 
   declaring a function, a function should always have a name. 
   
   ```
   function() {
      //Do something;
   }
   // Compiler gives an error since the syntax of declaring a function is wrong in the code above.

   ```
   To remove this error, we add the first set of parenthesis that tells the compiler that the function is not a function declaration, instead, it’s a function expression. 
   
    Second set of parenthesis: 
    ```
      (function (){
        //Do something;
      })();
    ```
    
     From the definition of an IIFE, we know that our code should run as soon as it is defined. A function runs only when it is invoked. If we do not invoke the function, 
     the function declaration is returned.
     Therefore to invoke the function, we use the second set of parenthesis.
    
 
#  Explain Higher Order Functions in javascript. 

  Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions. 
 
  Higher order functions are a result of functions being first-class citizens in javascript. 
  
  ```
    function higherOrder(fn) {
      fn();
    }
 
    higherOrder(function() { console.log("Hello world") }); 

  ```
 
 # Explain call(), apply() and, bind() methods. 
 
  ##  call() 
  
   It’s a predefined method in javascript.  This method invokes a method (function) by specifying the owner object. 
   
   ```
    function sayHello(){
      return "Hello " + this.name;
    }

    var obj = {name: "Sandy"};

    sayHello.call(obj);

    // Returns "Hello Sandy"

   ```
   
   
   ##  apply() 
   
    The apply method is similar to the call() method. The only difference is that, call() method takes arguments separately whereas, apply() method takes 
    arguments as an array. 
    
    ```
      function saySomething(message){
        return this.name + " is " + message;
      }

      var person = {name:  "Seenu"};

      saySomething.apply(person, ["awesome"]);

    ```
  
  ##  bind() 
  
   This method returns a new function, where the value of “this” keyword will be bound to the owner object, which is provided as a parameter. 
   
    Example with arguments: 
    
    ```
      var bikeDetails = {
          displayDetails: function(registrationNumber,brandName){
          return this.name+ " , "+ "bike details: "+ registrationNumber + " , " + brandName;
        }
      }

      var person = {name:  "Seenu"};

      var detailsOfPerson1 = bikeDetails.displayDetails.bind(person, "KA122", "Bullet");

      // Binds the displayDetails function to the person1 object

      detailsOfPerson1();
      // Returns Seenu, bike details: KA122, Thunderbird

    ```
#  What is currying in JavaScript? 
  
   Currying is an advanced technique to transform a function of arguments n, to n functions of one or less arguments. 
   
    By using the currying technique, we do not change the functionality of a function, we just change the way it is invoked. 
    Let’s see currying in action: 
    
      ```
       function multiply(a,b){
        return a*b;
      }

      function currying(fn){
        return function(a){
          return function(b){
            return fn(a,b);
          }
        }
      }

      var curriedMultiply = currying(multiply);

      multiply(4, 3); // Returns 12

      curriedMultiply(4)(3); // Also returns 12
    
    ```
  
    As one can see in the code above, we have transformed the function multiply(a,b) to a function curriedMultiply , which takes in one parameter at a time. 

#  Explain Scope and Scope Chain in javascript. 

   Scope in JS, determines the accessibility of variables and functions at various parts in one’s code. 
   In general terms, the scope will let us know at a given part of code, what are the variables and functions that we can or cannot access. 
   
    There are three types of scopes in JS:

    * Global Scope
    * Local or Function Scope
    * Block Scope

    ## Global Scope 
       Variables or functions declared in the global namespace have global scope, which means all the variables and functions having global scope can be 
       accessed from anywhere inside the code 
  
    ## Block Scope 
       Block scope is related to the variables declared using let and const. Variables declared with var do not have block scope. 
       Block scope tells us that any variable declared inside a block { }, can be accessed only inside that block and cannot be accessed outside of it. 
       
       ```
          {
            let x = 45;
          }

          console.log(x); // Gives reference error since x cannot be accessed outside of the block

          for(let i=0; i<2; i++){
            // do something
          }

          console.log(i); // Gives reference error since i cannot be accessed outside of the for loop block

       ```
  
  
   ##  Scope Chain 
   
     JavaScript engine also uses Scope to find variables. 
     
     ```
        var y = 24;

        function favFunction(){
          var x = 667;
          var anotherFavFunction = function(){
            console.log(x); // Does not find x inside anotherFavFunction, so looks for variable inside favFunction, outputs 667
          }

          var yetAnotherFavFunction = function(){
            console.log(y); // Does not find y inside yetAnotherFavFunction, so looks for variable inside favFunction and does not find it, 
                            // so looks for variable in global scope, finds it and outputs 24
          }

          anotherFavFunction();
            yetAnotherFavFunction();
        }

        favFunction();

     ```
  
   As you can see in the code above, if the javascript engine does not find the variable in local scope, it tries to check for the variable in the outer scope. 
   If the variable does not exist in the outer scope, it tries to find the variable in the global scope. 
  

#  Explain Closures in JavaScript. 

 Closures is an ability of a function to remember the variables and functions that are declared in its outer scope. 
 

# What are object prototypes? 

 All javascript objects inherit properties from a prototype. 
 
  For example, 
     Date objects inherit properties from the Date prototype 
     Math objects inherit properties from the Math prototype 
     
 On top of the chain is Object.prototype. Every prototype inherits properties and methods from the **Object.prototype.**
 
A prototype is a blueprint of an object. Prototype allows us to use properties and methods on an object even if the properties and methods do not 
exist on the current object. 


# What are callbacks?  

   A callback is a function that will be executed after another function gets executed.

   In javascript, functions are treated as first-class citizens, they can be used as an argument of another function, can be returned by another
   function and can be used as a property of an object. 
   
    Functions that are used as an argument to another function are called callback functions. 
    
    Example: 
    
    ```
        function divideByHalf(sum){
          console.log(Math.floor(sum / 2));
        }

        function multiplyBy2(sum){
          console.log(sum * 2);
        }

        function operationOnSum(num1,num2,operation){
          var sum = num1 + num2;
          operation(sum);
        }

        operationOnSum(3, 3, divideByHalf); // Outputs 3

        operationOnSum(5, 5, multiplyBy2); // Outputs 20

    ```



# 
