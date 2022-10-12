# Getting started

Currently backends are added by first changing pref.v as you noticed

Then you will have to change `vlib/v/builder/compile.v`

If you want to modify what files are going to be used (for example the `-b native` backend currently does not parse the files in `builtin` at all, since it does not support the full V language, same with the `-b` go one)

Then make a folder named for example `vlib/v/builder/fpcbuilder/` and copy there `vlib/v/builder/golangbuilder/golangbuilder.v` and modify it.

Finally, copy `cmd/tools/builders/golang_builder.v` to `cmd/tools/builders/fpc_builder.v`

And modify that (it is the entry point of the executable, that V will launch to do the actual compilation)

For example, the go one currently has:
```v
module main

import v.builder.golangbuilder

fn main() {
    golangbuilder.start()
}
```

The current way that backends work is that V (the frontend) checks if there is an executable named after the backend in `cmd/tools/builders/NAME_builder` and if there is not, it tries to compile `cmd/tools/builders/NAME_builder.v`

If that works, then now there should be an executable in that folder, NEWER than the V frontend executable itself
once it is compiled, it would not be recompiled, but used directly.

i.e. the iteration once it is setup, is to either remove the executable, or to do: `v self`, followed by `v -b fpc file.v`, which in turn will try to recompile the backend executable, and then use it to compile `file.v`.


i.e. the overhead is +1-2 seconds for the additional `v self` + the time needed to recompile the backend.

After the backend executable is present, the V frontend, will pass it all the arguments received by the user as if the user has typed say: `cmd/tools/builders/fpc_builder -b fpc file.v`

It is the backend's job to parse this again (that is why the `start()` function in `cmd/tools/builders/golang_builder.v` for example does:

```v
module golangbuilder

import os
import v.pref
import v.util
import v.builder
import v.gen.golang

pub fn start() {
    mut args_and_flags := util.join_env_vflags_and_os_args()[1..]
    prefs, _ := pref.parse_args([], args_and_flags)
    builder.compile('build', prefs, compile_golang)
}
```

But this time, in the child executable, it does `builder.compile('build', prefs, compile_golang)` at the end of the `start()` function (`compile_golang` is a function in the same module)

That callback is going to be called with a `(mut b builder.Builder)` parameter.

In which `b.pref.path` is `file.v` from the example invocations above.

(you can also access the rest of the parameters in `b.pref`)

Parsing and checking is done with: `b.front_and_middle_stages(files)`, where files is `[]string` of all the files that you want processed (as I said, different backends can choose to process different files, depending on how advanced they are, usually just passing `[b.pref.path]` is a good start, until you have enough to handle all of builtin, you can hardcode stuff like `println/1` and/or `exit/1`, to have some feedback)

After that passes, you will have an array of processed `ast.File` in `b.parsed_files`.

You can use something like `vlib/v/ast/walker/` (`import v.ast.walker`) like this:
```v
for file in b.parsed_files {
    walker.inspect(file, unsafe { &mut b }, fn (node &ast.Node, data voidptr) bool {
    }
}
```
To process all of the nodes in the AST of the files (edited) or walk over them manually, using matches, if you prefer more control.

i.e. the `TLDR` is, that I think that you can just modify `cmd/tools/builders/NAME_builder.v`, but the existing convention is to put the actual driver/setup implementation that handles the command line options in `vlib/v/builder/NAMEbuilder/` (reusing `v.pref`), and the actual code that produces the `text/binary` output in `vlib/v/gen/NAME/`
