(https://briancallahan.net/blog/20210814.html)

# What Our Compiler Will Be And NOT Be
	- We will be writing a "Single-Pass" compiler 
	[This is a wiki link](https://en.wikipedia.org/wiki/One-pass_compiler)

# A Language to Compile
	- Pascal was developed specifically for teaching programming and was designed to be parsed 	       with a single-pass compiler
	- PL/0 was designed specifically for teaching comiler construction
	 (https://en.wikipedia.org/wiki/PL/0)
	- This PL/0 compiler will be implemented in C (SSSOOOO COOOL!)


# A Basic Walkthrough of Our Compiler

---------------------------------------------------------------------

+-------+
| Start |
+-------+
    |
    +---+
        |
        V
+----------------+    +------------+      -------      +--------+
| Read in a line |    | Parse into |     /       \  No | Write  |
|                |--->|            |--->| Pass 1? |--->|        |
| of assembly    |    | tokens     |     \       /     | binary |
+----------------+    +------------+      -------      +--------+
        ^                                    |             |
        |                                    | Yes         |
        |                                    |             |
        |                                    V             |
        |                               +---------+        |
        |                               | Record  |        |
        |                               | address |        |
        |                               +---------+        |
        |                                    |             V
        |                                    |       -----------
        |                                    |  No  / End of    \
        +------------------------------------+-----|             |
                                                    \ assembly? /
                                                     -----------
                                                           |
                                                           | Yes
                                                           |
                                                           V
                                                        +-----+
                                                        | End |
                                                        +-----+

--------------------------------------------------------------------


# Here is the syntax for PL/0:
--------------------------------------------------------------------
program		= block "." .
block		= [ "const" ident "=" number { "," ident "=" number } ";" ]
		  [ "var" ident { "," ident } ";" ]
		  { "procedure" ident ";" block ";" } statement .
statement	= [ ident ":=" expression
		  | "call" ident
		  | "begin" statement { ";" statement } "end"
		  | "if" condition "then" statement
		  | "while" condition "do" statement ] .
condition	= "odd" expression
		| expression ( "=" | "#" | "<" | ">" ) expression .
expression	= [ "+" | "-" ] term { ( "+" | "-" ) term } .
term		= factor { ( "*" | "/" ) factor } .
#factor		= ident
		| number
		| "(" expression ")" .
#ident		= "A-Za-z_" { "A-Za-z0-9_" } .
#number		= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" .

--------------------------------------------------------------------

# Now Lets Consider a Rough Workflow of Our Compiler

--------------------------------------------------------------------

#+-------+
#| Start |
#+-------+
    |                                      No
    +--+                   +------------------------------+
       |                   |                              |
       V                   |                              |
#+-------------+            V                        ------------
#| Read in     |   +-------------+    +-------+     / Ready to   \
#|             |-->| Lex a token |--->| Parse |--->|              |
#| source code |   +-------------+    +-------+     \ emit code? /
#+-------------+            ^                        ------------
                           |                              |
                           |          +-----------+  Yes  |
                           |          | Emit code |<------+
                           |          +-----------+
                           |No              |
                           |                |
                           |                V
                           |          --------------
                           |         / End of       \  Yes +-----+
                           +--------|                |---->| End |
                                     \ source code? /      +-----+
                                      --------------
--------------------------------------------------------------------

- There are three main parts to this compiler
	* The Lexer
	* The Parser
	* The Code Generator

# The LEXERs job:
	* It starts by taking in the free form source code and and turn it into  a series of TOKENs




