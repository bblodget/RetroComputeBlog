---
layout: post
title: TurboForth, Part 1
---

Now that I have a disk storage solution, with [TIPI](https://bblodget.github.io/RetroComputeBlog/TIPI/), for my TI99/4a,
I am excited to try [TurboForth](http://turboforth.net/).  I bought a TurboForth cartridge.
When it boots up it says it is V1.2.3.

![TurboForth]({{ site.baseurl }}/images/2019-03-21/turboforth.jpg "TurboForth")

## Quick [Forth Tutorial](http://turboforth.net/tutorials/tutorials.html)

In Forth you define words.  Words are made up out of other words.
You separate words with whitespace such as ' ' or newline.
Data/parameters are passed between words using the [stack](http://turboforth.net/tutorials/the_stack.html).
If it looks like a number or a literal Forth will
push it on the stack.  If it looks like a word
Forth will try an execute it.  That's the bulk of it!

In Forth [:](http://turboforth.net/lang_ref/view_word.asp?ID=144) is the word for defining a new word.
[;](http://turboforth.net/lang_ref/view_word.asp?ID=143) is the word that indicates the end of a definition.
Also most words take parameters from the stack.
So you must push the parameters onto the stack before calling
the word.  In Forth there is a convention for documenting the
parameters a word expects on the stack and the values
it leaves on the stack.  Basically it is ( param1 param2 -- ret1 ret2).
This is called the Data Stack Signature.
So the Data Stack Signature for [+](http://turboforth.net/lang_ref/view_word.asp?ID=47) would be ( a b -- a+b).
It so happens that [\(](http://turboforth.net/lang_ref/view_word.asp?ID=118) is a word that begins
a comment so you often
see the Data Stack Signature to the right of the word when it is defined.

Another handy word is [.](http://turboforth.net/lang_ref/view_word.asp?ID=205)
which pops the top value off of the stack and prints it.

So the following adds 1 + 1 and prints the result

```
1 1 + .
```

The following defines a word that increments the value on the top
of the stack by 2.

```
: add2 ( a -- a+1)
2 +
;
```

## Creating a new Block file

Blocks and Block files are described in the
[TurboForth blocks tutorial](http://turboforth.net/tutorials/blocks.html)

A Block can be thought of as a type of virtual memory.
A block is 1024 bytes (or 1KB).  It can be flushed to disk or loaded
into memory.  Blocks can be edited using TurboForth's built in editor.
It is a convenient way to store source code, but can
be used to store binary data as well.

A Block file is a file on disk that stores a number of blocks.
I created a new Block file on disk (TIPI filesystem) for storing my words.

The following commands create a block file named **MYBLOCKS** 
that holds 40 blocks (40KB) on DSK1 and tells TurboForth to use it by default.

```
40 MKBLK DSK1.MYBLOCKS
S" DSK1.MYBLOCKS" USE
```


## Border: First Program

For my first program my goal was to draw a border of asterisks \*
around the screen, then wait for a keypress before showing the
'ok:' prompt.

I have some experience with Forth, so I jumped straight to the 
[Graphics Words](http://turboforth.net/lang_ref/words_by_glossary_category.asp?ID=16).
I also looked at the [graphics tutorial](http://turboforth.net/tutorials/graphics.html).

First I defined a word, **CLS**,  that clears the screen.  This sets
the graphics mode to 0.  This mode is a 40 column text mode (2 colors).
Calling [GMODE](http://turboforth.net/lang_ref/view_word.asp?ID=219) has the side effect of clearing the screen. 


```
: CLS
0 GMODE
;
```

Next I defined the words, **TOP_LINE** and **BOT_LINE**.
These words prints 40 \* across the top and bottom of the screen.  
This is done with the [HCHAR](http://turboforth.net/lang_ref/view_word.asp?ID=220) command.  
Its Data Stack Signature is ( row col ascii_num repeat -- )
The asci_num for asterisks \* is 42.

```
: TOP_LINE
0 0 42 40 HCHAR
;

: BOT_LINE
23 0 42 40 HCHAR
;
```

Then define the words **LEFT_LINE** and **RIGHT_LINE**
for the sides. Here we use [VCHAR](http://turboforth.net/lang_ref/view_word.asp?ID=232) 
instead of [HCHAR](http://turboforth.net/lang_ref/view_word.asp?ID=220) to draw
vertical lines.


```
: LEFT_LINE
0 0 42 24 VCHAR
;

: RIGHT_LINE
0 39 42 24 VCHAR
;
```

The we define the word to draw the full border.
After we draw the border we move the cursor
to the middle of the screen with the [GOTOXY](http://turboforth.net/lang_ref/view_word.asp?ID=131) command.
Then we wait for a [KEY](http://turboforth.net/lang_ref/view_word.asp?ID=133) press.  We don't care
which key was pressed so we [DROP](http://turboforth.net/lang_ref/view_word.asp?ID=17) the value.

```
: BORDER
CLS
TOP_LINE BOT_LINE LEFT_LINE RIGHT_LINE
20 12 GOTOXY
KEY
DROP
;
```

Here is what the program looks like typed into
the block editor.

![border_code]({{ site.baseurl }}/images/2019-03-21/border_code.jpg "Border Code")

And here is what it looks like when the word **BORDER** is run.

![border_run]({{ site.baseurl }}/images/2019-03-21/border_run.jpg "Border Run")



