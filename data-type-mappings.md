<!--- CudaText: lexer_file=Markdown; tab_size=2; tab_spaces=Yes; newline=LF; --->

# V Types

## Primitive types

[V primitive types documentation](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types)

| V       | Free Pascal | Notes |
|---------|-------------|-------|
| bool    | boolean     |       |
| int     | int32       | [See V int primitive type](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types) |
| i8      | int8        |       |
| i16     | int16       |       |
| i32     | int32       |       |
| i64     | int64       |       |
| u8      | uint8       |       |
| u16     | uint16      |       |
| u32     | uint32      |       |
| u64     | uint64      |       |
| voidptr | pointer     |       |
| rune    | int32       |       |
| isize   |             | [See V isize primitive type](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types) |
| usize   |             | [See V usize primitive types](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types) |
| any     |             | Not in V yet? |

## String

`string` will use a V `c:backend` compatible [record/structure](https://github.com/vlang/v/blob/fc8e3d09717eaac3f2854640f91edfe55ede923f/vlib/builtin/string.v#L44).

When returned or needed to be memory managed, will be wrapped in a smart pointer type record.

Implementation details to be worked out on the smart pointer wrapping.
