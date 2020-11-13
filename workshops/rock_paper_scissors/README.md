---
name: 'Rock Paper Scissors'
description: 'Make a simple rock paper scissors game using Python'
author: '@JackTDC'
img: 'https://cloud-3skpb14wz.vercel.app/0rockpaperscissors.png'
---

Have you ever wondered how to build a game with Python? Have you ever wanted to build your own game but didn't because it was hard? Well, today I'm going to show you how to build one and show you just how simple it can be!

![homepage](https://cloud-8p13u30yt.vercel.app/rps.png)

Here's the [live demo][final_live_demo] and [final code][final_code].

[final_live_demo]:https://repl.it/@JackTDC/Rock-paper-Scissors#main.py
[final_code]: https://repl.it/@JackTDC/Rock-paper-Scissors#main.py
[repl_it]: https://repl.it

## Part 1: Prerequisites

You should have a beginner understanding of: 
- Python

## Part 2: Setup

### Setting up your code environment on Repl.it

[Repl.it](https://repl.it) is an online code editor where you can build your game. You don't have to use Repl.it but I suggest you do as it sets everything up for you and you don't require any installations.

To get started, go to [repl.it/languages/python](https://repl.it/languages/python). Your coding environment will spin up in just a few seconds!

You should see something like the following:

![main.py file in Repl.it](https://cloud-lukrtynqs.vercel.app/repl.png)

## Part 3: Making The Game
![GIF](https://cloud-931ehqeec.vercel.app/coding.gif)

### 1) Importing Modules
First, we'll need to import the `randint` module. To learn more about the randint package click [here][randint].

[randint]: https://www.journaldev.com/36085/randint-method-in-python/

On the first line of the `main.py` file, type `from random import randint`. This will import the randint module.

### 2) Giving values to the variables
Let's make a list for all the entries a player could enter. In your `main.py` file, add:
```py
t = ["r","p","s"]
```
'r' stands for `Rock`, 'p' stands for `Paper` and 's' stands for `Scissors`.

Now, let's write some code that chooses rock, paper or scissors randomly.
```py
computer = t[randint(0,2)]
```
Now we will set a variable to false and by using it in a `while` loop, we can keep the game playing infinitely! To do that, just type 
```py
player = False
```

Now, let's write some code so the player can enter their name.
```py
name = input("Enter your name:")
```

Also to make a scoring system, we'll write the following code:
```py
You = 0
PC = 0 
```

Next, we want to give an option to the player to reset the game.
```py
print("Type 'reset' to reset score")
```

Here's the code so far:
```py
from random import randint

t = ["r", "p", "s"]

computer = t[randint(0,2)]

player = False

name = input("Enter your name:")
print("Type 'reset' to reset score")

You = 0
PC = 0
```

### 3) Keeping the game in a loop
![loop](https://cloud-nmra250be.vercel.app/loop.gif)

For this, I'll use a `while` loop but you can also use `for` loops!

At the bottom of the `main.py` file, add this:
```py
while Player == False: 
```
Next, we'll use `input` to allow the player to choose either `Rock`, `Paper`, or `Scissors`.
```py
player = input("Rock, Paper, Scissors?(r,p,s)")
```

There are 5 possibilities:
1. The player chooses `Rock`
2. The player chooses `Paper`
3. The player chooses `Scissors`
4. The player enters `reset`
5. The word entered is invalid

### 4) Producing separate outcomes for all possibilities

To make things easier I will be making functions to print if the player wins or loses.
First I will write the following before we make the function.
```py
message = ""
```
This will help us by printing different message by calling the same function.



Now I will be making a function that will be called only when the player wins. To do that enter the following code:
```py
def win():
  global You
  You+=1
  print (message)
  print('Computer =', PC, '\n', name, '=', You)
```
Now we will make a function that will be called only when the player losses.
```py
def lose():
  global PC
  PC+=1
  print (message)
  print('Computer =', PC, '\n', name, '=', You)
```
Now we have to call these functions in the right places.


First we will assume that the outcome is a tie to do that enter the following code:
```py
while player == False:
    player = input("Rock, Paper, Scissors?(r,p,s)")
    if player == computer:
        print("Tie!")
        print('Computer =',PC)
        print(name,'=',You)
```
Next, we want to write some code for each action that someone can take.
```py
    elif player == "r":
        if computer == "p":
            message = "You lose!","Paper covers Rock"
            lose()
        else:
            message ="You win!", "Scissors cuts Paper"
            win()
```
What would the code be for paper and scissors?
```py
    elif player == "p":
        if computer == "s":
            message = "You lose!","Paper covers Rock"
            lose()
        else:
            message ="You win!", "Scissors cuts Paper"
            win()
    elif player == "s":
        if computer == "r":
            message = "You lose!","Paper covers Rock"
            lose()
        else:
            message ="You win!", "Scissors cuts Paper"
            win()
```
Now for the last part we are going to assume that the player wants to reset the game or he/she entered a invalid entry
```py
    elif player == "reset":
      You=1*0
      PC=1*0
      print("The score has been reset!")
    else:
        print("That's not a valid play. Please select a valid option!")
```

### 5) Keeping the loop going
Now as we have completed building the game we need it to run infinitely.

To do this you just need to add the following statement after the `while` statement at the end,
```py
player = False
```
This will instantly trigger the loop and keep the game going.

And at last, you need to add the following command so the computer chooses a different outcome every time
```py
computer = t[randint(0,2)]
```

So, this is the final code:
```py
from random import randint

t = ["r", "p", "s"]

computer = t[randint(0,2)]

player = False

name = input("Enter your name:")
print("Type 'reset' to reset score")

You = 0
PC = 0

def win():
  global You
  You+=1
  print(message)
  print('Computer =', PC, '\n', name, '=', You)

def lose():
  global PC
  PC+=1
  print (message)
  print('Computer =', PC, '\n', name, '=', You)

while player == False:
    player = input("Rock, Paper, Scissors?(r,p,s)")
    if player == computer:
        print("Tie!")
        print('Computer =',PC)
        print(name,'=',You)
    elif player == "r":
        if computer == "p":
            message = "You lose!,Paper covers Rock"
            lose()
        else:
            message ="You win!, Scissors cuts Paper"
            win()
    elif player == "p":
        if computer == "s":
            message = "You lose!,Paper covers Rock"
            lose()
        else:
            message ="You win!, Scissors cuts Paper"
            win()
    elif player == "s":
        if computer == "r":
            message = "You lose!,Paper covers Rock"
            lose()
        else:
            message ="You win!, Scissors cuts Paper"
            win()
    elif player == "reset":
      You=1*0
      PC=1*0
      print("The score has been reset!")
    else:
        print("That's not a valid play. Please select a valid option!")
    player = False

    computer = t[randint(0,2)]
   

```

### 6) The End
**And congratulations! You just made your own game using Python!**

![Congratulations](https://cloud-b32jvmwl1.vercel.app/congo.gif)

If you haven't created an account on [repl.it](https://repl.it), make sure to make an account to save this wonderful creation!

If you are facing difficulties signing up watch [this](https://www.youtube.com/watch?v=Mtqp4CUepk0).

Here are some things which you can do:

- Consider changing and adding more features!
- Making it a two-player game.
- Make the game only of 3 turns instead of running it infinitely.
- You can give the text a typewriter effect. (If you don't know how to do it watch [this][typewritter] video)

**Demos from people who have hacked this workshop:**

- [Adding new actions](https://repl.it/@JackTDC/Rock-Paper-Scissors-Lizard-Spock)
- [Changing the interface](https://repl.it/@JackTDC/Rock-Paper-Scissors#main.py)
- [Making it two-player](https://repl.it/@JackTDC/Rock-paper-Scissors2-player#main.py)

[typewritter]: https://youtu.be/2h8e0tXHfk0

Now that you have finished building your game, you should share your beautiful creation with other people! Remember, it's as easy as giving them your URL!

You probably know the best ways to get in touch with your friends and family, but if you want to share your project with the worldwide Hack Club community there is no better place to do that than on Slack.

1. In a new tab, open and follow [these directions][slack] to signup for our Slack.
2. Then, post the link to the [`#scrapbook`](https://hackclub.slack.com/messages/scrapbook) channel to share it with everyone!

[slack]: https://slack.hackclub.com/
