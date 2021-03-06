# malc TODO

Functions missing from the Mal core:

- conj
- exceptions / throw / try-catch
- meta / with-meta
- read-string (that's a big one)
- eval (requires JIT compiling?)

Fetaures missing from the runtime:

- proper string escaping of `" \ \n`
- exceptions

Compiler features:

- separate macro namespace (currently we expand macros in malc's namespace,
  which is ugly)
- better error detection during compilation (for example, calling `+` with
  non-integer arguments, or wrong number of arguments)
- add debugging symbols (-g)
- hide malc's internal functions (those defined in nativefuncs.mal) so they
  won't be visible from the user's Mal program (but will be visible to
  `core-impl.mal`)

Performance:

- don't compile the "standard library" every time.  Compile `nativefuncs.mal`
  into an object file or archive once, and just link it to any Mal program that
  is compiled.
- hash tables are linear lists with O(n) lookup/add/remove complexity.

Tests:

- automatically convert test suites from the Mal project to executable testable
  programs compiled by malc
