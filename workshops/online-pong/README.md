---
name: 'Online Pong'
description: 'Learn how to build multiplayer browser games with Colyseus.js'
author: '@scitronboy',
img: 'https://cloud-a4tglngif.vercel.app/0thumb.png'
---

Perhaps you've built simple games before, but have you ever built a multiplayer game? In this workshop, we we will be building a simple online version of the classic game of Pong - one that you can play it with someone else on a different computer, or even in a different country! We will be using a fantastic open-source TypeScript framework called [Colyseus.js](https://colyseus.io/) that makes the networking and matchmaking (connecting users to a game automatically when they open the site) easy!

[If you get stuck, you can always view the code files here](https://github.com/scitronboy/multiplayer-pong/tree/workshop/replit)

TODO Link to final game

## Prerequisites

This is **not** a beginner-level tutorial. I will assume you are at least somewhat familiar with JavaScript/TypeScript features such as classes, ternary expressions, promises, and arrow functions. If you are not, I would recommend going and checking out some of the other JavaScript workshops before working up to this. There are lots of cool ones, including [JS clocks](https://workshops.hackclub.com/simple_clock/), [pianos](https://workshops.hackclub.com/tunes/), and [bullet-dodging](https://workshops.hackclub.com/dodge/) games.

We will be using the [TypeScript programming language](https://www.typescriptlang.org/docs/) for the server, which is a _superset_ of JavaScript (just JS but with extra features), as it is the recommended language for Colyseus. While you should be able to write standard JS most of the time and get away with it, I recommend [checking out some of typescript's features](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html) beforehand (specifically, types) if you've never used it before.

## How it works

<details>
<summary><strong>Websockets and Canvas explanation</strong></summary>
  
Most browser-based games use what are called *websockets* to communicate with a *game server*, a special kind of web server responsible for synchronizing each player's movements across every player's browser. Websockets are not like traditional browser (HTTP) requests, as they form a *persistent, 2-way connection* with the server, allowing each server and client to send messages back and forth to each other as quickly as needed. For example, if one player in the game moves or does some other game action (depending on the type of game), the game will immediately send that move to the server using a socket connection, where the server will process the move (and perform any validation necessary to prevent cheating), and send it back over a socket connection to all the other players, whose games will process the new data and update the first player's position, so that everyone can see when one player moves. 

Fortunately, the browser's [WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) is easy enough to use already, but for this tutorial we will be using a JavaScript package called [Colyseus](https://colyseus.io/) that handles the websocket connection and game state automatically, so we can just focus on the game itself.

As for the game itself, we will just be using the native browser [canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) as we don't need anything more than that for this simple game. However, if you want to make more complex games in the future there are many available frameworks such as [Phaser.js](https://phaser.io/) that provide useful tools such as physics and animation engines.

_----End of explanation----_

</details>

So, here's the brief explanation of the project:

1. We'll have a file called `index.ts` (typescript) where we set up and ininitialize the game server. 
2. When a user requests the game from the server, it will send back a `game.html` and `game.js` file - these are standard JavaScript and HTML files, where we will write the client-side code (code that runs on the player's computer) for the game. This includes drawing all the shapes to the canvas.
3. We'll also have a file called `PongRoom.ts` that runs on the server. Here, we will put all the server code for the game. It has two main parts: "state" classes, where we can define all the variables we'll need to keep track of for the game, including the position of the Pong rackets ands and Pong ball; and a "room" class that tells Colyseus what to do when players join a game, leave a game, sends an update, etc. (Each game is basically an instance of the room class)
4. The Colyseus.js library will take care of communicating between the server (the `PongRoom.ts` file) and the client (the `game.js` file). This includes sending messages back and forth, but also synchronizing the state variables across the players and server.
5. When a user moves their racket, for example, we'll tell the Colyseus.js library to send the movement to the server, and the server will update the state, which will automatically get sent to the other player.

Well, let's get started!

## Part 1 - Setting up the Project and Server

I'll be coding everything on Repl.it in this workshop (but if you want, you can use a local environment). Before we start, you'll have to [create your own Repl account](https://repl.it/signup) if you haven't already. 

I have created a template with the code we'll need to begin with. [Click here to access the template](https://repl.it/@scitronboy/MultiplayerPong-template). Then, click the fork button to make your own copy of the template:

![fork button](https://cloud-crjvx8agm.vercel.app/0forkbutton.png)

Or, if you want to start from scratch and add the first few files yourself instead of using the template, click below.

<details>
<summary><strong>I don't want to use the template</strong></summary>

Create an empty project by going to <https://repl.it/languages/typescript>. I would recommend renaming it to something like "Pong" from the dropdown at the top so you can easily find it later.

First things first, let's properly configure the project and add the packages we'll need. Open `tsconfig.json` and replace everything with this, so it works with Colyseus:

```json
{
  "compilerOptions": {
    "outDir": "lib",
    "target": "es6",
    "module": "commonjs",
    "strict": true,
    "strictNullChecks": false,
    "esModuleInterop": true,
    "experimentalDecorators": true
  }
}
```

Great! Now let's add our packages to a new file named `package.json` so that Repl knows what to install (you can add a new file with the icon at the top of the file list):

```json
{
  "private": true,
  "name": "online_pong",
  "version": "0.1.0",
  "main": "index.ts",
  "scripts": {
    "start": "ts-node-dev index.ts"
  },
  "devDependencies": {
    "@types/express": "^4.17.1",
    "ts-node": "^8.1.0",
    "ts-node-dev": "^1.0.0-pre.63",
    "typescript": "^3.4.5"
  },
  "dependencies": {
    "@colyseus/monitor": "^0.12.2",
    "colyseus": "^0.14.0",
    "express": "^4.16.4"
  }
}
```

Now, open `index.ts` - let's prepare the server!

First, let's import the packages we just installed. They include Colyseus (the game framework itself), Colyseus monitor, a small Colyseus add-on that lets you view the game state in real time, and Express, a JavaScript server library that makes it easy for us to send the game files to the players when they visit the website.

```javascript
import http from "http"
import path from "path"
import express from "express"
import { Server } from "colyseus"
import { monitor } from "@colyseus/monitor"
```

Now, we can use this code to set up Express and Colyseus (make sure to read the comments so you know what's going on!):

```typescript
const port = 3000 // We need to choose a port for our game to run on, but it doesn't really matter as REPL will automatically detect it.
const app = express() // This line creates a new Express app for the server

app.use(express.json()) // This line tells express to interpret incoming requests as JSON, which makes it easy for Colyseus to understand and interact with the requests.

// (you can just ignore this next block) On REPL, Colyseus doesn't work over HTTPS without additional configuration. For building this pong workshop this workaround is necessarry to make it work on Repl, but make sure to remove this if you expand your game into a full website with many people playing it, as this effectively disables encryption to prevent mixed content errors. For some reason, converting everything to HTTPS instead wasn't working, even though it should.
app.use((req, res, next) => {
  if (req.headers['x-forwarded-proto'] === 'http') {
    next()
  } else {
    return res.redirect(302, 'http://' + req.headers.host + req.url) 
  }
})

const server = http.createServer(app) // here, we initialize a HTTP server using our express app.
const gameServer = new Server({ server }) // This line adds Colyseus, the game framework, to our Express server!

app.use('/colyseus', monitor()) // This sets up a route allowing us to view all the Colyseus state data in real-time from a browser. I'll explain it later.

gameServer.listen(port) // Finally, we start the server by listening to incoming HTTP requests from players' browsers.
console.log(`Listening on http://localhost:${ port }`)
```

The server will work now, but it doesn't actually _do_ anything. Let's fix that. Create two new empty files, `game.html`, and `game.js`. As mentioned previously, these files will be sent to the player to run on their computers!

In `game.html` let's add a basic HTML page with a title and header:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Pong</title>
    <script src="https://unpkg.com/colyseus.js@^0.14.0/dist/colyseus.js"></script>
  </head>
  <body>
    <h1>Pong</h1>

    <script src='/game.js'></script>
  </body>
</html>
```

You can see in the `head` section we are importing the Colyseus.js library using a `<script>` tage from a remote CDN, and in the `body` section we import the `game.js` script (from our own Pong server).

You can leave `game.js` empty for now.

Now that we created an HTML file, we need to return it to the user when they visit the game website. Under the `app.use('/colyseus'... )` line in `index.ts`, add two new pieces of code:

```javascript
app.get('/', (req: express.Request, res: express.Response) => {
  res.sendFile(path.resolve('game.html')) // Respond with the game file when the user visits the server. Path.resolve makes sure the path is absolute so Express can find the file.
})

app.get('/game.js', (req: express.Request, res: express.Response) => {
  res.sendFile(path.resolve('game.js')) // Send game.js as well.
})
```
Basically, these are both Express "routes" that the server performs when the user does a certain action. Specifically, when the user sends a GET request to the server for the `/` path - the homepage of the website - the server sends the HTML file we just made back. The same thing happens for `game.js`.

You might have noticed the colons and `express.Request` and `express.Response` after the `req` and `res` (request and response) arguments. This is a typescript feature, and it simply tells typescript what type the arguments are so that it can do proper type checking on them.

Finally, create one more empty file called `PongRoom.ts`. We won't add much to it now, but we will add all the pong server logic to it later.

Let's export a skeletal Colyseus room class definition from it. First we have to import the base classes from Colyseus. I'll explain all this later, but for now we can do this with an import statement at the top of the file:

```javascript
import { Room, Client } from "colyseus"
```

Then, add the following empty class:

```typescript
export default class PongRoom extends Room {

  onCreate (options: any) {
  
  }

  update (delta: number) {
    
  }

  startGame () {
    
  }

  onJoin (client: Client, options: any) {

  }

  onLeave (client: Client, consented: boolean) {

  }
}
```

And let's take the exported room class, and import it into `index.ts` so that we can tell the Colyseus game server about it! Add this to the top of the `index.ts` file, right below all the other `import` statements:

```javascript
import PongRoom from "./PongRoom"
```

And, we can tell Colyseus about it with the `gameServer.define` method, **above the line** that says `app.use('/colyseus', monitor())`:

```javascript
gameServer.define('pong', PongRoom) // Add the pong room to the server
```

TODO add image

_----End of template setup----_

</details>

Let's press the "Run" button now and see what happens.

Your project and `index.ts` should look something like this now:

![The index.ts file](https://cloud-ek1yunog1.vercel.app/0pbeginning.png "The index.ts file")

After waiting a few moments for it to run - hopefully without errors - you'll notice a new popup in the top right, showing a blank window that might look like this (or it might be completely blank):

![The broken preview](https://cloud-58evowjmq.vercel.app/0brokenpreview.png "the broken preview")

This isn't really a problem, as it's caused by a workaround we used for Colyseus in our server file (it's related to complicated WSS colyseus support on Repl.it, in case you were wondering). All you need to do is click on the little icon at the top right to open it in a new tab instead, where it should work. It might take a bit for it to load at each step. If the tab times out before it loads, just reload it and try again.

![The blank pong page](https://cloud-qfkqba8yv.vercel.app/0blankpongpage.png "Blank pong page you should see when you open a new tab")

Yeah!! The server works!! 

I should note that whenever you make changes to the code you will have to reload the game tabs to see the changes (the one you got when you pressed the new tab icon, not the Repl.it tab!).

## Part 2 - The Canvas, Game Loop, Colyseus.js, and User Input, But Not Necessarily in That Order.

Now we can start writing the `game.js` file!

### The Canvas

Let's add a [canvas element](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API), where we will draw all the game graphics, to the middle of the `<body>` section:

```html
<body>
  <h1>Pong</h1>

  <canvas id='game-canvas' width='600' height='600'></canvas>

  <script src='/game.js'></script>
</body>
```

Now in `game.js` the first thing we need to do when a user visits the website is ask them for a username to show to the other player. We can do that using the `prompt` function.

```javascript
const userName = prompt("Choose a name:") || 'player' // Fall back to "player" if the user doesn't enter a name
```

Then, we need to get a reference to the canvas's context, so that we can draw on it later:

```javascript
const canvas = document.getElementById('game-canvas')
const width = canvas.width, height = canvas.height // Store the canvas width and height as well
const ctx = canvas.getContext('2d') // This is the canvas context
```

### Handling user input

The only user input we need to handle is the browser's left and right arrow keys, which will move the player's racket left or right. We need to be able to check from the game loop to see if the left or right arrow keys are pressed, so let's add some event handlers that will detect when the user presses or releases the left/right arrows and update a variable.

Add two variables to the `game.js` file:

```javascript
let leftIsPressed, rightIsPressed
```

Then, add a key up and key down event handler that updates these variables depending on which key was pressed or released:

```javascript
window.onkeydown = function (e) {
  if (e.key === 'ArrowLeft') leftIsPressed = true
  if (e.key === 'ArrowRight') rightIsPressed = true
}

window.onkeyup = function (e) {
  if (e.key === 'ArrowLeft') leftIsPressed = false
  if (e.key === 'ArrowRight') rightIsPressed = false
}
```

### Communicating with the server

Now we need to start a connection with the server before we are able to find or join a pong game. With Colyseus.js, this is easy. Add this line to `game.js` now to create a Colyseus Client, which we will then use to interact with the server:

```javascript
// Connect to the colyseus server on port 3000:
const client = new Colyseus.Client(`ws://${window.location.hostname}`)
```

Then, we want to find or create a pong game. The Colyseus `joinOrCreate` function creates a new game room on the server (we will program the pong game room soon), or joins one that isn't full (with this game, each game room of course only holds 2 players). We also pass an object with a `name` property. This will be sent to the game room as well, where we will later save it so that the other player can see it.

```javascript
let room // We'll store the room in this variable
let isPlayer1 // Keep track of who's player 1 and 2
client.joinOrCreate('pong', { name: userName || 'player'})
.then(r => { // We successfully found or created a pong game
  console.log("joined successfully", r)
  room = r
  room.onMessage('youArePlayer1', m => { isPlayer1 = true }) // If the server tells us we're player 1, set isPlayer1 to true
}).catch(e => { // Something went wrong
  console.error("couldn't join room", e)
})
```

Note that this code will throw an error now because we haven't coded the game room on the server yet.

This is what `game.js` should look like so far:

![game.js file so far](https://cloud-qms4n6xxd.vercel.app/0first3gamesteps.png "This is what your game.js should look like now")

### Making a game loop

I've placed the explanation of the game loop and delta time in a details box, so please click the arrow to read it if you are not familiar with `requestAnimationFrame` and delta time. Then, you can go ahead and add this code to the next part of your `game.js` script.

<details>
  <summary><strong>I'm not familiar with this code</strong></summary>
  
Most games have at least one or more *game loop*, that is, a chunk of code composed of an update and drawing function, or maybe a function to calculate physics, etc, that runs as frequently as possible while a game is running (this frequency is known as the frame rate and is often 60 times per second). We need one of these in this game to update the position of the pong ball and rackets every frame, and check to see if a player is holding down one of the arrow keys.

Also, the HTML canvas is quite literally a "canvas" in the sense that you can't adjust the position of something (like a player) or remove something from the canvas (you can only add things) - instead, you have to clear the whole canvas and redraw every object on it, each with their new updated positions. This is the main thing the game loop will be responsible for. 

We will create a function called "loop", which we will pass to a built in browser function called `requestAnimationFrame`. requestAnimationFrame registers a function to be called as soon as possible before the browser repaints the page (which happens roughly 60 times per second on most displays), and because we can call it over and over again at the end of each loop it's the perfect place to call our game loop to make our game graphics appear as smooth as possible (however it won't eliminate lag).

It's also important to keep track of when exactly the loop runs every time, so that we can calculate how many milliseconds it's been since the loop last ran. This is a concept known as "delta time" and it's extremely important for ensuring smooth, consistent gameplay regardless of how fast a player's computer is. For example, if a player moves 10 pixels every frame, then they would move faster on faster computers, but multiplying their speed by the delta time would make sure their speed is always consistent. Fortunately, `requestAnimationFrame` passes the number of milliseconds since the window was loaded into the loop function, which we can use to calculate the delta time.

Each time the loop runs, we will check to see if the player is holding down the left or right arrow. For our purposes, we can simply assume that the user has been holding the left or right key down since the last loop ran, and we can use delta time (the time since the last time the loop was called) to calculate how many pixels the racket should move. So, if the player is holding one of the keys down when the loop runs, we can take deltatime and divide it by 2 to make racket movement a little slower (so, holding the left key down for one second = 1000ms/2 = 500 = the racket moves 500 pixels to the left), and we'll use `room.send` to send it to the server using Colyseus, where it will be handled by a method we'll write later. 

We'll also add a function called `draw`, where we will eventually place all the code for drawing the rackets and pong ball to the screen. However, we don't want to draw anything unless the game has started, so we will only call the `draw` function from the loop if `room.state.gameStarted` is true (we will add room state later).

_----End of explanation----_

</details>

```javascript
function draw () {
  
}

let lastRender = 0 // Initialize lastRender variable to keep track of when the loop was last run.
function loop(timestamp) {
  let delta = timestamp - lastRender // How many milliseconds have past since the loop last ran?

  // Erase the canvas and refill with black every time the loop runs
  ctx.fillStyle = 'black'
  ctx.clearRect(0, 0, width, height)
  ctx.fillRect(0, 0, width, height)

  // Check for user input and tell the Colyseus room to send it to the server
  if (leftIsPressed) room.send('moveRacket', { move: -(delta / 2) }) // Negative sign so the racket moves left
  if (rightIsPressed) room.send('moveRacket', { move: (delta / 2) })

  if (room && room.state.gameStarted) draw() // Draw everything if the game has started

  lastRender = timestamp // Update the last render variable
  window.requestAnimationFrame(loop) // Schedule this function to be run again.
}

window.requestAnimationFrame(loop) // Schedule the loop function to be run next frame
```

Once you add the game loop, you should be able to refresh the game tab, and you will be prompted to enter your name:

![username prompt](https://cloud-d76at74rw.vercel.app/0nameprompt.png "The username prompt")

Then you will see a black rectangle where the canvas is:

![The blank canvas](https://cloud-fkb3m0as0.vercel.app/0blankcanvas.png "The blank canvas")

If you don't see it, make sure you remembered to use `requestAnimationFrame` and set the `fillStyle` to black in the loop:

![Game loop code](https://cloud-mbbb8hggb.vercel.app/0gameloopstep.png "Game loop code")

## Part 3 - Pong Room Server Code

Now, we have to start writing the `PongRoom.ts` file, that controls pong games from the server. Go ahead and create the empty file now. There are 5 things we need to do before we can finish writing the game client:

1. Add some setup code to the `PongRoom` `onCreate` method
2. Add two game state schemas (a schema is kind of like a model of how variables will be stored): one schema class to hold each player's variables, like name and racket position, and one schema class to hold information about the game itself, like where the pong ball is.
3. We have to add a method adds a user's name and id to the state when they join the game.
4. We have to write a `startGame` function that resets the position of the pong ball and starts the game
5. We have to add a listener callback to listen for messages from the client telling the server that the player moved their racket

It's a lot to do and it might get a little boring without any visible results. Unfortunately though, we have to write all this before we can continue writing the game itself!

### The `PongRoom` Class

If you look open the `PongRoom.ts` file, you'll see a single import statement at the top and a bare `PongRoom` class, that extends a `Room` class imported from Colyseus. Colyseus's room class is a class that you can extend to define a "game room" class. You can think of a room like the set of rules that Colyseus follows for a specific type of game - in this case, Pong. 

Each Colyseus room has several methods we can override to do things when certain events happen. For example, we can do different things when a player joins, leaves, sends a message to the server (think: updating the position of a player's pong racket), disconnects, etc. Each room also has several methods we can call to change the room's settings or access different features of Colyseus - for example setting the number of players that can join, setting how often the server sends an update to each player, or accessing the game's built-in clock. _In this workshop, "room" is often interchangable with "game" although I often use "game" to refer to an instance of the room class._

The `onCreate` method is called when a new instance of a room (a new game) is created. Here, we will use it to set up the room. Add the following code to the `onCreate` method in the `PongRoom` class:

```typescript
onCreate (options: any) {
  this.setState(new PongState()) // Set the state for the room, this is a class we will write in the next step
  this.setSimulationInterval(delta => this.update(delta)) // Set a "simulation interval" aka an update function (similar to the loop function in game.js) that's called about 60 times per second. We'll use this later.
  this.setPatchRate(25) // The patch rate determines the interval (in milliseconds) at which the server sends state updates to the client.
  this.maxClients = 2 // Only 2 players per Pong game!
  this.clock.start() // Start the game clock, a colyseus feature we'll use later
}
```

### State classes

Before we can extend state schema classes, we have to import the base classes from Colyseus. The template already has imported the `Room` and `Client` classes from Colyseus, but we can also import the `Schema` class and `type` decorator with a second import statement right at the top of the `PongRoom.ts` file:

```typescript
import { Schema, type } from "@colyseus/schema"
```

Whenever you have game data that needs to be synchronized across the players and the server, you should use Colyseus's _state_. Each room instance can have one state instance set, and that state class can be declared by extending the Colyseus `Schema` class. In each schema, we must define all the variables we want to use, as well as their types (you can also use schemas within schemas!), using a typescript feature called a "decorator", like this: `@type(either another schema, or something like 'int8' meaning 8 bit integer)` This is because Colyseus has to know what variables to send to the clients, and it also has to know what type of data they are so that it can binary encode them (using a library called msgpack) so they take up as little space as possible when being transfered over websockets.

We'll start by writing a `Player` schema with some variables about each player. **Make sure you add both schema classes above the exported `PongRoom` class** (as TypeScript won't automatically "hoist" classes to the top of the file, like it does with functions and methods).

```typescript
class Player extends Schema {
  // This class keeps track of the racket position, score and name for a player
  @type('number')
  racketX: number = 250 // Initializing it at 250 will make sure it's centered

  @type('int8')
  score: number = 0

  @type('boolean')
  hasWon: boolean = false // Has the player won?

  @type('string')
  name: string // The player's user name

  @type('string')
  clientId: string // We'll use this to keep track of which player is which
}
```

Next, we'll add a `PongState` schema to store schemas for each player as well as information about the pong (add this class between the other two):

```typescript
class PongState extends Schema {
  @type('boolean')
  gameStarted: boolean = false // Has the game started?

  // We instantiate two player schema classes, one for each player
  @type(Player)
  player1: Player = new Player() 
  @type(Player)
  player2: Player = new Player()

  // We also define a few variables to keep track of the Pong
  @type('number')
  pongX: number
  @type('number')
  pongY: number
  @type('boolean')
  pongDirection: boolean // 1 means it's flying towards player 2, 0 means it's flying toward player 1
  @type('float32')
  pongAngle: number // 0 means it's flying in a straight line, 1 is 45 degrees right, -1 is 45 degrees left
}
```

### Handling When a Player Joins

Now that we wrote some nice state schemas, let's set the name of each player when they join the room! From within a room class, you can always access the state variables from the `this.state` object! We also have to start the game when there are two players. We can do this in the `onJoin` method of the `PongRoom` class:

```typescript
onJoin (client: Client, options: any) {
  // Determine whether this is player 2 or player 1 joining. If player 1 already exists then this is player 2.
  const alreadyHasPlayer1 = !!this.state.player1.clientId // If the player1 clientId is already there, this must be player 2
  const newPlayerState = alreadyHasPlayer1 ? this.state.player2 : this.state.player1

  // Set the player's name and ID:

  newPlayerState.name = options.name // options contain options passed from the player. Remember when we wrote the .joinOrCreate part of game.js? We passed the user's name to the server so that we could use it here!

  newPlayerState.clientId = client.sessionId // We can also get the new player's session ID (assigned by Colyseus) and set it. We can then use this to identify them.

  if (alreadyHasPlayer1) {
    // We now have 2 players and can start the game!!!
    this.clock.setTimeout(() => this.startGame(), 2000) // Wait 2 seconds before starting
  } else {
    client.send('youArePlayer1') // This is player 1, make sure to let them know!
  }
}
```

### Starting the Game

Now, in the `startGame` method of the `PongRoom` class, all we have to do is reset the pong and randomize its direction, then set the `gameStarted` state variable to true, which will let the clients know the game is starting. Remember when we were writing `game.js` and we accessed `room.state.gameStarted`? Well, we're setting it here, and because it's defined in the schema colyseus makes sure it is transfered to each player as well.

```javascript
startGame() {
  this.state.pongDirection = Math.random() <= 0.5 // Randomize the starting direction of the pong
  // Reset the position and angle of pong
  this.state.pongX = 300
  this.state.pongY = 300 
  this.state.pongAngle = 0
  this.state.gameStarted = true // Start the game!!!
}
```

### Handling when a user moves their racket

Earlier, in the `game.js` loop, we sent a message to the server every time the player pressed the arrow keys to move their racket. We need to add a listener to the room now to actually update the racket state variable when it recieves that message. We sent the message from the client using `room.send` and we can receive it in the room by using `this.onMessage` with a callback. **Add this to the bottom of the `onCreate` method** in `PongRoom`, before the closing bracket, so it's registered as soon as a room is created:

```typescript
this.onMessage('moveRacket', (client, data) => {
  // First, we check the client's id to see whether they're player 1 or player 2
  const player = (client.sessionId === this.state.player1.clientId) ? this.state.player1 : this.state.player2

  player.racketX += data.move // Adjust the player's pong position with the data passed from the player (game.js)

  player.racketX = Math.min(Math.max(player.racketX, 0), 500) // We clamp the paddle position so the player can't move it off the canvas
})
```

Done! Most of the pong room logic is now ready.

## Part 4 - Drawing the Pong Ball, Rackets and Score

Now that we have the state schemas all set up on the server, we can access the state from the client and use it to draw everything onto the canvas!

### The Rackets

We can easily draw the rackets by getting their positions from `room.state`, which is always up-to-date with the latest state on the server. Add functions to draw the two rectangles to the `draw` function in `game.js`:

The player's own racket should always be at the bottom of the screen, so we can create new variables `topPlayer` and `bottomPlayer`, depending on whether this player is player 1 or player 2, to make sure we don't mix this up.

```javascript
function draw () {
  // This player plays from the bottom of the canvas
  const bottomPlayer = isPlayer1 ? room.state.player1 : room.state.player2
  const topPlayer = isPlayer1 ? room.state.player2 : room.state.player1

  // Draw the rackets
  ctx.fillStyle = 'white' // set the color
  // Draw the bottom racket with a width of 100 and height of 20
  ctx.fillRect(bottomPlayer.racketX, height - 20, 100, 20)
  // Draw opponent's top racket
  ctx.fillRect(topPlayer.racketX, 0, 100, 20)
}
```

### The Pong Ball

Next, we should show the movement of the pong ball on the canvas. It doesn't actually move yet - simulating its movement is the next part - but we can still draw it with some more code at the end of the `draw` function.

Again, the `state.pongY` actually represents how far away from player 1 the pong ball is, so that means that player 1 has to flip/invert pongY to get the correct coordinate (because canvas coordinates start at the top left, but the player's racket is at the bottom of the canvas). Add this to the bottom of the draw function:

```javascript
// Draw the pong ball
ctx.fillStyle = 'limegreen'
ctx.beginPath() // Start a new drawing path
const pongY = isPlayer1 ? height - room.state.pongY : room.state.pongY // For player 1 we should flip the direction of the ball to get the correct relative coordinate
ctx.arc(room.state.pongX, pongY, 10, 0, 2 * Math.PI) // Draw the ball with a radius of 20
ctx.fill() // fill it in
```

### The Score

The score also does not yet change, but we can still draw it as the third part in the `draw` function.

```javascript
// Draw the score
ctx.fillStyle = 'white'
ctx.font = '30px Arial'
ctx.fillText(bottomPlayer.score, 15,  height - 45) // The bottom player's score
ctx.fillText(topPlayer.score, 15, 45) // The top player's score
```

Your `draw` function should now look something like this:

![the draw function code](https://cloud-850hguur0.vercel.app/0drawfunc.png "The draw function")

Now, let's press run and open the game in a new tab as we did earlier to see if everything is working! You'll just see a blank black square at first - that's because there's only one "player".  Earlier when we were writing `PongRoom`, we wrote the `onJoin` function so that it would only set `state.gameStarted` to true once there were two players. But, the draw function only runs if the game has started. So, you can trick the server into thinking there are two players and starting the game by opening the game in another tab so you have it open in two tabs at a time. Then, if you wait a few seconds for the game to start, you should suddenly see the `draw` function drawing everything to the canvas!

![pong game with draw function](https://cloud-6o9ib5amw.vercel.app/2pong_draw.png "The pong game")

Now, we've also added user input listeners that send updates to the server, that updates the state. So, you should be able to move the racket using the left and right arrow keys from one tab:

![moving the racket from one tab](https://cloud-6o9ib5amw.vercel.app/1pong_racketp1.png "moving the racket from one tab")

And then go to the second tab, and see the racket that you moved in the first tab!!!

![and seeing it change the racket in the second tab](https://cloud-6o9ib5amw.vercel.app/0pong_racketp2.png "and seeing it change the racket in the second tab")

Congratulations! This is all there is to the basics of multiplayer games with Colyseus!

But, the ball doesn't actually move... In the next part, we will update the position of the ball at every frame!

## Part 5 - Calculating Pong Ball Movement on the Server

Open `PongRoom.ts` again and scroll down to the `update` function in the `PongRoom` class. You may remember we set this up earlier as a "simulation interval". It runs 60 frames per second on the server by default, and we'll use it to update the position of the Pong ball each frame.

Here's how it will work:

1. We keep track of the x position, y position (**where a greater y value means the ball is closer to player 2**), direction (1 means it's flying toward player 2, 0 means it's flying toward player 1) and angle (-1 means it's aiming 45 degrees left, 1 means it's aiming 45 degrees right, 0 means it's flying straight up or down) of the pong ball in the room state (we added this earlier).
2. Each update, we move it (delta/3) y pixels in whichever direction it's travelling. If the direction is 1 (`true`), we move the ball **towards** player 2 by increasing the ball's Y position, and if it's 0 (`false`) we decrease the ball's Y position, moving it away from player 2.  (we'll keep delta/3 as a variable named `speedConstant` so that we can easily access or change it). Because we are changing the ball's position by (`delta / 3`) pixels each frame, that means it would move 1000 pixels across the canvas in 3 seconds, because delta (time) counts how many milliseconds (thousandths of a second) have passed each frame. We also add (the angle value * (delta/3)) to the ball's X position each frame to get a new x position for the ball. This means that if the angle value is 1, the ball will move sideways (along the X axis) the same amount as it will move vertically (along the Y axis) - aka 45 degrees.
3. If we detect the ball has moved within the "goal" area of either player (within 20px of the end) by using an `if` statement to check the ball's Y position, we check to see if it collided with the racket by comparing the ball's X position with the racket's X position.
4. If it **did not** collide, we increment the other player's score, and reset the ball's position and start a new round using the same `startGame` function we wrote earlier.
5. If it **did** collide, we switch the direction of the ball, and calculate the new direction it's flying in using this formula: ((ball x position - center of racket x position) / "bounce constant (which will be 40)"). Basically, this formula will ensure that when the ball bounces on one side of the racket, it bounces off in that direction, at an angle proportional to the distance from the center of the racket. When the ball bounces off the center of the racket, it will fly in a straight line, as described by this drawing:

 ![Basic pong physics](https://cloud-fnc0pkw30.vercel.app/0pongphysics.png "Basic pong physics")

 Note that you can adjust this formula if you want the ball to bounce differently. Setting the "bounce constant" variable to half the racket width (50) would cause a maximum bounce angle of 45 degrees, but we will decrease that to 40 to give a slighly larger possible bounce angle.
6. If we detect the ball touching a side of the canvas using another `if` statement, we flip the angle (multiply it by `-1`) so it bounces back.
7. If either player has more than 10 points, we'll set `hasWon` to true for that player and destroy/disconnect the room. We will use this variable in the next step to announce the winner.

Here's the function:

```typescript
update (delta: number) {
  if (!this.state.gameStarted) return // Don't update the ball's position if the game hasn't started!

  const speedConstant = delta / 3 // Increasing the number 3 will make the ball move slower and vice versa.
  
  const bounceAngleConstant = 40 // less is more!

  // Update the ball's Y position:
  if (this.state.pongDirection) this.state.pongY += speedConstant // Ball is moving TOWARD player 2, so we increase its y position
  else this.state.pongY -= speedConstant // else, ball is moving away from player 2, so we decrease its Y position

  // Update the ball's X position:
  this.state.pongX += (speedConstant * this.state.pongAngle) // Change the x value depending on the angle.

  if (this.state.pongY + 10 >= 580 || this.state.pongY - 10 <= 20) // If ball is touching goal zone on either side (+- 10 to account for radius)...
  {
    const isOnPlayer1Side = this.state.pongY - 10 <= 20 // Is it on player 1's side or player 2's?

    const racketX = isOnPlayer1Side ? this.state.player1.racketX : this.state.player2.racketX // Get the racket position, depending on whos side it's on

    if (this.state.pongX >= racketX && this.state.pongX <= racketX + 100 ) { // If the ball's x position matches the racket, that means it collided!
      // Bounce the ball off the racket:
      this.state.pongDirection = !this.state.pongDirection // Flip the direction the ball is moving
      this.state.pongAngle = (this.state.pongX - (racketX + 50)) / bounceAngleConstant // Calculate the new angle for the racket, based on where the ball collided
      this.state.pongY = isOnPlayer1Side ? 30 : 570 // Move the ball's Y position to the edge of the racket to make sure it doesn't get stuck in the racket
    } else { // Ball did not collide with racket - SCORE!!!
      if (isOnPlayer1Side) this.state.player2.score += 1 // If the ball's on played 1's side, player 2 scored
      else this.state.player1.score +=1 // else, player 1 scored
      
      this.startGame() // We can just reuse the startGame function to start a new round
    }
  } else if (this.state.pongX >= 590 || this.state.pongX <= 10) { // If the ball is touching the left or right edge of the canvas...
    this.state.pongAngle *= -1 // Flip the angle so the ball bounces back
  }

  if (this.state.player1.score >= 10 || this.state.player2.score >= 10) { // If one of the players has a winning score of 10...
    if (this.state.player1.score >= 10) this.state.player1.hasWon = true // Player 1 won
    else this.state.player2.hasWon = true // else player 2 won

    // These are both Colyseus room methods:
    this.broadcastPatch() // Broadcast the new state update to make sure each player knows who won before we disconnect them

    this.disconnect() // Disconnect players from room and dispose of the room (it's no longer needed as the game is over)
  }
}
```

Press run, then close the two game tabs you already had open and open two new ones so you can test it out! If all went well, you should now have a fully functioning Pong game you can play with a friend! The ball will start moving in a random direction when the game starts, it will bounce off your rackets, and it will increment the other player's score when you miss it with your own racket!

![moving pong ball](https://cloud-h1cmx5i1y.vercel.app/0pongmovement.gif "Moving pong ball")

## Part 6 - Final Touches

The game works as it is now, but there are still a few things we need to fix and add. First, the server doesn't actually do anything when one player leaves, which sometimes allows a third player to join, causing weird problems, such as the rackets and ball getting mixed up and moving in the wrong direction. So, we need to disconnect the room when a player leaves or gets disconnected so that this doesn't happen. Second, we need to add some status text to tell the user what's going on: when the server is waiting for another player, who they're playing against, when the other player disconnects, and when either player wins.

Finally, we'll add two more optional features to the game: one, we'll add a short delay between rounds, so that each player has a chance to adjust their rackets before the next round begins. Second, we'll make it slightly more difficult by slowly increasing the speed of the ball over time.

#### Adding graceful disconnects

In the `PongRoom` class there's a `onLeave` function that get's called when a user leaves the pong room. In there, let's disconnect and dispose the room whenever a user leaves:

```typescript
onLeave (client: Client, consented: boolean) {
  this.disconnect() // If a player leaves the game is unplayable, so destroy the room and disconnect the remaining player so that they can find a new game.
}
```

### Adding status text

We could add the status/announcement text to the canvas, but I prefer to add UI elements like status text as HTML around the canvas, as it's easier to program and style. So let's add a new `<p>` tag to `game.html`, right between the header and canvas. At the start of the game, it will say "Waiting for opponent":

```html
<body>
  <h1>Pong</h1>
  <p id='game-status-text'>Waiting for opponent...</p>
  <canvas id='game-canvas' width='600' height='600'></canvas>

  <script src='/game.js'></script>
</body>
```

Now, let's create a reference to this element at the top of `game.js`:

```javascript
const gameStatusText = document.getElementById('game-status-text')
```

Now, we need to update this text when the server starts the game. We can do this using the Colyseus `state.listen` function, which calls a callback whenever a state variable (such as `gameStarted` changes. Let's add the game start listener to the bottom of the `.then()` callback, after the line that says `room.onMessage('youArePlayer1',...)` near the top of the file. It will change the status text to tell the user the name of the person they're playing against:

```javascript
room.state.listen('gameStarted', (currentValue, oldValue) => {
  // If the game has started and it wasn't started previously, update the status text
  if (currentValue && !oldValue) gameStatusText.innerText = `${room.state.player1.name} vs ${room.state.player2.name}`
})
```

Then, we can add another listener in the same place (right below the gameStarted listener) that listens for the server to disconnect using the Colyseus `room.onLeave` function (which could happen either on accident, or because the other player left, or because the game is over) and updates the status text to tell the user what happened:

```javascript
room.onLeave((code) => {
  gameStatusText.innerText = 'Game Over. ' // We were disconnected from the game (either intentionally or because of an error), let the player know it's game over.
  // Let the user know if either player won:
  if (room.state.player1.hasWon) gameStatusText.innerText +=  ` ${room.state.player1.name} won!!!` // If player 1 won, add their name
  else if (room.state.player2.hasWon) gameStatusText.innerText += ` ${room.state.player2.name} won!!!` // else if player 2 won, add theirs
  else gameStatusText.innerText += ' A player disconnected.' // If neither player won, that can only one of the players disconnected before the game was finished.
  gameStatusText.innerText += ' Reload the page to find a new game.' // Tell the player how they can find a new game.
})
```

Now, if you open two new tabs and start playing, you should see the status text appear above the canvas:

![pong status text element](https://cloud-1bp1dymvt.vercel.app/0winningtext.png "Pong status text")

<details>
<summary><strong>Optional features: delay between rounds and increasing ball speed</strong></summary>

### Adding a short delay between rounds

To add a short delay between rounds, let's add a new property to the `PongRoom` class (right at the top before any methods) called `roundIsRunning` that will keep track of whether or not a round is currently playing:

```typescript
export default class PongRoom extends Room {
  roundIsRunning: boolean
```

<details>
  <summary><i>Why did I add this property to the room class rather than the state class?</i></summary>
  
Put simply, If you have something that needs to be synchronized across the server and players, you should always add it to the state class, and if you have something that can just stay on the server, you can just add it as a property to the room class so as to prevent unnecessary data from being transfered across the websockets. The truth is, throughout this workshop I add several things to the state class that shouldn't really be added to the state class (yet) but in the future, make sure you only add stuff that's necessarry to the state class. 

As well as reducing the overall size of the state, you might sometimes want to omit data from the state class that you don't want the players to have access to. Remember, everything in the state can be accessed by any player even if you don't explicitly share it with them, they just have to open up their browser's developer tools and search for it. Sometimes, you'll have a state variable that you want to synchronize with just ONE player but not the others (so that they can't use it to cheat). In this case, you can try using [Colyseus's `@filter` decorators](https://docs.colyseus.io/state/schema/#filtering-data-per-client).

_----End of explanation----_

</details>

Then, somewhere in the `startGame` function we can set it to true:

```typescript
this.roundIsRunning = true
```

And in the `update` function, we can replace the line that starts a new round (`this.startGame()`) with this, which will set `roundIsRunning` to false and wait one second before starting the next round:

```typescript
this.roundIsRunning = false
this.clock.setTimeout(() => this.startGame(), 1000) // Wait 1 second before starting next round
```

And finally, we don't want the update function to run if a round isn't currently playing, so we can change the first line in the `update` function to:

```typescript
if (!this.state.gameStarted || !this.roundIsRunning) return // Don't update if the game or round hasn't started
```

### Making it slightly more difficult

There are many ways once could improve this game by making it look better, perform better with less lag, or making it more difficult. Here, we will add one such addition to demonstrate how you can add new features. 

In most pong games, the ball speed starts out slow and gradually increases, then resets when a match ends.

First, we need to add a new variable to keep track of when each match starts. let's add that to the top of the `PongRoom` class in `PongRoom.ts` right below the `roundIsRunning` property:

```typescript
  roundIsRunning: boolean
  roundStartedAt: number
```

Then, let's set that number every time a new game starts somewhere in `startGame`:

```typescript
this.roundStartedAt = this.clock.elapsedTime // Set the round started time using the timestamp from the colyseus clock
```

Now, let's double the speed of the pong every thirty seconds by replacing the `const speedConstant =` line in the `update` method. To do that, we'll subtract the elapsed time (ms) at the start of the round from the current elapsed time (ms), and divide it by 30,000 to see how many times 30 seconds have passed (we'll also add 1 so that it starts at 1 rather than 0) We'll multiply this by our speed constant in the `update` function, like this:

```typescript
const timeMsSinceRoundStart = this.clock.elapsedTime - this.roundStartedAt
const speedConstant = (delta / 3) * (timeMsSinceRoundStart / 30000 + 1) // Calculate the speed constant for the ball. It should gradually increase over time.
```

</details>


Our Pong game is finally complete! 

<video controls>
  <source src="https://cloud-8kwmmedl3.vercel.app/0pong.webm" type="video/webm">
</video>

## Part 7 - Conclusion

Now, you can go ahead and share the link to your game with your friends, and have fun playing and hacking it!

If you got stuck and need to check your code, you can [view any of the files from my version on this GitHub repo](https://github.com/scitronboy/multiplayer-pong/tree/workshop/replit)

#### Examples of how people have customized it:

+ a
+ b
+ c

You could also try...

+ a
+ b
+ c

If you're part of [the Hack Club slack](https://hackclub.com/slack/), make sure to share your game in the [#scrapbook](https://app.slack.com/client/T0266FRGM/C01504DCLVD) channel!

**This is the end of this workshop.** I will share a few final notes below, but you don't have to read them if you don't want to! Thanks for reading, and if you have any questions make sure to ask your club leader, or [DM me on the slack](https://hackclub.slack.com/archives/D0128N09V6U).

<details>
  <summary><strong>Final Notes</strong></summary>  
  
#### Using the Colyseus Monitor

You may recall when we were first setting up our server we added an express route that said `app.use('/colyseus', monitor())`. This added a built-in Colyseus monitor page that we can access by navigating to `your-game-url/colyseus`. It has details about each game room, the state, and the players connected to the room:

![the colyseus monitor](https://cloud-4vpsksvmq.vercel.app/0colyseusmon.png "The colyseus monitor")

There isn't really much point to using this for a simple Pong game, but as you continue to develop multiplayer games with Colyseus you might find it very useful for debugging your games (although if you publish your games make sure to [secure the monitor so only you can access it](https://docs.colyseus.io/tools/monitor/#restrict-access-to-the-panel-using-a-password)).

#### Learning more

If you would like to continue using the Colyseus framework, you will [find the docs useful](https://docs.colyseus.io/). But remember, if you're serious about delving into the topic, you should also try implementing networking on your own using WebSockets as a learning experience!

Also, multiplayer game networking, across the web and other gaming platforms like console and PC is very complex and fascinating. As you build more complex games, you will have to worry about things like **cheaters** and **network latency**. I personally think these are interesting topics and if you're interested in learning more about some of the techniques used even by big game studios to minimize these problems, I recommend starting with [this series on server-client architecture](https://www.gabrielgambetta.com/client-server-game-architecture.html). It goes over all sorts of interesting stuff like client-side prediction, server-side reconciliation and lag compensation. You'll see that the way we implemented movement here wasn't really optimal.

As for the game itself, if you want to continue making more complex browser games in the future there are helpful frameworks such as [Phaser.js](https://phaser.io/) that provide tools such as physics and animation engines, which you might want to look into.

Anyway, I hope I haven't overwhelmed you with all this, but rather given you a good introduction to the many possibilities and factors involved in multiplayer browser game development! Thanks for reading!
  
</details>