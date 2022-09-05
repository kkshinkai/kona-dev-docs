# Lexical Structure

The lexical structure of Kona describes what sequence of characters form valid
tokens of the language. These valid tokens form the lowest-level building blocks
of the language and are used to describe the rest of the language in subsequent
chapters. A token consists of an identifier, operator, keyword, punctuation, or
literal. The behavior of the lexical analyzer follows the longest match (aka.
maximal munch) principle.

## WhiteSpace and Comments

Whitespace is used to separate tokens, it will be ignored by the lexer. The
following characters are considered whitespace: space (`U+0020`), carriage
return (`U+000D`), and line feed (`U+000A`).

```
whitespace        ::= whitespace-item whitespace?
whitespace-item   ::= end-of-line
                    | inline-space
                    | comment
end-of-line       ::= U+000A | U+000D | U+000D U+000A
inline-spaces     ::= inline-space inline-spaces?
inline-space      ::= U+0020
```

Some common whitespace characters are illegal in Kona, such as line feed
(`U+000A`), carriage return (`U+000D`), horizontal tab (`U+0009`), vertical tab
(`U+000B`), form feed (`U+000C`), null (`U+0000`), etc. Some whitespace
characters behave differently depending on the editor. For example, tab may take
up 2, 4 or 8 spaces, and some editors will also use it to align indentation. To
avoid these confusions, Kona only allows space (`U+0020`) as inline whitespace.

Kona is also strict about line breaks. There are three kinds of line breaks:
line feed (LF, `U+000A`), carriage return (CR, `U+000D`), and carriage return
followed by line feed (CRLF, `U+000D U+000A`). Single carriage return is not in
use in modern operating systems, Kona only allows the others. In addition, You
should use the line break that matches your operating system, LF for Unix-like
systems (Linux, FreeBSD, macOS, etc.), CRLF for Windows, otherwise the compiler
will complain. If you are using different operating systems, Git may help you
with line break conversion automatically.

> **Why?**: TODO

Comments are treated as whitespace by the compiler. Kona doesn't want to be line
break sensitive (nor indent sensitive). We are not able to support single line
comments. Just like OCaml, Kona only supports block comments.

```
comment            ::= '(*' comment-text '*)'
comment-text       ::= comment-text-item comment-text?
comment-text-item  ::= comment
                     | Any Unicode scalar value except '(*' or '*)'
```

Nesting multiline comments is allowed, but the comment markers must be balanced.

## Keywords and Punctuation

The following keywords are reserved and can’t be used as identifiers: `else`,
`fn`, `if`, `in`, `let`, and `then`.

```
keyword ::= and andalso as case datatype else end fn fun if in infixl infixr let nonfix
of op orelse rec then val
```

The following tokens are reserved as punctuation: (, ), ;, `` ` ``, =, and =>.

```
punctuation ::= ';' | '(' | ')' | '`' | '=' | '=>'
```

## Literals

### Integer Literals
