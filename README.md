# Nix specification

This repository contains my work-in-progress notes on Nix written as a
specification for the language.  I'm not actually interested in creating a
standard for the language *per se* but I *am* working on a type checker and
language server for Nix and I created and published these documents for a few
reasons:

- they're really useful cheatsheets

  These started as private personal notes in Obsidian that I was using to
  organize my own thoughts and others asked me to publish these notes when they
  found out about them.

- I want to be able to reference these notes in discussions with others

  For example, I might want to link to these notes in a blog post or share them
  with someone who is curious about some corner case of the language.

- specification work helps my brain

  My experience [authoring Dhall](https://dhall-lang.org/) and in particular the
  [Dhall standard semantics](https://github.com/dhall-lang/dhall-lang/tree/master/standard#semantics)
  taught me that it's extremely worthwhile to specify things by hand first
  instead of jumping straight into coding.  There were all sorts of corner cases
  and issues that were caught and prevented by Dhall's specification-first
  approach to language development.

Right now this repository only contains syntax-related specification documents:

- [concrete-syntax.md](./concrete-syntax.md) - the concrete syntax tree

- [abstract-syntax.md](./abstract-syntax.md) - the abstract syntax tree

… and the next document I'm working on is a specification of a typechecking
semantics for Nix.
