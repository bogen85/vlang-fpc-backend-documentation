<!--- CudaText: lexer_file=Markdown; tab_size=2; tab_spaces=Yes; newline=LF; --->

# V Types

## Primitive types

[V primitive types documentation](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types)

`sizeof(type)` for both `V` and `Free Pascal` need to be identical in order to be a compatible mapping.

The following are identically sized between `V` and `Free Pascal`:

| V       | Free Pascal | Notes   |
|---------|-------------|---------|
| bool    | boolean     |         |
| f32     | single      |         |
| f64     | double      |         |
| int     | int32       | [See V int primitive type](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types) |
| i8      | int8        |         |
| i16     | int16       |         |
| i32     | int32       |         |
| i64     | int64       |         |
| u8      | uint8       |         |
| u16     | uint16      |         |
| u32     | uint32      |         |
| u64     | uint64      |         |
| i128    | TBD         | Not in V yet? (See 128 bit notes) |
| u128    | TBD         | Not in V yet? (See 128 bit notes) |
| voidptr | pointer     |         |
| rune    | int32       |         |
| isize   | (a signed integer)    | [See V isize primitive type](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types) |
| usize   | (an unsigned integer) | [See V usize primitive types](https://github.com/vlang/v/blob/master/doc/docs.md#primitive-types) |
| any     |             | Not in V yet? |

## 128 bit notes -- (Free Pascal 128 bit integers )

Compiler seems to know about [128 bit types.](https://gitlab.com/freepascal.org/fpc/source/-/blob/3a34fc7be3402cb52a436935f31c3c4ccb5a2d86/compiler/psystem.pas#L288)

See some custom usage [here.](https://gitlab.com/freepascal.org/fpc/source/-/blob/3a34fc7be3402cb52a436935f31c3c4ccb5a2d86/packages/hash/src/crc.pas#L71)

See [int128rec.](https://www.freepascal.org/docs-html/rtl/sysutils/int128rec.html)

See [this](https://forum.lazarus.freepascal.org/index.php?topic=45749.0) also.

## String

`string` will use a V `c:backend` compatible [record/structure](https://github.com/vlang/v/blob/fc8e3d09717eaac3f2854640f91edfe55ede923f/vlib/builtin/string.v#L44).

When returned or needed to be memory managed, will be wrapped in a smart pointer type record.

Implementation details to be worked out on the smart pointer wrapping.
