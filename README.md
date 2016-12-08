# FRIBBLE - Using SPITBOL to trifle with words and to play Words With Friends

Fribble is a collection of SPITBOL programs related to word games such as Scrabble and Words With Friends (WWF).

The goal is to construct a program that can play WWF.

A copy of the offical rules for WWF can be found in Appendix A.

##Strategy

Our strategy is at its core "brute force" in that each
new move will be determined by generating **all** possible moves and
then picking one that has a score at least as high as any other.

The reasoning behind this approach is based on the observation
that each new move will consists of at most about 10000,
possible permutations of the tiles in the rack that can be placed
starting at a given blank cell.
If we consider all moves, then there are 7! (about 5,000) ways to play seven tiles, 6! to
play six tiles, and so forth. This gives about 7! + 6! ... +1! possibilities (about 10,000).

As the game progresses there will be fewer and fewer blank cells to consider.

While this at first sight a large space to search, the number of impossible moves is
limited by the size of the dictionary (about 180,000 words) and 
the structure of English. For example, consider the number of plural words in the dictionary, that is words such adding 's' at the end of
the word results in a word that is also in the dictionary.
There are just under 50,000 plurals in the ENABLE list, which is about 30% of the total.

Almost all the possible moves will consist of nonsense strings
that are  not in the dictionary. Consider for example a  move
that starts with 'zqiij.' We can find such nonsense strings
by constructing a list of all the possibilites for the first
four characters in a valid word, and so forth.

SPITBOL is very fast. The game is now played in real time, but
over the internet, so that it is acceptable to take minutes,
if not hours, to find the best move for a given position.






The WWF board is a 15x15 grid, with rows 1..15 and columns 1..15.

The game is played with tiles, each representing a letter, and 'blank' tiles that can be used
to represent any letter.  Each player has at most seven tiles at any turn. We refer to these tiles as the 'rack.'

At the end of a turn the board must contain only words that occur in the WWF dictionary.

In SPITBOL terms, we can think of the board as two sets of lines, with each line having 15 characters. The
rows form one set of lines, the columns form the other. So at the end of a turn -- reading from left to right for
the rows and top to bottom for the columns -- each line must contain only words in the WWF dictionary.

We need only maintain the list of rows, since the columns can be obtained once the rows are known. 
We define the initial board with

	rows = array(16)
board.1
	gt(i = i + 1, 15)			:s(board.2)
	rows[i] = dupl'-',15)			:(board.1)
board.2


Need function to check a line for validity:

	define('valid(line)...') which succeeds if line is all blank or contains only valid words, and fails
				otherwise.


util.sbl summary

	add(str,word add word to string prefixing with space if string not null
	backwords(dict) - return dictionary with words reversed
	getcolumns(rows) - return columns as array of lines corresponding to the columns
	getwords(filename) - read list of words in file
	getdict(filename) - read list of words from file and build table mapping words to non-zero value
	isword(word,words)) - return nonzero value if word is contained in table words
	less(str,ch) - return character from string
	prefix(s,p) - insert p as prefix to each word in s
	words(s) return number of words (separated by spaces) in s
	
Representation of board as two-dimensional array
	board = array(15,15)

	getrows(board)
	getcolumns(board)

# Study

The directory *study* contains various programs used to study the dictionary structure, to assist in
refining the brute force approach to make the WWF player faster.



#Appendix A - Words with Friends Rules

The Offical Rules for WWF can be found at:

http://www.zygnawithfriends.com/wordswithfriends/support/WWF_Rulebook.html

1. Overall Objective

    Players exchange turns forming words horizontally or vertically on the board, trying to score as many points as possible for each word.

2. Tile Placement

  1. The first word must be placed so that one of the tiles is on the star in the center of the board.

  2. Every word following that must be placed so that at least one tile is shared from an existing word on the board.

  3. Tiles can only be placed in the same line vertically or horizontally each turn. (The rules don't state,
 but experience confirms, that no spaces are allowed between the placed tiles.)

  4. Tiles can be placed so that multiple new words are formed simultaneously using neighboring letters.

  5. Words cannot be placed if they create an illegal word using neighboring letters.

  6. All words labeled as a part of speech (including those listed of foreign origin, and as archaic, obsolete, colloquial, slang, etc.)
       are permitted with the exception of the following: proper nouns (words always capitalized), abbreviations, prefixes and suffixes
       standing alone or words requiring a hyphen or an apostrophe.

3. Scoring

  1. Double the value of any tiles that were played this turn on a Double Letter (DL) space, and triple the value of 
any tile that was played on a triple letter (TL) TL space this turn. 
Do not double the value of tiles on DL and TL spaces for tiles that were played on previous rounds.

  2. Add up the values of all letters in the word, even if some were played on a previous turn.

  3. Double the value of the word if any tiles this turn were played on a DW space (and double it again 
in the case were two DW spaces were played upon). Triple the value of the word if any tiles this turn were played on a 
TW space (and triple it again if two TW spaces were used). Do not multiply words if tiles on DW or 
TW spaces were used from a previous turn.

  4. It is possible to create multiple words with the same play. In this case, score each new word separately,
 including bonuses, and sum all of the new words together.  

  5. Words cannot be placed if they create an illegal word using neighboring letters.

  6. Thirty-five bonus points are awarded whenever a player uses all seven tiles on their rack in a single turn.

4. End Game

  1. The game ends when one player plays every tile in his rack, and there are no tiles remaining to draw from.
The game could also end if three successive turns have occurred with no scoring and as long as the score is not zero-zero.

  2. After the last tile is played, the opposing player will lose points equal to the sum of the value of his remaining tiles.
This amount is then awarded to the player who placed the last tile.

5. Dictionary

Words With Friends has more than 173,000 acceptable words for use in the game. Our list is based on the Enhanced North American Benchmark

   Lexicon (ENABLE), a public domain list used by many word games.  

The ENABLE word list for WWF can be found at:

http://code.google.com/archive/p/dotnetperls-controls/downloads/enable1.txt

WWF also allows some additional words though this is no official list of them.  For example, consider QI 
(force whose existence and properties are the basis of much Chinese philosophy and medicine).

A list of such words, created by peusing some web sites and also based on actual playing experience, can be
found in the definition of the variable *g.wwf* in file *util.sbl*.




