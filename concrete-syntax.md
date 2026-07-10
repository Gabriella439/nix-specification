# Concrete syntax

I created this by hand by consulting
[`lexer.l`](https://github.com/NixOS/nix/blob/master/src/libexpr/lexer.l) and
[`parser.y`](https://github.com/NixOS/nix/blob/master/src/libexpr/parser.y) from
the official Nix implementation.  The Nix grammar is obviously evolving slowly
so if you see anything out-of-date (or just plain wrong) please open an issue to
let me know or open a pull request to fix it.

This is not intended to be machine-readable and leaves out some details, like:

- comments/whitespace parsing
- precedence for operators

It also doesn't *exactly* match how the lexer and grammar are defined in the Nix
implementation.  I took some liberties to simplify things when I believed it was
safe to do so, although I could have made mistakes along the way.  I prioritized
clarity where possible because I use this document for my own personal working
notes.

```ebnf
expression =
    argument ":" expression
  | "assert" expression ";" expression
  | "with" expression ";" expression
  | "let" let-binding* "in" expression
  | "if" expression "then" expression "else" expression
  | pipe_expression

argument =
    identifier
  | formals
  | identifier "@" formals
  | formals "@" identifier

formals =
    "{" (("...")? | formal (", " formal)* ("," "...")?) "}"

formal = identifier ("?" expression)?

let-binding =
    identifier ("." attribute)* "=" expression ";"
  | inherit-binding

inherit-binding = "inherit" ("(" expression ")")? identifier* ";"

pipe_expression =
    operator_expression ("|>" operator_expression)+
  | (operator_expression "<|")+ operator_expression
  | operator_expression

operator_expression =
    operator_expression "?" attribute_path
  | unary_operator operator_expression
  | operator_expression binary_operator operator_expression
  | application_expression

unary_operator = "!" | "-"

binary_operator =
    "=="
  | "!="
  | "<"
  | "<="
  | ">"
  | ">="
  | "&&"
  | "||"
  | "+"
  | "-"
  | "*"
  | "/"
  | "->"
  | "//"
  | "++"

application_expression = select_expression+

select_expression =
    simple_expression ("." attribute_path ("or" select_expression)? | "or")?

attribute-binding =
    attribute_path "=" expression ";"
  | inherit-binding

simple_expression =
    variable
  | integer
  | float
  | string
  | indented_string
  | path
  | search_path
  | uri
  | ("let" | "rec")? "{" attribute-binding* "}"
  | "[" select_expression "]"
  | "(" expression ")"

attribute_path = attribute ("." attribute)*

attribute = identifier | string | interpolation | "or"

interpolation = "${" expression "}"

variable = identifier

integer = ? /[0-9]+/ ?

float = ? /(([1-9][0-9]*\.[0-9]*)|(0?\.[0-9]+))([Ee][+-]?[0-9]+)?/ ?

identifier = ? /[a-zA-Z_][a-zA-Z0-9_'\-]*/ ?

path = ? /([a-zA-Z0-9_+.-]*|~)(\/([a-zA-Z0-9_+.-]+|${expression})+)+\/?/ ?

search_path = ? /<[a-zA-Z0-9_+.-]+(\/[a-zA-Z0-9_+.-]+)*>/ ?

uri = ? /[a-zA-Z][a-zA-Z0-9+.-]*:[a-zA-Z0-9%\/?:@&=+$,_.!~*'-]+/ ?

indented_string =
  ? /''([^$']|${expression}|$[^{']|$'[^']|'[^'$]|''['$]|'$[^{']|'$'[^'])*($)?''/ ?

string = ? /"([^$"\\]|${expression}|$[^{"\\]|$\\(.|\n)|\\(.|\n))*($)?"/ ?
```
