# Javascript declarations

what is a variables

> JavaScript is case-sensitive and uses the Unicode character set.

> The source text of JavaScript scripts gets scanned from left to right and is converted into a sequence of input elements which are tokens, control characters, line terminators, comments or whitespace


what is assignment by reference?

how does assignment work in javascript 

syntax

  - var varname1 [= value1 [, varname2 [, varname3 ... [, varnameN]]]];
  - valid characters? any legal identifier, any legal expression.

## global assignment

    y = 1

## var

    var x = 0 

  - declares a variable and optionally assigns it.
  - processed before any code is executed.
  - > The scope of a variable declared with var is its current execution context, which is either the enclosing function or, for variables declared outside any function, global.

effect of strict mode

> 1. Declared variables are constrained in the execution context in which they are declared. Undeclared variables are always global.


> 2. Declared variables are created before any code is executed. Undeclared variables do not exist until the code assigning to them is executed.

> Declared variables are a non-configurable property of their execution context (function or global). Undeclared variables are configurable (e.g. can be deleted).

ReferenceErrors

hoisting variables

    var x = y, y = 'A';
    console.log(x + y); // undefinedA

> Here, x and y are declared before any code is executed, the assignments occur later. At the time "x = y" is evaluated, y exists so no ReferenceError is thrown and its value is 'undefined'. So, x is assigned the undefined value. Then, y is assigned a value of 'A'. Consequently, after the first line, x === undefined && y === 'A', hence the result.

    var x = 0;

    function f(){
      var x = y = 1; // x is declared locally. y is not!
    }
    f();

    console.log(x, y); // 0, 1
    // x is the global one as expected
    // y leaked outside of the function, though! 


## let

> let allows you to declare variables that are limited in scope to the block, statement, or expression on which it is used. This is unlike the var keyword, which defines a variable globally, or locally to an entire function regardless of block scope.

> Variables declared by let have as their scope the block in which they are defined, as well as in any contained sub-blocks . In this way, let works very much like var. The main difference is that the scope of a var variable is the entire enclosing function:

    var x = 'global';
    let y = 'global';
    console.log(this.x); // 'global'
    console.log(this.y); // undefined

> Redeclaring the same variable within the same function or block scope raises a SyntaxError.

- no effective hoisting

## const

> The const declaration creates a read-only reference to a value. It does not mean the value it holds is immutable, just that the variable identifier cannot be reassigned.

> This declaration creates a constant that can be either global or local to the function in which it is declared. An initializer for a constant is required; that is, you must specify its value in the same statement in which it's declared (which makes sense, given that it can't be changed later).

> Constants are block-scoped, much like variables defined using the let statement. The value of a constant cannot change through re-assignment, and it can't be redeclared.

> All the considerations about the "temporal dead zone" that apply to let, also apply to const.

> A constant cannot share its name with a function or a variable in the same scope.

Therefore you can't declare without assignment.

    var foo     //  all good
    let bar     //  good too
    const baz   //  sorry