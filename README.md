![image](./Greek_Phalanx.jpg)

Phalanx is a chess playing program
Copyright (c) 1997, 1998, 1999, 2000, 2014 Dusan Dobes


LICENSE AND WARRANTY

- Phalanx is free software; you can redistribute it and/or modify it
  under the terms of the GNU General Public License as published by
  the Free Software Foundation; either version 2, or (at your option)
  any later version.
- Phalanx is distributed in the hope that it will be useful, but
  WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  GNU General Public License for more details.
- You should have received a copy of the GNU General Public License
  along with Phalanx; see the file COPYING.  If not, write to
  the Free Software Foundation, 59 Temple Place - Suite 330, Boston,
  MA  02111-1307, USA.


WHERE TO GET PHALANX

- http://sourceforge.net/projects/phalanx/
- ftp://sunsite.unc.edu/pub/Linux/games/strategy/   (Only sources)

  Please let me know if there are others, i will update this list.
  Feel free to put Phalanx on your ftp site.


COMPILING, PORTABILITY

Compiling is simple, at least under Linux. Just type `make'.  You might want
to edit the optimization flags in makefile to produce faster binary.  Phalanx
is developed under GNU C, GNU Debugger, and GNU Make.  If your system does
not support GNU exensions (e.g. long options), remove '-DGNUFUN' from DEFINES
in makefile.  If you have incompatible 'make', try this:
$ cat *.c > allphalanx.c; cc -O allphalanx.c -o phalanx


INTERFACE, COMMAND LINE OPTIONS

Phalanx is xboard compatible.  Running with xboard: 'xboard -fcp phalanx'.
Note that permanent brain (pondering) is off by default.  Newer versions of
xboard set it on with the 'hard' command.  If this does not work, try
'xboard -fcp "phalanx -p+"' or (for <4.0.0 versions of xboard) change your
initString (see Xboard documentation for details).  It's better to stop
permanent brain in both programs, when playing Phalanx against another
program on a machine with one CPU.
It's also possible to run phalanx without xboard. Do "phalanx -h" to get
a list of command line options. One important command of phalanx's ASCII
interface is "help".


MORE ABOUT INTERFACE

I'm trying to write an interface that fits following three requirements:
- Xboard compatibility.  For best results, get the latest version of Xboard.
- shell-like interface that allows running commands in a batch. It's very
  useful for testing. Example: look into the file test.fin. It's a set
  of chess problems and solutions. You can simply send this file to
  Phalanx's stdin:
  $ phalanx -c+ -o- -b- -f10 < test.fin | tee result
  ( Where: -c+ .... use cpu time
  -o- .... don't use polling input
  -b- .... no opening book
  -f10 ... fixed time 10 seconds per move )
  Watch how it works.
- Acceptable ASCII interface.


COMMAND LINE OPTIONS

Usage:
phalanx [options] [<moves> <minutes> [<increment in seconds>]]
phalanx [options] [<seconds per move>]
phalanx bench
phalanx bcreate [book creation options]

Options:
-t <transposition table size in kilobytes>
Transposition table really needs to fit in the RAM memory, it should
never be swapped to disk. Maximum size is your total RAM minus space
needed for OS (8-20 MB) minus space for other (possible) applications.
-f <fixed search time in seconds>
-x <+/->  xboard mode on/off                  default: on
"Xboard mode off" is designed for the ascii interface. In -s+ mode it
shows also the move that is currently searched.
-p <+/->  permanent brain on/off              default: off
Phalanx ponders on a predicted move with the permanent brain on. It is
sometimes called pondering. Xboard sets it on with the 'hard' command
regardless of this option, just use xboard menu to set it on/off.
-s <+/->  show thinking on/off                default: off
Phalanx shows depth, evaluation, time used in centiseconds, nodes
searched, and principal variation (best line) when searching. Xboard
overrides this option with 'post' and 'nopost' commands, just use
xboard menu instead of this.
-c <+/->  cpu time                            default: off
This one defaults to the "wall clock" real time. It's better to use
the CPU time when running test suites.
-o <+/->  polling input                       default: on
Use -o- for running test suites. Phalanx reads the positions from its
standard input and polling input makes it stop thinking after almost
zero seconds on each position.
-b <+/->  opening book                        default: on
-l <+/->  learning on/off                     default: off
-r <resign value in centipawns>               default: 0 (no resigning)
-e <easy level 0...99>                        default: 0 (best play)
1 is the hardest and 99 is the easiest easy level. Phalanx tries to
emulate human-like blunders, the higher the number the more blunders it
plays. It also adds more randomness with the easy levels, repeating
games should be impossible. Easy levels set hashtable size to zero,
pondering and learning to off. Since version XXIII, Phalanx uses
the time normally, it does not respond immediatelly. NPS is lowered
to 100-300, unless overriden by the -n agrument. Root moves randomizing
is used here as well, between 10 to 60 centipawns, unless overriden
by the -z argument.
-z <random evaluation in centipawns>          default: 0 (best play)
Randomize play. Add pseudo-random values to root moves evaluations.
The numeric value limits the random range, the interval of random
evaluations to be added is [-N/2 ... N/2].
-z <random evaluation in centipawns>:<N>
Like above, but only randomize first N moves. This is to avoid repeating
lines in opening, while keeping almost the same playing strength. For
example, -z 20:10 will randomize first 10 moves in the game by
20 centipawns.
-n <nodes per second>                         default: 0 (no limit)
Limits the speed to weaken the engine and to use less resources: The
speed of the machine does not matter here, it uses usleep() during
the search, so with low NPS it does not raise the machine load.
-v
Print version and exit.
-P <primary book directory>
-S <secondary book directory>
-L <learning file directory>
-g <log file name>

Book creation options:
-b <buffer size in cells>
Stage 1. One cell is 10 bytes. Twice the space is needed for sorting
the buffer, use no more than 1/3 of your total RAM.
-p <maxply>
Stage 1. Stop parsing the game at given ply (halfmove). Default value is
70, it's 35 full moves.
-c <max length of comment>
Stage 1. PGN files have comments in {}. Phalanx skips the comments when
parsing. Big PGN files also have lots of errors, sometimes '{' does not
have matching '}'. So, if the comment goes over the maximal length, we
suspect the text is no more the comment and continue parsing. Default
value is 4096 bytes.
-w <winning move value>
Stage 1. We are adding the [position,move] to the database. The side to
move won the game. The appropriate database entry gets <winning move
value> bonus. Default is 5 points. Use values in 1-10 range.
-d <drawing move value>
Stage 1. Same as the previous except that the game was drawn. Default
value is 2 points.
-l <losing move value>
Stage 1. Same as the previous except that the side to move lost the
game. Default value is 1 point.
-u <unknown result move value>
Stage 1. Same as the previous except that there is no result of the game
in the PGN file, sad and common case. Default value is 1 point.
-v <min value of move to add>
Stage 2. Moves that have less total points than this value are not
added into the sbook.phalanx. Bigger numbers create smaller books.
The smallest reasonable number is 10. Default is 15.
-g <min value percentage of best move to add others>
Stage 2. Use values in 50-100 range. The 2nd, 3rd, and other moves are
stripped if their value is less than G% of the value of the best move.
The G is decreased in positions where the 1st move value has high enough
value to give more variability in frequently played opening, e.g. we
want a bit more than just sicilian, maybe 1. ... e5 is also good.
50 gives lots of variability and maybe dubious moves, 100 gives almost
no variability and only the highest valued moves. Default is 80.

Note that the book creation does not work with DOS end-of-lines in the PGN
input, just use dos2unix filter if needed.

Examples: phalanx -c+ -s+ -o - -x- -f 60 -t4000
xboard -fcp "phalanx -r800"
xboard -fcp 'phalanx -e 99'


ASCII INTERFACE COMMANDS

about            about Phalanx, show version, copyright info, and settings
bd               display position (same as 'd')
bk               show book moves (xboard) - shows ECO code/name and all moves
from both primary and secondary opening books. The last line
is the text string that is used to identify the position in
the pbook.phalanx. Primary book moves are shown along with
their probabilities. There is no such probability info for
the secondary book moves, as all secondary book moves have
equal probability.
book             enable/disable opening book
both             machine plays both
depth            set search depth in plies (xboard). Search will be stopped
at given depth, no timing info is used.
fen              display position in FEN
force            user plays both (xboard)
go               switch sides, start computing (xboard)
history          show game moves in full notation
level N M I      set level to N moves in M minutes, increment I seconds.
Phalanx needs its time info have updated from xboard via
the 'time' command to make this level work well.
level N          set level to fixed time N seconds per move
new              new game (xboard)
post             show thinking (xboard)
remove           take back last move, two plies (xboard)
nopost           do not show thinking (xboard)
quit             quit (same as 'exit' or end of file character)
score            show static evaluation
time <N>         remaining time is N/100 s (xboard)
undo             undo last ply (xboard; same as 'u')
<FEN position>   set test position, start search, show result
#                comment


OPENING BOOK

From version VI, there are two book files - primary (pbook.phalanx), and
secondary (sbook.phalanx).  A position is first searched in pbook.phalanx.
Only if it's not found there, sbook.phalanx is searched.  You can specify
book directories via command line (-P, -S) or use environment variables
PHALANXPBOOKDIR and PHALANXSBOOKDIR.  Otherwise Phalanx tries to find its
book files in current directory (./book.phalanx, ./sbook.phalanx) and finally
in compiled-in directory (/usr/local/lib/phalanx).  You can change the
compiled-in directory in makefile.
- pbook.phalanx is 'hand'-written, text book.  One line per position, sorted.
  This time, it's bigger than really needed, because it was the only book
  file till version V.  The size will be smaller and the line format might
  change to EPD+SAN in future.
- sbook.phalanx is binary book, generated from large PGN files.  Six bytes
  per move (4 hash key, 2 move).  You can generate your own sbook.phalanx
  with 'phalanx bcreate <options>', like this:
  $ ./phalanx bcreate < manyGMgames.pgn
  Book creation has two stages. First stage reads and parses PGN from
  standard input and creates the 'rbook.phalanx' file. If rbook.phalanx
  already exists, the first stage is skipped. Second stage filters positions
  and moves from the rbook.phalanx to sbook.phalanx. First stage needs about
  30 times more time than the second one. Look at 'book creation options' for
  more details on creating your sbook.phalanx.


ECO DATABASE

From version XX, Phalanx has ECO database.  Command 'bk' can also print ECO
code and opening/variation name.  This works with xboard 4.0 via
"Help"/"Book" menu item.  If you want to activate this feature, you must type
the 'bk' command (or click the "Help"/"Book" menu item in xboard) once per
session in the initial position.  Then, the ECO index is created (that might
take few seconds on a slow machine).  The ECO index is based on positions
rather than on moves so transpositions between openings can take place.  The
eco.phalanx file is taken from Gnuchess distribution (originally eco.pgn, but
this is not real pgn).


INSIDE THE MACHINE

Phalanx uses (traditional) 10x12 board implementation.  There are three
often used board implementations: "8x8" (GNU Chess), "bitboard" (Crafty),
and "10x12" (Nimzo, Phalanx).  In short, "10x12" is easy to implement and
the code and basic data structures are small ( == fast on PC).  The engine
uses many well known techniques: PVS (principal variation search),
transposition/killer table, static-eval cache, history killers, SEE (static
exchange evaluator), null move pruning, forward pruning, internal iterative
deepening, chess-specific extensions.


AUTHOR

Dusan Dobes
