# Abstract syntax

This file contains a simplified *abstract* syntax tree for Nix, meaning that
this is the representation of Nix's syntax that matters for specifying Nix's
semantics.

Some differences from the [concrete syntax](./concrete-syntax.md) are:

- This doesn't include trivial variations on various syntactic forms

  For example, the concrete syntax permits both `x@{ y }: e` and `{ y }@x` but
  I only include one of the two variants here since they are semantically
  identical.

- This simplifies the set of operators to the core set of operators

  For example, this only includes the `<` comparison operator and not other
  comparison operators.  I could include them but they'd just clutter up the
  semantics and it doesn't really add anything to explicitly mention them.

- Representative built-ins are explicitly mentioned

  For example, `builtins`, `null`, `true`, `false`, and `import` are not
  explicitly mentioned by the concrete syntax (they are all parsed as
  identifiers) but they are extremely relevant to the abstract syntax and
  semantics.

  All built-in functions and values are available under the `builtins` record,
  but Nix permits several
  [unqualified built-ins](https://nix.dev/manual/nix/2.34/language/builtins.html)
  and I added several representative ones to the abstract syntax tree.

<!--
> Note: The reason I don't use Github-flavored markdown with a `math` code
> block is that GitHub's MathJax renderer does not render `\overline{…}`
> correctly.
-->

![Nix abstract syntax tree](./abstract-syntax.svg)
