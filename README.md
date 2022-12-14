<!---
  CudaText: lexer_file=Markdown; tab_size=2; tab_spaces=Yes; newline=LF;
  --->

# vlang-fpc-backend-documentation

[Documentation for VLang FPC (Free Pascal Compiler) backend project](https://github.com/bogen85/vlang-fpc-backend-documentation)

https://www.freepascal.org/

##
1. [VLang fork for pull requests (private)](https://github.com/bogen85/vlang-fpc-backend)
1. [VLang Free Pascal backend development branch (private)](https://github.com/bogen85/vlang-fpc-backend/tree/das-develop-main)

## Goals
1. Able to function at same tier level as the primary C backend.
1. Full interop with C (obviously)
1. Full interop with Free Pascal (and Lazarus) units. (with some limitations that may require bindings to work around, automatically generated)
1. Ability to use a V module (that is using this backend) as a FreePascal unit. (with some limitations that may require bindings to work around, automatically generated)
1. Support for all FPC targets and architectures.
1. No memory allocation leaks.

## Note on memory management

- The V frontend assumes automatic memory management, but currently not provide support for it.
- Currently in V the backend must provide that support.

## Memory management
1. Will use smart pointers (runtime reference counting, automatic free when count reaches 0)
1. Smart pointers will provide an API for the backend to increment/decrement count as needed (when there are handoffs to C/other)

## Anonymous functions
1. Since V Closures must declare captures, FPC closures won't need to be used.
2. Closures will managed by custom Object Pascal classes (generated per closure depending on capture and function arguments and return)
  - Captures variables with be data members
  - Function itself will be an object method
3. Managed by smart pointers, so backend API is applicable
4. FPC anonymous functions (and function references) may be used in other contexts in this backend, but not specifially to support V Closures.

## Callback functions for export to other languages
1. Similar to Closure objects

## Trampolines (for callbacks that cross language and other incompatible memory management boundries)
1. A trampoline template will be needed to create directly callable by C function pointers
  - Trampoline will need to be hot patchable to refer back to the to anonymous function or callback function objects as required.

## Case sensitivity vs insensitivity

Obviously to function as a VLang backend, since V is a case sensitive language, this backend will need to fully support that. Both for V code and interop with other languages.

Pascal however, specifically the Free Pascal Compiler in this case, is not case sensitive.

Free Pascal already has symbol attributes to deal with case sensitive imports/exports, so that will be used.

Unless symbol collisions become a major issue, since the V front end will be enforcing case sensitive matching, I plan to not apply any sort of suffix hint unless there is an acutal collision.

Handling of collsions could be maintained as follows:
1. Maintain a list of collisons, which scope, etc, and just create a serial tag suffix for the particular collision.
2. Create a modified (base36?) run length encoding suffix to identify which symbol characters are upper case, or if the entire symbol is uppercase, or if it is title case only. (Will only do this if symbol collisions becomes enough of an issue that a simple serial tag is insufficient to accomadate)
