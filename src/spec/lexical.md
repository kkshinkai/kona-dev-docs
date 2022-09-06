# Lexical Structure

## Reserved Words

The following are the reserved words used in the Core. They may not (except `=`)
be used as identifiers

```sml
and as case datatype do else end exception fn fun if in infix
infixr let local nonfix of op open orelse rec then type val with
withtype while ( ) [ ] { } , : ; ... _ | = => -> #
```

## Special Constants

### Integer Constants

An integer constant (in decimal notation) is an optional negation symbol (`~`)
followed by a non-empty sequence of decimal digits `0`,..,`9`.

```sml
0   (* 0: int *)
42  (* -42: int *)
~1  (* 0: int *)
~   (* negative function: int -> int *)
~ 3 (* a negative function and an integer constant 3 *)
```

An integer constant (in hexadecimal notation) is an optional negation symbol
followed by `0x` followed by a non-empty sequence of hexadecimal digits
`0`,..,`9`, `a`,..,`f`. (`A`,..,`F` may be used as alternative for `a`,..,`f`.)

```sml
0x0      (* 0: int *)
0xabcdef (* 11259375: int *)
0xABCDEF (* 11259375: int *)
0xAbCdEf (* 11259375: int *)
~0x1     (* -1: int *)
0x       (* 0: int and a variable named x *)
~0x      (* -0: int and a variable named x *)
```

### Word Constants

A word constant (in decimal notation) is `0w` followed by a non-empty sequence
of decimal digits. A word constant (in hexadecimal notation) is `0wx` followed
by a non-empty sequence of hexadecimal digits.

```sml
0w0        (* 0: word *)
0w123      (* 123: word *)
0w         (* 0: int and a variable named w *)
~0w123     (* -0: int and a variable named w123 *)
```

### Real Constants

A real constant is an integer constant in decimal notation, possibly followed by
a point (`.`) and one or more decimal digits, possibly followed by an exponent
symbol (`E` or `e`) and an integer constant in decimal notation; at least one of
the optional parts must occur, hence no integer constant is a real constant.

```
0.7    (* 0.7: real *)
3.32E5 (* 332000.0: real *)
3E~7   (* 0.0000003: real *)
23     (* 23: int *)
.3     (* a dot `.` and an integer constant 3 *)
4.E5   (* 4: int, a dot `.` and a variable named E5 *)
1E2.0  (* a real constant 1E2, a dot `.` and an integer constant 0 *)
```

> Note: There are two options for dealing with illegal source like `.3`:
>
> 1. Lex it as a dot (`.`) and an integer constant `3`, the error will be
>     reported by the parser later (because `.` is only allowed in qualified
>     identifiers);
> 2. Lex it as an invalid real constant token `0.3` and report the illegal
>     literal error;
>
> We should choose the second option. It could provide better error messages.
