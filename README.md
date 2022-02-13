# BingoCards
[LaTeX](http://tug.org/) and [python](http://www.python.org)-powered randomized bingo cards with custom cells. 
Takes a user-supplied list of phrases and randomly assigns them to a user-specified number of bingo cards, using LaTeX. 
A python script creates a complete checklist for all possible bingo card cells. 
This project is ideal for televised political events like State of the Union or presidential [or primary] debates.
New for 2022: also ideal for a Super Bowl party.

Although I made some changes to the LaTeX code, the bulk of the code -- and all of the hard stuff -- 
was taken from [tex.stackexchange.com](http://tex.stackexchange.com/questions/63357/automatically-generated-bingo-cards).

## Introduction

The list of phrases (and don't make any individual phrase very long) 
needs to be semicolon-separated text, with a small bit of LaTeX 
encompassing the list. You can make the list about anything; I have
added a vice-presidential list too, but the relevant words will always
change.

```LaTeX
\myItems{Believe me;
Make America Great Again;
Build a Wall;
Benghazi;
Crooked Hillary;
% [et cetera]
% note comments are allowed, as is whitespace
We don't \emph{win} anymore
}
```

:exclamation: **Important:** the list of items must _not_ include a final semicolon.
 LaTeX interprets a final semicolon to mean "the next entry is a blank", 
 and you will end up with randomly empty cells on the bingo cards.
The list should terminate only with a close-brace, though the brace can be on a new line.

The LaTeX file, when run, will take the list, pick 24 at random 
(the "Free Space" is humorously listed as _"Candidate Talks Past Allotted Time"_, 
as though that could ever _not_ happen), and assign them to a 
standard 5 x 5 bingo card. This random process is nice because it avoids 
"sets" of cards all providing Bingo at the same time. 

The LaTeX file includes a for-loop to create some number (initially set at 50) of PDF cards. 

## Thanks

I've gotten help over the years from a few people, including the above-mentioned StackExchange thread,
my spouse, and in 2022, from my Germanic friend Robin, who might prefer that I leave him out of this, but
he is a LaTeX wizard, and I know it.

## Customizing the Code

The bingo cards also include a small label below the bingo grid, so customize the ```\biglabel``` macro to suit your event.

```LaTeX
% make a nice identifier for the card (in case, say, there is more than one
% presidential debate, etc.)
\newcommand{\biglabel}{\vspace{0.2in}\begin{center}
\begin{LARGE}
Some Bingo Event

State of the Union

01 January 1900

\end{LARGE}
\end{center}
}
```

If you want to use an arbitrary "Free Space" label, you can change the 
[TikZ](https://en.wikipedia.org/wiki/PGF/TikZ) code:

```LaTeX
\node [scale=1.2] at ($(\col,-\row)-(0.5,0.3)$) {Something};
\node [scale=1.2] at ($(\col,-\row)-(0.5,0.5)$) {Else};
\node [scale=1.2] at ($(\col,-\row)-(0.5,0.7)$) {Entirely};
```

And note there is a `\renewcommand` or two you can uncomment for either a 
totally generic "Free Space" square, or a blindingly obvious topic/comment.

For those wholly new to TikZ, the second number of the pair is a y-coordinate 
for the line, and a line space of 0.2 is about right.
If you trim the cell to two lines, you will want to change the y-values 
to be more centered. 

## The Master Checklist

The python script parses the entire list and provides a ```longtable``` 
checklist for the moderator to use when evaluating a given event. 
The ```bingochecklist.tex``` file reads the output of the python script, 
which is a file called ```checklist1.tex```.

So, from a terminal on your machine of choice, run 

```shell
python makechecklist.py
```

And if you've got all the files where they are supposed to be, it will 
form up the guts of the checklist. 
Next, compile ```bingochecklist.tex``` a few times and you'll be good to go. 
You need to compile the TeX file at least twice because it uses 
```longtable```, and column widths are determined dynamically over 
the course of a few LaTeX compiles. 

Jesse Hamner, 2016-2022. :rocket:
