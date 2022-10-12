# vlang-fpc-backend-documentation
Documentation for VLang FPC backend project

https://www.freepascal.org/

## Goals
1. Able to function at same tier level as the primary C backend.
1. Full interop with C (obviously)
1. Full interop with FPC units.

## Memory management
1. Will use smart pointers (runtime reference counting, automatic free when count reaches 0)
1. Smart pointers will provide an API for the backend to increment/decrement count as needed (when there are handoffs to C/other)

## Anonymous functions
1. Since V Closures must declare captures, FPC closures won't need to be used.
1. Closures will managed by custom Object Pascal classes (generated per closure depending on capture and function arguments and return)
  - Captures variables with be data members
  - Function itself will be an object method
1. Managed by smart pointers, so backend API is applicable

## Callback functions
1. Similar to Closure objects

## Trampolines (for callbacks that cross language and onther incompatible memory management boundries)
1. A trampoline template will be needed to create directly callable by C function pointers
  - Trampoline will need to be hot patchable to refer back to the to anonymous function or callback function objects as required.
