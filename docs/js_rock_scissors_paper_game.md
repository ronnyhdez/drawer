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
