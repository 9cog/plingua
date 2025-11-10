# P-Lingua Language Reference

## Overview

P-Lingua is a domain-specific language designed for defining membrane computing systems (P-systems). This reference provides complete syntax and semantics documentation.

## Table of Contents

1. [Lexical Structure](#lexical-structure)
2. [Data Types](#data-types)
3. [Expressions](#expressions)
4. [Statements](#statements)
5. [Membrane Structures](#membrane-structures)
6. [Rules](#rules)
7. [Patterns and Models](#patterns-and-models)
8. [Modules](#modules)
9. [Directives](#directives)
10. [Grammar Reference](#grammar-reference)

---

## Lexical Structure

### Comments

```plingua
// Single-line comment

/* Multi-line
   comment */
```

### Identifiers

```ebnf
identifier ::= (letter | '_' | '.') (letter | '_' | '.' | digit)*
letter     ::= 'a'..'z' | 'A'..'Z'
digit      ::= '0'..'9'
```

Examples:
```plingua
myVariable
protein_kinase
x.y
_temp
cell_1
```

### Literals

#### Integer Literals

```plingua
42          // Decimal
0x2A        // Hexadecimal
0X2a        // Hexadecimal (case-insensitive)
```

#### Floating-Point Literals

```plingua
3.14
2.5e10
1.0E-5
.5          // Equivalent to 0.5
```

#### String Literals

```plingua
"Hello, World!"
"Escaped quote: \" "
"Newline: \n"
"Tab: \t"
```

#### Boolean Literals

```plingua
true        // Equivalent to 1
false       // Equivalent to 0
```

### Keywords

Reserved keywords that cannot be used as identifiers:

```
def         let         call        system      return
if          else        while       do          for
true        false       int_t       long_t      double_t
string_t    prob_t      rexp_t      @include    @model
@mu         @ms         @d
```

### Operators

```
Arithmetic: +  -  *  /  %
Comparison: <  <= >  >= == !=
Logical:    && || !
Bitwise:    &  |  ^  ~  << >>
Assignment: =  += -= *= /= %= &= |= ^= <<= >>=
Increment:  ++ --
```

### Delimiters

```
( )    Parentheses
[ ]    Square brackets (membranes)
{ }    Curly braces (blocks, indices)
' '    Single quotes (labels)
,      Comma
;      Semicolon
:      Colon
-->    Arrow (rules)
<-->   Double arrow (communication)
@      At symbol (directives)
#      Empty multiset
?      Question mark (patterns)
```

---

## Data Types

### Primitive Types

#### Integer Types

```plingua
int_t x;        // 32-bit signed integer
long_t y;       // 64-bit signed integer
```

Range:
- `int_t`: -2,147,483,648 to 2,147,483,647
- `long_t`: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807

#### Floating-Point Types

```plingua
double_t pi = 3.14159265359;
```

Range: IEEE 754 double-precision (64-bit)

#### String Type

```plingua
string_t name = "membrane";
```

Strings are immutable sequences of characters.

#### Probability Type

```plingua
prob_t p = 0.75;
```

Special type for probabilities, range [0.0, 1.0].

#### Regular Expression Type

```plingua
rexp_t pattern = "^protein_[0-9]+$";
```

Used for pattern matching in advanced features.

### Composite Types

#### Objects

Objects are symbolic identifiers with optional indices:

```plingua
a               // Simple object
protein         // Named object
x{i}            // Single-indexed object
edge{i,j}       // Multi-indexed object
molecule{i,j,k} // Three-indexed object
```

#### Multisets

Unordered collections of objects with multiplicities:

```plingua
a, b{2}, c{3}   // a×1, b×2, c×3
#               // Empty multiset
```

**Operations:**
- Addition: Merge multisets
- Subtraction: Remove objects
- Containment: Check if objects present

#### Labels

Hierarchical identifiers for membranes:

```plingua
'skin                 // Single-level label
'nucleus.inner        // Two-level label
'cell{i}.organelle{j} // Indexed label
```

#### Membranes

Hierarchical structures containing objects:

```plingua
[ multiset ]'label              // Simple membrane
[ a, b ]'outer [ c ]'inner      // Nested membranes
[ ]'cell^+                      // Charged membrane
```

---

## Expressions

### Arithmetic Expressions

```plingua
x + y       // Addition
x - y       // Subtraction
x * y       // Multiplication
x / y       // Division (integer if both operands are integers)
x % y       // Modulo
-x          // Unary minus
+x          // Unary plus
```

**Precedence** (highest to lowest):
1. Unary operators: `+ - ! ~`
2. Multiplicative: `* / %`
3. Additive: `+ -`
4. Shift: `<< >>`
5. Relational: `< <= > >=`
6. Equality: `== !=`
7. Bitwise AND: `&`
8. Bitwise XOR: `^`
9. Bitwise OR: `|`
10. Logical AND: `&&`
11. Logical OR: `||`

### Comparison Expressions

```plingua
x < y       // Less than
x <= y      // Less than or equal
x > y       // Greater than
x >= y      // Greater than or equal
x == y      // Equal
x != y      // Not equal
```

Result: Boolean (`true` or `false`)

### Logical Expressions

```plingua
x && y      // Logical AND (short-circuit)
x || y      // Logical OR (short-circuit)
!x          // Logical NOT
```

### Bitwise Expressions

```plingua
x & y       // Bitwise AND
x | y       // Bitwise OR
x ^ y       // Bitwise XOR
~x          // Bitwise NOT
x << n      // Left shift
x >> n      // Right shift
```

### Conditional Expression

```plingua
condition ? expr1 : expr2
```

Evaluates to `expr1` if condition is true, otherwise `expr2`.

### Call Expression

```plingua
call functionName(arg1, arg2, ...)
```

Invokes a user-defined module.

### System Call Expression

```plingua
system functionName(arg1, arg2, ...)
```

Invokes a built-in system function.

---

## Statements

### Variable Declaration

```plingua
let identifier = expression;
let x = 10;
let name = "value";
```

Variables are automatically typed based on the initializer.

### Assignment Statement

```plingua
identifier = expression;
x = 42;
y += 10;
z *= 2;
```

Compound assignment operators:
```plingua
+=  -=  *=  /=  %=  &=  |=  ^=  <<=  >>=
```

### Expression Statement

```plingua
expression;
x++;
call doSomething();
```

### Block Statement

```plingua
{
    statement1;
    statement2;
    ...
}
```

Creates a new scope for variables.

### If Statement

```plingua
if (condition) {
    // statements
}

if (condition) {
    // true branch
} else {
    // false branch
}
```

### While Loop

```plingua
while (condition) {
    // statements
}
```

### Do-While Loop

```plingua
do {
    // statements
} while (condition);
```

### For Loop

```plingua
for (initialization; condition; increment) {
    // statements
}

// Example
for (let i = 0; i < 10; i++) {
    // loop body
}
```

### Return Statement

```plingua
return;              // Return from void function
return expression;   // Return value
```

---

## Membrane Structures

### Membrane Definition

#### Basic Syntax

```plingua
@mu = [ multiset ]'label;
```

#### Nested Membranes

```plingua
@mu = [ multiset1 ]'outer
      [ multiset2 ]'inner1
      [ multiset3 ]'inner2;
```

Tree structure:
```
outer
├── inner1
└── inner2
```

#### Hierarchical Structure

```plingua
@mu = [ ]'environment
      [ ]'organism
        [ ]'cell
          [ ]'nucleus
          [ ]'mitochondria;
```

Tree structure:
```
environment
└── organism
    └── cell
        ├── nucleus
        └── mitochondria
```

### Membrane Charges

```plingua
[ ]'label       // Neutral (0)
[ ]'label^0     // Explicit neutral
[ ]'label^+     // Positive (+1)
[ ]'label^-     // Negative (-1)
```

Charges affect rule applicability.

### Initial Multisets

```plingua
// Set multiset for a label
@ms(label) = a{10}, b{5}, c;

// Add to multiset
@ms(label) += d{2}, e;

// Works with indexed labels
@ms(cell{i}) += x{i} : 1 <= i <= 10;
```

### Extended Membrane Structures

Extend existing structure:

```plingua
// Initial structure
@mu = [ ]'cell;

// Extend it
[ ]'cell [ ]'organelle;
```

---

## Rules

### Rule Syntax

```ebnf
rule ::= '[' lhs arrow rhs ']' label [ charge ] [ features ]
lhs  ::= parent_multiset membrane_pattern
rhs  ::= parent_multiset membrane_production
```

### Basic Evolution Rules

```plingua
// Object transformation
[a --> b]'cell

// Multiple objects
[a, b --> c, d]'cell

// With multiplicities
[a{2}, b --> c{3}]'cell

// Membrane contents
[a [b]'inner --> [c]'inner ]'outer
```

### Communication Rules

#### Send In

```plingua
// Send object from parent to membrane
a [ ]'cell --> [b]'cell
```

Objects in parent region enter the membrane.

#### Send Out

```plingua
// Send object from membrane to parent
[a]'cell --> b [ ]'cell
```

Objects in membrane exit to parent.

#### Bidirectional Communication

```plingua
// Exchange between two membranes
[a]'cell1 <--> [b]'cell2
```

### Dissolution Rules

```plingua
// Mark for dissolution
[a --> @d]'cell

// Dissolve and produce
[a]'cell --> b

// Conditional dissolution
[a]'cell --> @d
```

When a membrane dissolves:
1. Contents move to parent (or environment)
2. Children become siblings
3. Membrane is removed

### Division Rules

```plingua
// Divide into two identical membranes
[a]'cell --> [b]'cell [b]'cell

// Divide with different contents
[a]'cell --> [b{1}]'cell [b{2}]'cell

// Divide with inner membranes
[a [x]'inner]'outer --> [b [x]'inner]'outer [c [x]'inner]'outer
```

### Inner Membrane Rules

Affect nested membranes:

```plingua
// Modify inner membrane contents
[ [a]'inner --> [b]'inner ]'outer

// Consume from both levels
[x [a]'inner --> y [b]'inner ]'outer

// Multiple inner membranes
[ [a]'inner1 [b]'inner2 --> [c]'inner1 [d]'inner2 ]'outer
```

### Rule Features

#### Priority

```plingua
[a --> b]'cell : priority=10;
```

Higher priority rules execute first. Default is 0.

#### Probability

```plingua
[a --> b]'cell : probability=0.7;
```

Probabilistic rule application for stochastic P-systems.

#### Pattern Association

```plingua
[a --> b]'cell : pattern="evolution";
```

Associate rule with a named pattern for semantic control.

### Ranges in Rules

```plingua
// Parameterized rule generation
[a{i} --> b{i}]'cell : 1 <= i <= 10;

// Multiple range variables
[x{i}, y{j} --> z{i,j}]'cell : 
    1 <= i <= n, 
    1 <= j <= m;

// Conditional ranges
[edge{i,j} --> colored{i,j}]'graph : 
    i < j, 
    1 <= i <= n, 
    1 <= j <= n;
```

---

## Patterns and Models

### Pattern Definition

Patterns are reusable rule templates:

```plingua
!patternName {
    rule1;
    rule2;
    ...
}
```

**Example:**

```plingua
!evolution {
    [a --> b]'cell;
    [b --> c]'cell;
    [c --> a]'cell;
}

!communication {
    a [x]'cell --> [x, a]'cell;
    [y]'cell --> y [ ]'cell;
}
```

### Pattern Variables

Patterns can use wildcards:

```plingua
!generic_evolution {
    ?[a -> v]'h;      // Evolution (any result)
    ?[a ->  ]'h;      // Dissolution
}
```

Wildcards:
- `?[ ]` - Any membrane
- `?a` - Any object
- `'h` - Pattern variable for label

### Model Definition

Models specify rule application semantics:

```plingua
@model(modelName) = pattern1, pattern2, ...;
```

**Semantic operators:**
- Sequential: `,`
- Bounded parallel: `@n{...}` (at most n applications)
- Unbounded parallel: `@{...}` (any number of applications)

**Examples:**

```plingua
// Sequential application
@model(sequential) = pattern1, pattern2;

// Maximally parallel
@model(parallel) = @{pattern1, pattern2};

// Bounded parallel
@model(bounded) = @5{pattern1}, pattern2;

// Complex composition
@model(complex) = 
    @{evolution}, 
    @1{communication}, 
    @{division};
```

### Predefined Models

#### Transition Model

```plingua
@include "transition_model.pli"
@model<transition>
```

Standard maximally parallel model.

#### Division Model

```plingua
@include "membrane_division_model.pli"
@model<membrane_division>
```

Supports membrane division rules.

#### Tissue Model

```plingua
@include "tissue_model.pli"
@model<tissue>
```

For tissue P-systems with cell communication.

---

## Modules

### Module Definition

```plingua
def moduleName(param1, param2, ...) {
    // Module body
    return value;  // Optional
}
```

### Parameters

Parameters are passed by value:

```plingua
def calculate(x, y) {
    return x + y;
}
```

### Return Values

```plingua
def factorial(n) {
    if (n <= 1) {
        return 1;
    }
    return n * call factorial(n - 1);
}
```

### Calling Modules

```plingua
let result = call moduleName(arg1, arg2);
```

### Main Module

Entry point of the program:

```plingua
def main() {
    // Initialize P-system
    @mu = [ ]'cell;
    @ms(cell) += a{10};
    [a --> b]'cell;
}
```

Can accept command-line arguments:

```bash
plingua program.pli 42 "hello"
```

```plingua
def main(arg1, arg2) {
    // arg1 = 42, arg2 = "hello"
}
```

### System Modules

Built-in functions:

```plingua
system print(value);
system random(min, max);
```

---

## Directives

### @include

Include another P-Lingua file:

```plingua
@include "filename.pli"
@include "path/to/file.pli"
```

**Behavior:**
- File is parsed and included at directive location
- Relative paths searched first
- Include paths specified with `-I` flag
- Circular includes detected and prevented

### @mu

Define membrane structure:

```plingua
@mu = [ ]'label;
```

Only one `@mu` directive allowed per program.

### @ms

Define initial multiset:

```plingua
@ms(label) = objects;   // Set
@ms(label) += objects;  // Add
```

Multiple `@ms` directives for same label accumulate.

### @model

Define semantic model:

```plingua
@model(name) = semantics;
```

### @d

Dissolution marker (used in rules):

```plingua
[a --> @d]'cell
```

---

## Grammar Reference

### Complete EBNF Grammar

```ebnf
(* Top level *)
plingua ::= { definition }

definition ::= include | model_definition | module | pattern | statement

include ::= '@include' string

model_definition ::= '@model' '(' identifier ')' '=' model_body
                  | '@model' '<' identifier '>'

module ::= 'def' identifier '(' [ parameters ] ')' '{' { statement } '}'

parameters ::= identifier { ',' identifier }

pattern ::= '!' identifier '{' { rule } '}'

(* Statements *)
statement ::= variable_decl | assignment | if_stmt | while_stmt 
            | do_while_stmt | for_stmt | return_stmt | call_stmt
            | rule | membrane_structure | multiset_init | block

variable_decl ::= 'let' identifier '=' expression ';'

assignment ::= identifier assign_op expression ';'
assign_op ::= '=' | '+=' | '-=' | '*=' | '/=' | '%=' 
            | '&=' | '|=' | '^=' | '<<=' | '>>='

if_stmt ::= 'if' '(' expression ')' statement [ 'else' statement ]

while_stmt ::= 'while' '(' expression ')' statement

do_while_stmt ::= 'do' statement 'while' '(' expression ')' ';'

for_stmt ::= 'for' '(' statement expression ';' expression ')' statement

return_stmt ::= 'return' [ expression ] ';'

call_stmt ::= 'call' identifier '(' [ arguments ] ')' ';'

block ::= '{' { statement } '}'

(* Membrane structures *)
membrane_structure ::= '@mu' '=' membrane ';'

membrane ::= '[' multiset ']' label [ charge ] { membrane }

label ::= '\'' identifier { '.' identifier }

charge ::= '^' ( '+' | '-' | '0' )

(* Multisets *)
multiset_init ::= '@ms' '(' label ')' ( '=' | '+=' ) multiset ';'

multiset ::= [ multiobject { ',' multiobject } ]

multiobject ::= object [ '{' expression '}' ]

object ::= identifier [ '{' expression { ',' expression } '}' ]

(* Rules *)
rule ::= '[' lhs arrow rhs ']' label [ charge ] [ ranges ] [ features ] ';'

lhs ::= multiset [ membrane ]

rhs ::= multiset [ membrane_production ]

membrane_production ::= membrane { membrane }

arrow ::= '-->' | '<-->'

ranges ::= ':' range { ',' range }

range ::= expression ( '<' | '<=' | '>' | '>=' | '==' ) expression

features ::= ':' feature { ',' feature }

feature ::= identifier '=' expression

(* Expressions *)
expression ::= logical_or_expr [ '?' expression ':' expression ]

logical_or_expr ::= logical_and_expr { '||' logical_and_expr }

logical_and_expr ::= bitwise_or_expr { '&&' bitwise_or_expr }

bitwise_or_expr ::= bitwise_xor_expr { '|' bitwise_xor_expr }

bitwise_xor_expr ::= bitwise_and_expr { '^' bitwise_and_expr }

bitwise_and_expr ::= equality_expr { '&' equality_expr }

equality_expr ::= relational_expr { ( '==' | '!=' ) relational_expr }

relational_expr ::= shift_expr { ( '<' | '<=' | '>' | '>=' ) shift_expr }

shift_expr ::= additive_expr { ( '<<' | '>>' ) additive_expr }

additive_expr ::= multiplicative_expr { ( '+' | '-' ) multiplicative_expr }

multiplicative_expr ::= unary_expr { ( '*' | '/' | '%' ) unary_expr }

unary_expr ::= { ( '+' | '-' | '!' | '~' | '++' | '--' ) } postfix_expr

postfix_expr ::= primary_expr { ( '++' | '--' ) }

primary_expr ::= literal | identifier | call_expr | '(' expression ')'

call_expr ::= 'call' identifier '(' [ arguments ] ')'
            | 'system' identifier '(' [ arguments ] ')'

arguments ::= expression { ',' expression }

literal ::= integer | float | string | 'true' | 'false'
```

---

## Reserved Identifiers

### Keywords

```
def let call system return if else while do for
true false int_t long_t double_t string_t prob_t rexp_t
```

### Directives

```
@include @model @mu @ms @d
```

### Special Symbols

```
# (empty multiset)
? (pattern wildcard)
' (label prefix)
^ (charge indicator)
```

---

## Type System

### Type Inference

Variables are automatically typed:

```plingua
let x = 42;          // x : int_t
let y = 3.14;        // y : double_t
let s = "text";      // s : string_t
let b = true;        // b : int_t (boolean)
```

### Type Conversion

Implicit conversions:
- `int_t` → `long_t`
- `int_t` → `double_t`
- `long_t` → `double_t`

Explicit conversion not supported; use expressions:

```plingua
let x = 42;
let y = x * 1.0;  // Convert to double
```

### Type Compatibility

Rules for operators:
- Arithmetic: Both operands converted to widest type
- Comparison: Operands must be comparable
- Logical: Operands treated as boolean (0 = false, non-zero = true)
- Bitwise: Operands must be integral

---

## Scoping Rules

### Variable Scope

```plingua
let x = 1;           // Global scope

def foo() {
    let y = 2;       // Function scope
    {
        let z = 3;   // Block scope
    }
    // z not accessible here
}
// y not accessible here
```

### Shadowing

Inner scopes can shadow outer variables:

```plingua
let x = 1;

def foo() {
    let x = 2;       // Shadows global x
    {
        let x = 3;   // Shadows function-scope x
    }
}
```

### Membrane Labels

Labels are in a separate namespace:

```plingua
let cell = 10;       // Variable named 'cell'
@mu = [ ]'cell;      // Label named 'cell' (no conflict)
```

---

## Semantic Rules

### Rule Application

1. **Selection Phase**: Identify applicable rules
2. **Priority Filtering**: Consider only highest priority
3. **Semantics Checking**: Apply model constraints
4. **Application Calculation**: Determine number of applications
5. **Execution Phase**: Apply selected rules

### Maximal Parallelism

Unless otherwise specified, rules apply maximally in parallel:

```plingua
// If we have a{10}, both applications happen simultaneously
[a{5} --> b{5}]'cell  // Applies 2 times (consumes all a's)
```

### Conflict Resolution

When rules conflict:
1. Higher priority wins
2. Within same priority, all applicable rules execute
3. Random selection if specified

---

## File Organization

### Recommended Structure

```
project/
├── main.pli              # Entry point
├── models/
│   ├── base.pli          # Base definitions
│   └── extended.pli      # Extensions
├── patterns/
│   ├── evolution.pli     # Evolution patterns
│   └── communication.pli # Communication patterns
└── examples/
    └── test.pli          # Test cases
```

### Include Strategy

```plingua
// models/base.pli
!basic_rules {
    // definitions
}

// models/extended.pli
@include "models/base.pli"
!advanced_rules {
    // extends basic_rules
}

// main.pli
@include "models/extended.pli"
def main() {
    // use definitions
}
```

---

## References

For more information, see:
- [Implementation Guide](implementation_guide.md)
- [Architecture Overview](../architecture/overview.md)
- [Formal Specifications](../specifications/)
- [Code Examples](../examples/)
