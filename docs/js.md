
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

## Switch statement

Instead of typing a complet `if else` conditional statement, we can use a
switch statement to check a series of conditions. Let's take a look on how
to transform a series of statement created with `if else` to a switch statement:

```
let groceryItem = 'papaya';
 
if (groceryItem === 'tomato') {
  console.log('Tomatoes are $0.49');
} else if (groceryItem === 'papaya'){
  console.log('Papayas are $1.29');
} else {
  console.log('Invalid item');
}
```

```
let groceryItem = 'papaya';
 
switch (groceryItem) {
  case 'tomato':
    console.log('Tomatoes are $0.49');
    break;
  case 'lime':
    console.log('Limes are $1.49');
    break;
  case 'papaya':
    console.log('Papayas are $1.29');
    break;
  default:
    console.log('Invalid item');
    break;
}
 
// Prints 'Papayas are $1.29'
```

Pay attention to the `break;` and the last piece which is the `default`

## Magic Eight Ball project

The idea of this project is to use control flow and somethings learned in
the chapter:

```
let userName = ''
userName ? console.log(`Hello, ${userName}!`) : 
            console.log('Hello!')

const userQuestion = 'Should I accept offer?' 

console.log(`The ${userName} asked: ` + userQuestion)

let randomNumber = Math.floor(Math.random() * 8);

let eightBall = ''

switch (randomNumber) {
  case 1:
    eightBall = 'It is certain'
    break;
  case 2:
    eightBall = 'It is decidedly so'
    break;
  case 3:
    eightBall = 'Reply hazy try again'
    break;
  case 4:
    eightBall = 'Cannot predict now'
    break;
  case 5:
    eightBall = 'Do not count on it'
    break;
  case 6:
    eightBall = 'My sources say no'
    break;
  case 7:
    eightBall = 'Outlook not so good'
    break;
  case 8:
    eightBall = 'Signs point to yes'
    break;
}

console.log(eightBall)

//This will print something like:
Hello!
The  asked: Should I accept offer?
Outlook not so good
```

## Functions

There are like 3 ways to create a function in js:

```
const plantNeedsWater = function(day) {
  if (day === 'Wednesday') {
    return true;
  } else {
    return false;
  }
};
```

The same function but as an **arrow function**
 - The idea is to avoid the need to write the word `function`:
```
const plantNeedsWater = (day) => {
  if (day === 'Wednesday') {
    return true;
  } else {
    return false;
  }
};
```
### Consice body arrow functions

The most condensed form of a function are known as **consice body**

 - Functions that take only one parameter, do not need parethensis. If the
   arrow function have zero or more than one parameter, parenthesis are
   required.

```
const functionName = () => {};
const functionName = parameter => {};
const functionName = (A, B) => {};
```
 
 - Functions with a one line body do not need the curly braces. Without the
   curly braces, whatever the line evaluates, it will be returned inmediately.
   The body of the function should be inmediately after the `=>` and we can
   remove the `return`. This is referred as **implicit return**

```
const sumNumber = number => number1 + number2;

const sumNumber = (number1, number2) => {
	const sum = (number1 + number2);
	return sum;
}
```

This is another example on how to transform a function to a consice one:

```
const plantNeedsWater = (day) => {
  return day === 'Wednesday' ? true : false;
};

const plantNeedsWater = day => day === 'Wednesday' ? true : false;
```

## Scope pollution

Scoping rules are the same as in R. Nothing new. But there is one phenomena
that I should be aware of: **Scoping pollution**

This is an example:

```
let num = 50;
 
const logNum = () => {
  num = 100; // Take note of this line of code
  console.log(num);
};
 
logNum(); // Prints 100
console.log(num); // Prints 100
```

So, a good practice is to not declare global variables!!!

### Best practice for scoping pollution

It's called **block scope**. Using this method will improve the code in
several ways:

 - Will make it more legible, since everything is organized in discrete 
   sections.
 - Code will be more understandable, since it clarifies which parts are 
   associated with specific parts of the program.
 - Code will be modular and easier to mantain.
 - It saves memory, since variables will cease to exist after the block
   finishes running.

An example on how to use the block scope:

```
const logSkyColor = () => {
  const dusk = true;
  let color = 'blue'; 
  if (dusk) {
    let color = 'pink';
    console.log(color); // Prints "pink"
  }
  console.log(color); // Prints "blue"
};
 
console.log(color); // throws a ReferenceError
```




# Setting the dev enviroment:

## Installing nodejs

Downloaded from https://nodejs.org/en/

**Warning**
  - Installing from apt, will install an outdated version of nodejs.

So, I downloaded the tar.gz file from the webpage, and then I run:

```
sudo tar -xf node-v18.13.0-linux-x64.tar.xz --directory=/usr/local --strip-components=1
```

After that, I checked the installed version:

```
node -v
```

## First react app

Run the command `create-react-app`
```
create-react-app react-test
```

There is one image in the `img` folder with the terminal output after running
this command.

This will install all the third party libraries needed to create the react app.
Once everything is ready, I can run the template app with

```
cd react-test
npm start
```

Now that we have a template app, we can open VisualStudio to check the
folder structure.

To start playing around with our first app, we can proceed to delete all the
template files inside the `src` folder. We are going to build our own files
from scratch.

## Tips

 - On user settings, I can set up the `formatting on save` to apply formatting
   changes when saving the file.
 - To change several words that are the same (for example `div`) I can
   highlight one of the words and press `ctrl + d`. This will enable multi
   line editing.


# A short game in js

The idea was to create the 'rock, paper, scissors' game using the concepts
of creating a function and the ifelse.

```
console.log('hi');

const getUserChoice = (userInput) => {
  userInput = userInput.toLowerCase();
  if (userInput === 'rock') {
    return (userInput);
  } else if (userInput === 'paper') {
    return(userInput);
  } else if (userInput === 'scissors') {
    return(userInput)
  } else {
    console.log('Error, option not available');
  }
}

const getComputerChoice = () => {
  compNumber = Math.floor(Math.random() * 3 )
  let compChoice = ''

  switch(compNumber) {
    case 0:
      compChoice = 'rock';
      break;
    case 1:
      compChoice = 'paper';
      break;
    case 2:
      compChoice = 'scissors';
      break;
    }
  return(compChoice)
}

const determineWinner = (userChoice, computerChoice) => {
  if (userChoice === computerChoice) {
    return('It\'s a tie!')
  }

  if (userChoice === 'rock') {
    if (computerChoice === 'paper') {
      return('Computer won')
    } else {
      return('Human won')
    }
  }

   if (userChoice === 'paper') {
    if (computerChoice === 'scissors') {
      return('Computer won')
    } else {
      return('Human won')
    }
  }

   if (userChoice === 'scissors') {
    if (computerChoice === 'rock') {
      return('Computer won')
    } else {
      return('Human won')
    }
  }
}

const playGame = () => {
  let userChoice = getUserChoice('paper')
  console.log(userChoice)  
  let computerChoice = getComputerChoice()
  console.log(computerChoice)

  console.log(determineWinner(userChoice, computerChoice))
}

playGame()
```
