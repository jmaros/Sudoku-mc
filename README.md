# Sudoku-mc
Sudoku creator and solver command line tool
(Note: -mc
 VS2019 and VS2020 projects will create SudokuMC.exe instead of sudoku.exe
 in hounour of Michael Kennett, in fact it should be MK instead of MC, but
 for historical reasons I stick with my original mistakenly choosen name.
 To play the game there is no additional file, or command line argument
 needed, though you may specify some. The below description was made for
 the linux version, but it applies for the windows versions too, except
 for the filepath name conventions.
)

SYNOPSIS
       (play the game)
	      sudoku [options] ...  [<filename>]

       (generate boards)
	      sudoku -g [<num>] [options] ...

       (solve boards)
	      sudoku -v [options] ...

       (calculate statistics)
	      sudoku -s [options] ...


DESCRIPTION
       The  sudoku board game is played on a 9x9 grid, divided into rows, col-
       umns, and 9 blocks of 3x3 squares. The objective is to fill  the  empty
       squares	with  the digits 1-9, so that each row, column, and block con-
       tains each of the digits 1-9 (and hence, it is  not  possible  for  any
       digit to appear twice in the same row, column or block).


EXAMPLE
       Consider the following board, and the squares marked `a'-`h' and `x':

	 5 . a | 6 b 1 | . . .	     The digits appearing in each of the
	 7 9 . | . . . | c 6 8	     squares `a'-`h' can be uniquely
	 d 3 . | . 8 . | 7 . .	     determined. For example, the value
	-------+-------+-------      at `a' must be 8, since an 8 appears
	 . 5 . | 4 1 e | . . 2	     in the other rows and columns of the
	 . . 1 | f x . | 6 . .	     block. Using similar logic, it must
	 8 . . | . 3 7 | . 4 .	     be true that:
	-------+-------+------- 	  b = 7        f = 8
	 . . 4 | . 9 . | g 2 .		  c = 1        g = 8
	 2 8 h | . . . | . 9 7		  d = 1        h = 5
	 . . . | 5 i 8 | . . 6		  e = 6        i = 2

       In  contrast, it is not possible to uniquely determine the value of `x'
       with the given information - it could take either the value 2 or 5.

       The board now contains the squares:

	 5 . 8 | 6 7 1 | . . .	  It is now possible to determine the values
	 7 9 . | . . . | 1 6 8	  appearing in other empty squares.
	 1 3 . | . 8 . | 7 . .
	-------+-------+-------
	 . 5 . | 4 1 6 | . . 2
	 . . 1 | 8 x . | 6 . .	  <<< the value of x must now be 5.
	 8 . . | . 3 7 | . 4 .
	-------+-------+-------
	 . . 4 | . 9 . | 8 2 .
	 2 8 5 | . . . | . 9 7
	 . . . | 5 2 8 | . . 6

       Repeating this process a few more times reveals the solution:

	 5 4 8 | 6 7 1 | 2 3 9
	 7 9 2 | 3 4 5 | 1 6 8
	 1 3 6 | 9 8 2 | 7 5 4
	-------+-------+-------
	 3 5 7 | 4 1 6 | 9 8 2
	 4 2 1 | 8 5 9 | 6 7 3
	 8 6 9 | 2 3 7 | 5 4 1
	-------+-------+-------
	 6 1 4 | 7 9 3 | 8 2 5
	 2 8 5 | 1 6 4 | 3 9 7
	 9 7 3 | 5 2 8 | 4 1 6


GAME INTERFACE
       The sudoku game has  a  simple  text  interface	(using	the  curses(3)
       library).  The  board  is  displayed in the middle of the screen, along
       with a summary of the allowed key presses.  The	cursor	can  be  moved
       around  the  board  using the arrow keys or the standard vi(1) movement
       keys, and each square (except for the fixed board squares that are ini-
       tially  revealed)  can  be  set to a given digit by pressing the corre-
       sponding number key, or cleared by  pressing  either  the  `0'  or  `.'
       keys.

   Generating a New Board
       A  new  board can be generated at any time by pressing the `n' key, and
       either a precanned or randomly generated board will be  displayed.   If
       the  -n	command line option is set, then only precanned boards will be
       displayed.

   Entering a Custom Board
       A custom board (e.g. found on the internet, or published in  a  newspa-
       per)  can  be entered into the game by first clearing the current board
       (press the `c' key), entering the published squares (using  the	cursor
       motion  keys and entering the appropriate numbers), and then fixing the
       squares by pressing the `f' key. At this  point,  the  entered  squares
       will be fixed (and cannot be changed).

   Hints
       The interactive game provides a simple hint mechanism to provide assis-
       tance in solving the board. It attempts to highlight areas of the board
       where  moves  can  be made. If repeated hints are requested, the system
       starts revealing the digit that can be placed on the board.

       Often the hints can be quite cryptic. For example, consider  the  board
       below:

	   v v v

	   . . 7 | . . 9 | . . .
	   9 . 6 | 7 4 . | . 1 5
	   . . 2 | 5 1 . | . . .
	  -------+-------+-------
	>  6 . 5 | . 7 . | . . 8  <    The characters ><v^ highlight the
	>  . 7 . | . . . | . 3 .  <    area of the hint
	>  8 . . | . . . | 7 . 6  <
	  -------+-------+-------
	   . . . | . 6 7 | 8 . .
	   7 4 . | . 5 . | 9 6 2
	   . 6 . | 4 . . | . . .

	   ^ ^ ^

       The  system  gives  the hint `try the digit 3', but it is certainly not
       obvious, with the revealed squares, where the 3 goes.


