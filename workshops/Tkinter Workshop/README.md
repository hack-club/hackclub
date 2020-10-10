---
name: "To Quote or Not To Quote?"
description: "Let's build a slideshow of quotes with Tkinter and Python!"
author: "@bezlin6mechminerz"
---

Having a user interface is very useful because it gives both a good interactive experience for the user and makes it much more visually appealing. So today we will see how to make a simple user interface with python in this workshop.

The workshop will look something like this.

![ezgif.com-video-to-gif-3.gif](https://cloud-rbupe5aas.vercel.app/0ezgif.com-video-to-gif-2.gif)

So we will be making a quotes hub which displays quotes.

View a [live demo](https://repl.it/@bezlin/trialtkinterpy-2#main.py)

View the [final code](https://repl.it/@bezlin/trialtkinterpy-2#main.py)

This workshop will take about 20 minutes.

## Getting started

We will be using [Tkinter](https://docs.python.org/3/library/tk.html), a python library to develop the user interface. This project requires [Repl.it](https://repl.it) We will be using it for coding. This is awesome because you don't have to have python on your machine. Just follow this link and start coding!

To get started, [click here](https://repl.it/languages/Tkinter). Your coding environment will be ready in a few seconds!
<img width="1440" alt="Screenshot 2020-09-18 at 5 39 06 PM" src="https://cloud-o8q46nkko.vercel.app/screenshot_2020-09-25_at_9.23.06_pm.png">

#### What is Tkinter?

Tkinter is a library in python used to make cool User Interfaces(UI). What the user sees on the screen to interact with the program is called an user interface. Here we will use this library (Tkinter) to make a UI.

## Code and explanation.

Go to repl and let's get started. Coding along is good than going through all this stuff at once and then getting to code.
![video](https://cloud-1h9458u6z.vercel.app/54_blog_image_13.gif)

So to make the Quotes Hub we need to import the library Tkinter.

```python
import tkinter as tk
```

import Tkinter means we are importing a python library which is Tkinter here. Also for ease, we imported Tkinter as tk so that we don't have to repeat "Tkinter" every time. Instead, we can use tk. We also need another library called random. Let's import it.

```python
import random
```

This library as the name says is used for making random choices. Here we need to display the quotes randomly, so we need to import it. Now let's make a list of quotes.

```python
quotelist = [

  "Instead of wondering when your \nnext vacation is, maybe you \nshould set up a life you don’t \nneed to escape from.",

  "Sometimes\n you win,\n sometimes \nyou learn.",

  "Avoiding failure\n is to avoid \nprogress.",

  "If the plan \ndoesn’t work, \nchange the plan,\n but never the \ngoal.",

  "Creativity \nis \nintelligence \nhaving \nfun."

  ]
```

We are declaring a variable here named quotelist and assigning a list of quotes to it.

Now let's make a window for the UI. For that we need to write this code.

```python
window=tk.Tk()
```

Here, we have made a variable "window" and this is going to be our window. We need to name our window right? So we named it as the window itself. "tk.Tk()" creates the window.Now we need to set the size of the window, for that we use this code.

```python
window.minsize(800,560)
```

This function (minsize) takes 2 parameters:the height and the width. It in measured as pixels like CSS but here we don't have to specify any units because the Tkinter knows that the parameters are to be taken as pixels so we can simply give minsize(800,600). So here we give 800px for width and 600px for height for our window. You can give any height and width as you like. Now we need a title for our window and  we can set it by this code.

```python
window.title("Quotes Hub")
```

You can specify the name of the window inside the function in quotes. Now, let's give the window color. If you like white which is the default ,go with it or do this.

```python
window.config(bg="black")
```

I gave black. You can also specify the color in hex code. RGB too.
window.config means we are configuring our window and setting up its bg which is short for background. Now let's display the Quotes Hub in the window we made. For that we use the Label function.

```python
tk.Label(window, font=("Helvetica", 60, "bold"),text="QUOTES HUB", bg="black", fg="white").pack()
```

The Label function is used to display text on the window. The Label function takes parameters such as font,text,background color(bg) ,font color(fg) .The first parameter you should give is the name of the window in which you need this text to be displayed.

The .pack() function packs the text at the center layout. Now let's make a button to move to the page where we see our first quote.

```python
font=("Helvetica", 60, "bold")
```
Here, in this piece of code, we are setting up the style of the text. font=("font name", font-size, font-weight).
Now, lets make a button.

```python
tk.Button(window,command=lambda:fun1(random.choice(quotelist)),text="Open",bg="aqua",fg="black",width=20).place(x=315,y=300)
```

The Tkinter library consists of A Button function which is used to display a button on the window. The button function  takes many parameters such as text,bg,fg,width,height etc... The first parameter you should give is the window in which you need this button to be displayed. Place() function is used to place the element in the particular coordinate we need it to be ,for example:  button.place(x,y). The main thing here is the command parameter inside the button function

```python
command=lambda:fun1(random.choice(quotelist))
```

This represents the action to be taken when the button is clicked. We can make a function run when clicking on the button using command= function. If you don't use Lambda here then the function will run immediately when you run the code. You don't want to go to the quote suddenly when you run the code right? So to prevent that we use lambda. Then comes the function you want to execute. Here we are calling a function fun1()and passing a parameter. Here the parameter is a random quote. We need to pick a random quote from our quote list we assigned above. For that we use the function random.choice (quotelist).

```python
window.mainloop()
```

window.mainloop() tells Python to run the Tkinter event loop mainloop() at the end of a program in a Python file,else the Tkinter application will never run, and nothing will be displayed.

###### Now the code will look like this.

```python
import tkinter as tk
import random

quotelist = [

"Instead of wondering when your \nnext vacation is, maybe you \nshould set up a life you don’t \nneed to escape from.",

"Sometimes\n you win,\n sometimes \nyou learn.",

"Avoiding failure\n is to avoid \nprogress.",

"If the plan \ndoesn’t work, \nchange the plan,\n but never the \ngoal.",

"Creativity \nis \nintelligence \nhaving \nFun."

]
window = tk.Tk()
window.minsize(800, 560)
window.title("Quotes Hub")
window.config(bg="black")
tk.Label(window, font=("Helvetica", 60, "bold"),
         text="QUOTES HUB", bg="black", fg="white").pack()
tk.Button(window,command=lambda:clicked(random.choice(quotelist)),text="Open",bg="aqua",fg="black",width=20).place(x=315,y=300)
window.mainloop()
```

In the output, an error will be shown,that the function clicked is not declared. So let us make the button work. You need to put this code above the window declaration ,only then will it work.

```python
def clicked(quote):
```

In this function, we are showing a new page in which we display a random quote. We are using the Frame function to create a page here.

```python
F1=tk.Frame(window,bg="light salmon",width=800,height=560)
F1.place(x=0,y=0)
```

The Frame function is used to organize a group of widgets. It acts like a container that can be used to hold the other widgets. The rectangular areas of the screen are used to organize the widgets to the python application. And in that frame, we display the quote at the center using the label function.

```python
tk.Label(F1,text=quote,font=("Helvetica", 30,"bold"),bg="light salmon",fg="black").pack()
```

We have passed the random quote to this function

```python
text=quote
```

Here we gave text as the random quote passed from the button

If you don't understand it, just go to the live demo and see the button workflow [here](https://repl.it/@bezlin/graphics#main.py).

#### Finally the code will look like this.

```python
import tkinter as tk
import random

quotelist = [

  "Instead of wondering when your \nnext vacation is, maybe you \nshould set up a life you don’t \nneed to escape from.",

  "Sometimes\n you win,\n sometimes \nyou learn.",

  "Avoiding failure\n is to avoid \nprogress.",

  "If the plan \ndoesn’t work, \nchange the plan,\n but never the \ngoal.",

  "Creativity \nis \nintelligence \nhaving \nFun."

  ]

def clicked(quote):
  F1=tk.Frame(window,bg="light salmon",width=800,height=560)
  F1.place(x=0,y=0)
  tk.Label(F1,text=quote,font=("Helvetica", 30, "bold"),bg="light salmon",fg="black").pack()
window = tk.Tk()
window.minsize(800, 560)
window.title("Quotes Hub")
window.config(bg="black")
tk.Label(window, font=("Helvetica", 60, "bold"),
         text="QUOTES HUB", bg="black", fg="white").pack()
tk.Button(window,command=lambda:clicked(random.choice(quotelist)),text="Open",bg="aqua",fg="black",width=20).place(x=315,y=300)
window.mainloop()

```

Its simple, you just have to go through the functions and workflow.

![ezgif com-gif-to-mp4](https://cloud-5m2nwfs8r.vercel.app/ezgif.com-video-to-gif-4.gif)

## Hacking time!

Now you know Tkinter a bit. You should not stop here. You need to learn more and to master in UIs you need to practice a lot. The best way to learn it is to look at the world, think of an idea and then try to make it.

Lastly, don't get give up.

I am attaching the Tkinter documentation for you to take a look at [Learn Tkinter](https://docs.python.org/3/library/tk.html).
Also if you are new to python, take a look at [python](https://www.python.org/doc/) documentation too

### Applications made using Tkinter by other hackers.

[Hariprasad Rajan](https://repl.it/@HariprasadR03/haroadas#main.py)

[Kk Haridev](https://repl.it/@DandaThor/harri-kkkkk#main.py)

[Aswin Prakash](https://repl.it/@AswinPrakash/VirtualIncomparableLegacysystem#main.py)

[Anupriya Shaji](https://repl.it/@bezlin/trialtkinterpy#main.py)

After building share your repl link to all communities don't forget about slack and make everyone know the piece of work you have done.

![made_it](https://cloud-a58thexdk.vercel.app/tenor.gif)

Yes, You have made it.