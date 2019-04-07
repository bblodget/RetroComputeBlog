---
layout: post
title: TI Fighter, a TuroboForth Game
---

Toward the end of the of last month's [RetroChallenge](http://www.retrochallenge.org/p/entrants-list-201903.html) 
I started looking at [TurboForth](http://turboforth.net/).  I was impressed with the TurboForth game
[Dark Star](http://turboforth.net/fun/darkstar.html). I thought it would be fun to make
my own game.  I used Dark Star as example code to get my game started. I borrowed
the rock tiles and the routines to draw them.

The game I chose to make is inspired by [Street Fighter](https://en.wikipedia.org/wiki/Street_Fighter_(video_game))
from 1987. My son, Josh, is into retro games and is helping with the game design and graphics.
Currently the game requires two joysticks.  Since development has just started
at this time you basically can just move the characters around and kick.
No collision is detected yet.

Here is a couple of snapshot of what we have so far.

![StandOff]({{ site.baseurl }}/images/2019-04-06/standoff.jpg "StandOff")

![BlockKick]({{ site.baseurl }}/images/2019-04-06/block_kick.jpg "BlockKick")

In this series of post I plan to blog about the development of this game.

## Running the Game

The source for [TI_Fighter is on github](https://github.com/bblodget/TI_Fighter).
Basically there are three files:
* __ti_fighter.fs__ : This is the source for the game written in TurobForth.
* __FIGHTER__ : This is the "block file" version of the source that can be loaded
by TurboForth.
* __move.py__ : This is a python script I wrote to convert ti_fighter.fs
into the block file FIGHTER.

### Using TIPI

I am using [TIPI](http://ti994a.cwfk.net/TIPI.html) as my disk solution for my TI-99/4A.
Using TIPI I am able to ssh into Raspberry Pi that is hosting the tipi_disk.  Then
from the tipi_disk directory I clone out the TI_Fighter repository.

```
pc> ssh tipi@tipi
...
tipi> cd tipi_disk
tipi> git clone git@github.com:bblodget/TI_Fighter.git
```

I then update tipi.config to have DSK3\_DIR set to TI\_Fighter.
After you update the tipi.config you need to reboot the PI.
Here is the top three lines from my tipi.config:

```
DSK1_DIR=TurboForth122.DSK1
DSK2_DIR=TurboForth122.DSK2
DSK3_DIR=TI_Fighter
```

The TurboForth122 DSK1 and DSK2 files I got from the 
[TurboForth Download page](http://turboforth.net/download.html).

Then from the TI-99/4A I start TurboForth and type:

```
S" DSK3.FIGHTER" USE
1 LOAD
FIGHTER
```

After you get the game started it is probably
best to "depress" the ALPHA LOCK key, so the joysticks
work correctly.

To break out of the game hit any key on the keyboard.

### Using Classic99 Emulator

The [Classic99 Emulator](http://www.harmlesslion.com/software/Classic99) 
is a TI-99/4A emulator for Windows. It comes with TurboForth as
one of the built-in cartridges.

To get TI_Fighter installed simply copy the block file __FIGHTER__
into DSK3 folder.  You can then load the game from TurboForth
in the same way:

```
S" DSK3.FIGHTER" USE
1 LOAD
FIGHTER
```

In Classic99 you can go to Options->Options to set the Joystick
configuration.  If you select "PC Keyboard" to emulate a
Joystick use the arrow keys to move, and Tab to fire.

## Development

My current development flow is to ssh into the TIPI Raspberry Pi
and use [vim](https://www.vim.org/) to edit __ti_fighter.fs__.
Then I run the __move.py__ to copy the data over to the 
FIGHTER block file.  Then from the TI I type:

```
RELOAD
1 LOAD
FIGHTER
```

The file __ti_fighter.fs__ starts with the following:

```
FORGET -->>

: -->> 0 EMIT [COMPILE] --> ; IMMEDIATE

: Reload ( --)
    \ Reloads FIGHTER block file
    S" DSK3.FIGHTER" USE
    ;
```

The first line "FORGET -->>" says forget the word "-->>"
and all words created after it.

The word "-->>" basically says compile the next block in the block
file.  Every block in the block file __FIGHTER__ ends with 
the the word "-->>" to load the next block.  That way you can
load the whole game using "1 LOAD".

The word "Reload" reloads the block file "FIGHTER".  When
you type "1 LOAD" the first thing it does is forgets
the previous definitions and then redefines them all.

I find this development flow easier than trying
to type using the TurboForth's block editor.

But there is probably a better method....