OPTIONS
       -c<class>
	      Generate a board until it finds a board of the specified class.
	      Supported classes are: very easy, easy, medium, hard, and fiendish.

       -d     Describe the moves needed to solve the board. Can only  be  used
	      with the -v option for solving precanned boards.

       -f<format>
	      Set output format. The supported formats are:
		standard  Default text format; std is a shortcut.
		compact   Compact text format.
		csv	  Comma separated values, suitable for importing
			  into a spreadsheet.
		postscriptps is a shortcut.
		html	  Simple HTML.

       -g[<num>]
	      Generate	<num>  boards  (or just 1 board, if not specified) and
	      write them to standard output.

       -n     No random boards generated in the interactive game. Requires the
	      optional file of precanned boards to be specified.

       -r     Run in restricted mode, disallowing any games to be saved.

       -s     Calculate  statistics  for  the precanned boards, and attempt to
	      classify the difficulty of solving the boards. Can be used  with
	      the -v option.

       -t<filename>
	      Set  the template file. The file set on the command line will be
	      used instead of the default template file.

       -v     Solve precanned boards, writing the solution to standard output.

       <filename>
	      Name of the optional file containing precanned boards.

       -w     Write default template to the working directory if it doesn't
	      exist yet.


ENVIRONMENT
       No environment variables are used directly by the sudoku program.


FILES
       /usr/share/sudoku/template
	      Template file for generating new sudoku boards.

       /usr/share/sudoku/precanned
	      Optional file, containing `precanned' sudoku boards.


FILE FORMATS
   /usr/share/sudoku/template
       The  template  file  contains  a sequence of patterns that are used for
       generating new sudoku boards. Each pattern is started by a line with  a
       leading	`%'  character,  and  consists of 9 lines of 9 characters. The
       character `.' represents a square that  is  initially  blank,  and  the
       character  `*'  represents a square with a digit that is initially dis-
       played.

   Compact text format
       This format is similar to that of the template file, but contains  rep-
       resentations  of  game  boards.	Each board is started by a line with a
       leading `%' character, followed by an optional title for the board that
       is  displayed when the game is played. This is followed by 9 lines of 9
       characters, where the  character  `.'  represents  an  initially  empty
       square,	and  the  characters  `1'-`9'  give the value of a fixed board
       square that is initially displayed. The sudoku program  can  read  pre-
       canned  files  in  this	format, and will write them when the -fcompact
       option is set.

   Standard text format
       This format is very similar to the compact text	format,  but  includes
       additional  characters  to  delimit the blocks in the board. The sudoku
       program can read precanned files in this format,  and  writes  them  by
       default, unless another output format is set by the -f option.

   Comma separated text format
       This  format  is useful for importing sudoku boards into a spreadsheet.
       It represents each board by 9 lines of  comma  separated  fields.  Each
       field  is  blank,  or contains a digit.	The sudoku program cannot read
       precanned files in this format, and writes them when the  -fcsv	option
       is set. Unlike the standard or compact text formats, there are no lines
       separating boards, and hence, it is really only feasible to  store  one
       board per file.

   Postscript format
       This  format  is useful for printing out sudoku boards. The sudoku pro-
       gram cannot read boards stored in this format, and writes them when the
       -fpostscript  option  is  set. Unlike the standard or compact text for-
       mats, it is not possible to store multiple boards in the same file.

   HTML format
       This format is useful for printing out sudoku boards. The  sudoku  pro-
       gram cannot read boards stored in this format, and writes them when the
       -fhtml option is set. Unlike the standard or compact text  formats,  it
       is not possible to store multiple boards in the same file.


SEE ALSO
       There  are  a  large  number of websites dedicated to the sudoku puzzle
       that can be found easily using a search engine.	Some  of  these  sites
       provide	game  boards that can be challenging to solve, and others pro-
       vide strategies for finding moves.


DIAGNOSTICS
       There are limited diagnostics available when an error occurs.


ACKNOWLEDGEMENTS
       Mark Foreman for the HTML output format; Joanna Ferris and Heather  for
       encouraging this endeavour.


AUTHOR
       Michael Kennett (mike@laurasia.com.au)


COPYRIGHT
       This  manual  page, and all associated files, have been placed into the
       public domain by Michael Kennett, July 2005. They may be used  by  any-
       body  for  any  purpose	whatsoever,  however NO WARRANTY, of any sort,
       applies to this work.
