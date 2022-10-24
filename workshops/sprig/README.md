---
name: 'Running a Sprig Workshop'
description: 'Use the Sprig game editor in your club.'
author: '@leomcelroy'
img: "https://user-images.githubusercontent.com/27078897/192363683-dc202e64-bf3b-475f-81c1-18cd769a4b6e.png"
---

# Running a [Sprig](https://editor.sprig.hackclub.com) Workshop

<img width="500" alt="Screen Shot 2022-09-23 at 4 41 25 PM" src="https://user-images.githubusercontent.com/27078897/192053140-3bf9cc19-0aa4-4cdb-9845-e4fdf04f2286.jpg">

[Sprig](https://editor.sprig.hackclub.com/) is a tool developed by Hack Club to help people quickly get started making interesting tile-games. It's a tiny game engine embedded in a web-based editor with integrated art and music tools. If you build a game in Sprig and share it in the community gallery you can get a [Sprig Console](https://sprig.hackclub.com)!

> This workshop is designed to help club leaders use Sprig in their clubs. 
> If you are a solo hacker you can jump straight into the [Sprig editor](https://editor.sprig.hackclub.com).
> You may be able to pick up some helpful tips reading through this though.

<img width="500" alt="Screen Shot 2022-09-23 at 4 41 25 PM" src="https://user-images.githubusercontent.com/27078897/192053241-51cd9891-2e13-482c-b5e0-2d285b87f62c.png">

We designed Sprig to accomodate workshops with minimal instruction and maximal making and exploring. This is how I would run Sprig as a workshop.

## Demo (2 minutes)

Give a quick demonstration of a fairly complete game with accessible code. You can go to the [gallery](https://sprig.hackclub.com/gallery) and sort by the `beginner` tag.

<img width="500" alt="Screen Shot 2022-09-23 at 5 00 24 PM" src="https://user-images.githubusercontent.com/27078897/192056026-c4c5492d-00e8-44fb-be35-6b74a1c3bfe2.png">

Here are a few games that meet the criteria:

- [Sokoban](https://editor.sprig.hackclub.com/?file=https://raw.githubusercontent.com/hackclub/sprig/main/games/sokoban.js)
- [Penguin Slide](https://editor.sprig.hackclub.com/?file=https://raw.githubusercontent.com/hackclub/sprig/main/games/penguin_slide.js)
- [Nomis](https://editor.sprig.hackclub.com/?file=https://raw.githubusercontent.com/hackclub/sprig/main/games/nomis.js)

## Toolkit (3 minutes)

Open up the toolkit and quickly describe the different things you can do. The point here is not to explain every function. It's just to give people a sense of what is possible and where to look for help. Only spend a few minutes doing this! Check out [the snippet below](#toolkit) to survey essentially every function in Sprig.

Make sure you **tell people how to save files**.

To save add a name like so:

```js
/*
@title: my_game_name
@author: leo_mcelroy
*/
```

And then hit `save`.

Recent files are available in the menu. An asterisk next to file means your current changes are unsaved. Files are saved in local storage in your browser. They will only be accessible on the computer you are currently using and the browser you are using. **The most reliable way to save is to downlad the `js` file**.

## Tell People to Get Hackin'! (rest of available workshop time)

Tell people to start by working through the [tutorial](https://editor.sprig.hackclub.com/?file=https://raw.githubusercontent.com/hackclub/sprig/main/games/getting_started.js).

After people complete the tutorial encourge them to start thinking of their own game. Often a good way to make and original game is to start with an existing one and adding a new mechanic to it. Check out the games tagged `hackable` to find remixable bases.

## Share (10 minutes)

Gather everyone's attention and have them share their games with you. Ask if anyone would like to demo their game to the group. Celebrate the work and use it as a chance for each person to get feedback, inspiration, and affirmation.

If the games are complete then [make a PR](https://sprig.hackclub.com/share) to submit them to the gallery and [earn a Sprig console](https://sprig.hackclub.com). 

If the games are not complete encourage people to keep working on them and plan to continue working in your next club meeting. 
You can also encourage people to share the games they have so far by posting links to those games in the [Sprig channel](https://hackclub.slack.com/archives/C02UN35M7LG) on Slack.
You can generate a link by navigating to the `share` -> `as link` option in the file menu, as seen below:

<img width="500" alt="Screen Shot 2022-09-23 at 4 41 25 PM" src="https://user-images.githubusercontent.com/27078897/192363738-35a5fa1e-d4e8-4b56-8ff1-878191ab8829.png" />

People could easily spend 3 or 4 club meetings working on games.

# Reference

<a name="toolkit">The toolkit</a> is essentially every function we added to Sprig, otherwise it's just JavaScript.
For more information on these functions check the `help` menu in the Sprig editor.

```js
const player = "p";
const wall = "w";

setLegend(
    [ player, bitmap`.` ],
    [ wall, bitmap`.` ],
);

setBackground(wall);

const level = map`.`
setMap(level);

setSolids([ player, wall ]);
setPushables({
  [player]: [wall]
})

getFirst(spriteType);
getAll(spriteType);
getTile(x, y);
tilesWith(spriteType0, spriteType1, ...);
addSprite(x, y, spriteType);
clearTile(x, y);

addText("string", { x, y, color: [r, g, b] });

clearText();

onInput("w", () => {});
afterInput(() => {});

const melody = tune`...`
playTune(melody)
playTune(melody, 5)
const playback = playTune(melody, Infinity)
playback.end()

width();
height();
```



