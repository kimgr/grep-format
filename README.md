# grep-format

Running [`clang-format`][1] over a big, mostly-well-formatted codebase is a
dramatic event. It can be hard to reason about the changes, because minor
inconsequential cleanups are mixed with bigger controversial reformats.

My approach so far has been to use [`git-format`][2] to format every code
change, so that the codebase gradually, very slowly, becomes cleaner as we make
changes.

But there's always going to be a few jarring patterns that break with the
defined style. What if we could `clang-format` more selectively to avoid the
big bang, but still do focused cleanups...

Why not combine the sloppy efficiency of `grep` with the immense exacting power
of `clang-format`?

    $ git grep -n "\s$" | grep-format

This will search for all lines in a repository with trailing whitespace, and
format these lines using `clang-format`. Files unsupported by `clang-format` are
ignored.

    $ git grep -En "if \(.+\)\s+return" | grep-format

Similarly, this finds all one-line guard clauses `if (...) return ...;` and
formats these specific lines.

In short, if you can form a regular expression to match a single-line anomaly in
your codebase, you can use `grep-format` to address it.

[1]: https://clang.llvm.org/docs/ClangFormat.html
[2]: https://github.com/kimgr/git-format


## Caveat ##

Given the fuzzy interplay between `grep` and `grep-format`, there's a
significant risk that the end result will be dirt soup.

Please make sure to run this tool on versioned code only, so you can revert
unwanted changes or reset failed experiments.
