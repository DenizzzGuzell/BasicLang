# BasicLang
CENG Programming Languages Term Project

The interpreter can be invoked as follows:

  $ python interpreter.py

Operators
  A limited range of arithmetic expressions are provided. Addition and subtraction have the lowest precedence, but this can be changed with parentheses.
  
  + - Addition
  - - Subtraction
  * - Multiplication
  / - Division
  MOD (or %) - Modulo

Commands
  Programs may be listed using the LIST command:
  
  > LIST
  10 LET I = 10
  20 PRINT I
  >
  
  A program is executed using the RUN command:
  
  > RUN
  10
  >
  A program may be saved to disk using the SAVE command. Note that the full path must be specified within double quotes:
  
  > SAVE "C:\path\to\my\file"
  Program written to file
  >
  The program may be re-loaded from disk using the LOAD command, again specifying the full path using double quotes:
  
  > LOAD "C:\path\to\my\file"
  Program read from file
  >
  
Architecture

The interpreter is implemented using the following Python classes:

  basictoken.py - This implements the tokens that are produced by the lexical analyser. The class mostly defines token categories and provides a simple token pretty printing method.
  
  lexer.py - This class implements the lexical analyser. Lexical analysis is performed on one statement at a time, as each statement is entered into the interpreter.
  
  basicparser.py - This class implements a parser for individual BASIC statements. This is somewhat inefficient in that statements, for example those in a loop, must be re-parsed every time they are executed. However, such a model allows us to develop an interactive interpreter where statements can be gradually added to the program between runs. Since the parser is oriented to the processing of individual statements, it uses a signalling mechanism (using FlowSignal objects) to its caller indicate when program level actions are required, such as recording the return address following a subroutine jump. However, the parser does maintain a symbol table (implemented as a dictionary) in order to record the value of variables as they are assigned.
  
  program.py - This class implements an actual basic program, which is represented as a dictionary. Dictionary keys are statement line numbers and the corresponding value is the list of tokens that make up the statement with that line number. Statements are executed by calling the parser to parse one statement at a time. This class maintains a program counter, an indication of which line number should be executed next. The program counter is incremented to the next line number in sequence, unless executed a statement has resulted in a branch. The parser indicates this by signalling to the program object that calls it using a FlowSignal object.
  
  interpreter.py - This class provides the interface to the user. It allows the user to both input program statements and to execute the resulting program. It also allows the user to run commands, for example to save and load programs, or to list them.
  
  flowsignal.py - Implements a FlowSignal object that allows the parser to signal a change in control flow. For example, as the result of a jump defined in the statement just parsed (GOTO, conditional branch evaluation), a loop decision, a subroutine call, or program termination. This paradigm of using the parser to simply parse individual statements, the Program object to make control flow decisions and to track execution, and a signalling mechanism to allow the parser to signal control flow changes to the Program object, is used consistently throughout the implementation.
  
