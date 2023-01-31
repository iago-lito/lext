This is a rewrite of Lext,
a flexible, programmable, regex-based and grammar-free lexer
featuring "dynamism"
(lexing behaviour can change depending on the input already consumed)
and contextualized errors reporting.

The principles behind Lext are language-agnostic,
so I expect Lext to be eventually declined into various languages
(Python, Julia, Rust..).
This prototype runs with Python.
As such, it should offer at least lexing facilities
for standard tokenizing/parsing of Python literals.
Especially when it comes to escaped strings.
