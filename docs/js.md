
# Notes on JS learning

From codecademy I obtained the following webpage that contains documentation
on the methods for strings:

[js documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

**methods** are actions we can perform. We call or use this methods by:

 - a period
 - the name of the method
 - opening and closing parethesis

 ## Built-in objects

 There are objects already define in JS that we can use. Each of this objects
 have methods. More on this, [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)

Example with `math` object:

```
Math.floor(Math.random() * 50)
```

This performs:

 - random generates number random between 0 and 1
 - Multiplyind by 50 makes it between 0 and 50
 - floor round the number to the lowest whole number


## Notes on properties and methods

 - Objects can have properties (stored information). Properties are denoted
 with a `.` after the name of the object, for example: `Hello.length`
 - Objects including instances of data types can have methods which perform
 actions.

## Notes on variables

 - `var` is pre E56 versions
 - Variables are stored in memory
 - `let` is preferred way to declare a variable when we want to be it 
 	reassigned.
 - Variables not initialized are assigned to `undefined`
 - `+` can concatenate strings
 - In E56 backtick and `${}` are use to interpolate values into a string
 - `typeof` return the data type of a value

## Truthy and Falsy assigment

 An example of a conditional that evaluates if the variable is empty or not
 and assigns a default string if not:

```
let username = '';
let defaultName;
 
if (username) {
  defaultName = username;
} else {
  defaultName = 'Stranger';
}
 
console.log(defaultName); // Prints: Stranger
```

Now, if we combine logical operators, we can do the same with less typing:

```
let username = '';
let defaultName = username || 'Stranger';
 
console.log(defaultName); // Prints: Stranger
```

A `||` statement check the left condition first. If username had a string value,
that one would be assigned to `defaultName`. Otherwise, if falsy, the `Stranger`
value would be assigned. This concept is known as **short-circuit-evaluation**

## Ternary operator

This is a way to simplify an `if else` statement:

```
let isNightTime = true;
 
if (isNightTime) {
  console.log('Turn on the lights!');
} else {
  console.log('Turn off the lights!');
}
```

Now, the way to apply the **ternary operator** is:

```
isNightTime ? console.log('Turn on the lights!') : console.log('Turn off the lights!');
```

This is another example:

```
let favoritePhrase = 'Love That!';

favoritePhrase === 'Love That!' ? console.log('I love that!') :
  console.log("I don't love that!");
```


