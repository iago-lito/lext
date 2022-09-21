# Lext

`Lext` is a small, general-purpose lexing framework
to ease parsing of simple DSLs not from formal grammars
but from python code.
In particular, lexers and parsers derived from `Lext`
can be organized into very modular `Readers`,
and they can be *dynamic*
in the sense that information read in the parsed file
can modify the parser behaviour before it reaches the end of this file.

### Status

There is no full-fleshed, centralized documentation yet,
as the project is still a brutal extraction from [Clibate] internals.

### Lexing

The core utilities of `Lext` are found in `BaseLexer` (or `Lexer`) methods
and feature reading basic tokens from a string input:
  - fixed strings
  - regex patterns
  - proper python strings like
  (`'simple'`, `"double"`, `"esc\"aped"`, `'''triple'''`, `r"raw"`, *etc.*)

### Context

`BaseLexer` is wrapped into a `Lexer` (or `ContextLexer`) type,
which features the same API but also keeps track
of positional information within the parsed file:
line number, column number, filename,
and a possible chain of file inclusions.

This enables production of useful parsing error messages.

### Towards parsing

The `Parser` works with a set of user-implemented `Reader`s.
Every reader is responsible for matching the upcoming text stream
and parse it into a user-defined object.

In a nutshell, the parsers transforms a string
into a sequence of user-defined, parsed objects,
each produced by the associated reader.

A contextualized parsing error is raised
whenever more than one reader match the same input (ambiguity),
or if no readers match (unexpected input).

### Hard readers.

Basic, "hard" readers are responsible to recognize both the start and end
of the text stream they need to consume in order to produce their parsed value.
To this end, they use a safe view into the underlying `(Context)Lexer`.

### Ignored input.

Readers producing `None` can be used to ignore whitespace and comments.

### Soft readers.

Readers producing `SplitAutomaton` values, or "soft" readers
are treated specially by the parser,
as they do not need to recognize the end of their parsed region.
Instead, the user-defined `SplitAutomaton`
splits the upcoming text stream bit by bit,
with a user-specified definition what "bits" are (*eg.* "lines" in [Clibate]),
and progressively construct the parsed value from successive bits
until another reader matches the input,
which effectively terminates the currently parsed region.

### Dynamic parsing.

Readers producing `ParseEditor` values
do not emit values in the eventual lexed stream.
Instead, they are given the opportunity to use the parser API
to modify its behaviour, add/remove readers *etc.*

[Clibate]: https://gitlab.com/iago-lito/clibate
