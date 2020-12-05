---
name: 'Pong Game'
description: 'Classic Pong Game with Python'
author: '@YashKalbande'
img: 'https://cloud-awrat6ufv.vercel.app/0pong_game.gif'
---


# Pong Game

**Pong Game** is one of the first computer games that was created in the world. Pong is a simple yet compelling **tennis** game. It features **two paddles** and a **ball**. The goal is to defeat your opponent. In this workshop, you'll learn how to build your own  **two-player** `Pong Game`. In this workshop, we are going to use libraries like `freegames`, `random`, and `turtle`. 
# Demo

Here's a [**demo**](https://repl.it/@YashKalbande/ponggame) of what we'll be making. You can see both the final code and test it out on this Repl environment.

![pong game demo](https://cloud-awrat6ufv.vercel.app/0pong_game.gif)

# Table of contents

- [Pong Game](#pong-game)
- [Demo](#demo)
- [Table of Contents](#table-of-contents)
- [Setup](#setup)
- [Building The Game](#building-the-game)
    - [1. Generate Random Values](#1-generate-random-values)
    - [2. Declare Global Variables](#2-declare-global-variables)
    - [3. Moving the Paddles](#3-moving-the-paddles)
    - [4. Drawing our Paddle](#4-drawing-our-paddle)
    - [5.Draw game and move Pong Ball](#5-draw-game-and-move-pong-ball)
    - [6. The Final Touch!](#6-the-final-touch!) 
- [Future Hacking](#future-hacking)

# Setup

### Setting up our Coding Environment
![gif of human coding](https://cloud-awrat6ufv.vercel.app/1coding.gif)

For this workshop, we're going to be using an online code editor called [Repl.it](https://repl.it/site/about).

1. Head over to [Repl.it/languages/Python3](https://repl.it/languages/python3)
   * This will create a new [Python 3](https://www.w3schools.com/python/) Repl for you to use to code.
2. Edit your new `main.py` file with the following code:

```python
from random import choice, random
from turtle import *
from freegames import vector
```
* The code above essentially says the following:
  * Import `choice` and `random` function from the module `random`. This is a built-in Python module that gets random values for our ball to move in random motion.
  * Add all the functions from the `turtle` library. Learn more about the Turtle library [here](https://docs.python.org/3/library/turtle.html). It is a way to implement graphics in Python projects
  * Get the [`vector`](http://www.grantjenks.com/docs/freegames/api.html#freegames.vector) functions from the `freegames` library. The [freegames library](http://www.grantjenks.com/docs/freegames/index.html) is a basic module with built-in functions that do standard game tasks like moving around and setting a playing field.

# Building The Game

### 1. Generate Random Values
```python
def value():
    return (3 + random() * 2) * choice([1, -1])
```
`value` function will return us randomly generate value between `(-5, -3)` or `(3, 5)` each time. This will give **unpredictable direction** to ball.

### 2. Declare Global Variables

Let's start declaring our global variable, we will start with `ball`:
```python
ball = vector(0, 0)
```
`ball` sets the **position** of tennis like a pong ball at the center of the screen.

Now, we will create `aim` variable which will get randomly generated values by `value` function and use it as parameters of direction and speed for `vector` method: 
```python
aim = vector(value(), value())
```
The `state` variable will act as a **JSON object** with the two keys being `1` and `2`. This will be used to update the movement of the paddle in the `y-axis`.
```python
state = {1: 0, 2: 0}
```

### 3. Moving the Paddles
 
We want both Paddle for Player **A** and **B** to be able to move Upward or Downward.  We're going to create a new function called `move` that will allow the Paddles to move.
```python
def move(player, change):
    "Move player position by change."
    state[player] += change
```
### 4. Drawing our Paddle

The turtle will be used to create a paddle. It is quite easy and fast to make closed graphical objects in python by using turtle. To create a paddle or simply rectangle follow the code:

```python
def rectangle(x, y, width, height):
    "Draw a rectangle at (x, y) with given width and height."
    up()
    goto(x, y)
    down()
    begin_fill()
    for count in range(2):
        forward(width)
        left(90)
        forward(height)
        left(90)
    end_fill()
```

What to understand from this code:
* We declare **rectangle** function with **x**, **y**, **width**, **height** as parameters for the function.
* **up()** method is used to pull the turtle's pen up from the screen. It does not leave any marks or traces of its movements.
* The turtle will go to `x`, `y` coordinates using the `goto()` method.
* **down()** method will put the turtle's pen down.
* **begin_fill()** tells the turtle that all upcoming closed graphical objects need to be filled with black color.
* We define a **for loop** to create a rectangle of given `height` and `width`.
* At last **end_fill()** tells turtle to stop the filling.

![understanding gif](https://cloud-bbd2e31l2.vercel.app/0understanding_gif.gif)

### 5.Draw game and move Pong Ball

```python
def draw():
    clear()
    rectangle(-200, state[1], 10, 50)
    rectangle(190, state[2], 10, 50)

    ball.move(aim)
    x = ball.x
    y = ball.y

    up()
    goto(x, y)
    dot(10)
    update()

    if y < -200 or y > 200:
        aim.y = -aim.y

    if x < -185:
        low = state[1]
        high = state[1] + 50

        if low <= y <= high:
            aim.x = -aim.x
        else:
            return

    if x > 185:
        low = state[2]
        high = state[2] + 50

        if low <= y <= high:
            aim.x = -aim.x
        else:
            return

    ontimer(draw, 50)
```

![confusion gif](https://cloud-ea7bxmsh5.vercel.app/0confusion_gif.gif)

Take a moment and try to understand what the code above is trying to say. The best way to interpret code is by turning it into plain English. Here are some helpful tips:
* First of all we called the `clear()` function to tell Turtle to leave the current coordinates and without leaving any traces move to coordinates which will be given now onwards.
* We called the `rectangle` function which we created before to draw **Paddle**. The first one will draw for player **A** and the second will draw for player **B**.
* We will store the direction of `ball` in vector **aim**, which contains the *x* and *y* coordinates of the ball in each frame. Every time we update a new frame, simply we are adding the aim vector to the ball vector to get a new position.
* Handling Collisions of **Pong Ball** is a very important part of code. So let us understand what we did next:
    * The first **if** statement check if the ball hits the top or bottom of the screen. If it satisfies the condition, so simply it will reverse the **y** direction of the ball.
    * Second **if** statement block check if ball hits **left** of the screen. Now, we first have to check if have hit the **paddle**. If we hit the paddle so simply **reverse** the **x** direction of the ball. If we did not hit the ball so **Game Over**.
    * Similarly third **if** block check if the ball hits **right** of the screen. Same checks, if hit the paddle so **reverse** the direction or **Game Over**. So now we complete handling collision.
* **ontime(draw, 50)** is a function of turtle library which will call **draw** function after each **50 milliseconds**. Simply it is serving as the **frame rate** of our game.
     
### 6. The Final Touch!

As we mentioned earlier, this game is coded using a principle called modular programming. This means that everything we code is in functions, now we can bring it together. We can call these functions by writing the function name and the parameters it needs in parentheses. This code block will put all the pieces together:

```python
setup(420, 420, 370, 0)
hideturtle()
tracer(False)
listen()
onkey(lambda: move(1, 50), 'w')
onkey(lambda: move(1, -50), 's')
onkey(lambda: move(2, 50), 'i')
onkey(lambda: move(2, -50), 'k')
draw()
done()
```

**Congrats! You've just made your own Pacman game! 🎉**

![applause gif](https://cloud-460srrq0c.vercel.app/0applause.gif)

Things to note:
   * These are all functions we created earlier, we are just putting the pieces together. 
   * We use `turtle's` built in `setup()` function to display screen.
   * `hideturtle()`, `listen()`, and `tracer(False)` are all default `turtle` functions that will let us remove any unnecessary default items/features and enable us to listen for keystrokes.
   * We are picking up the user's keystrokes by using Python's built-in `onkey()` function.
   
# Future Hacking 

Our version of Pong Game is pretty basic. We are utilizing 2D vector graphics and Python’s turtle graphics library to draw shapes and dots on our screen.

Here are some ideas which I suggest you try out:

* How would you add a computer player? Try to build a bot that will turn the two-player game into a single-player game. Also how about build two bots which will play among them. Sound crazy right!
* Our version of Pong Game is black and white, try different color combinations of paddles, balls, and background.
* As I explained earlier `ontimer()` method servers as frame rate. Try making the frame rate faster or slower.
* Experiment with changing the speed of the ball, changing the size of paddles.
* Try adding a second ball or add paddles at the top and bottom of the screen to make it a **4 player game**.
It is completely up to you how you make it in your style and creativity.

Happy Hacking!

![excitment gif](https://cloud-bvjcncpct.vercel.app/0excitment.gif)
