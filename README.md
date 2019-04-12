`bs-uchar`
----------
This is the Uchar compatibility library from OCaml 4.03, backported to
OCaml 4.02, and thus [BuckleScript][] (an OCaml-to-JavaScript compiler)
versions prior to 6.0.0 (which were forked from OCaml 4.02.3) and
[Reason][] (an alternative OCaml syntax targeting that compiler.)

You can safely ignore the installation instructions below when compiling
to JS. Instead:

1. Choose the correct version of `bs-uchar` for your version of
   BuckleScript: ([See below](#versioning--libraries) for details)

   | `bs-platform@...` | `bs-uchar@...` | Install with ...
   |              ---: | :---           | :---
   |    `4.x \|\| 5.x` | `<1.0.0`       | `npm i bs-uchar`
   |         `>=6.0.0` | `>=1.0.0`      | `npm i bs-uchar@next`

2. Install this fork through npm, as described in the above table.

3. Manually add `bs-uchar` to your `bsconfig.json`'s
   `bs-dependencies`:

        "bs-dependencies": [
          ...
          "bs-uchar"
        ],

4. Use `Uchar.t` in your program!

   [BuckleScript]: <https://bucklescript.github.io/>
   [Reason]: <https://reasonml.github.io/>

## Versioning & libraries

Unlike the original version of this package for ocamlbuild / Dune, I
[can't find a way][npm-support] to ensure the correct version of this
package is automatically installed in downstream BuckleScript
consumers of your code. If you're writing a BuckleScript library, this
means:

1. If your code target BuckleScript 4 & 5 (which it presumably does,
   otherwise you wouldn't need this shim!), and include
   `bs-uchar@0.0.x` in your `dependencies`, *users of your library
   won't be able to compile for BuckleScript 6* ...

2. ... and vice-versa: if you are targeting BuckleScript 6 (why are
   you using this shim?) instead, and you depend on `bs-uchar@1.0.x`,
   *users of your library won't be able to compile for BuckleScript 4
   or 5.*

This sucks a lot, and makes *actually using* any OCaml-side libraries
(many, even most, of which depend on `Uchar.t`) a nightmare. Until
someone comes up with a better solution, this means that **I suggest
you not include `bs-uchar` in your `dependencies` for libraries** —
only do so for applications.

One alternative is to include `bs-uchar` in `peerDependencies`
instead — this comes with its own costs, and isn't something I
necessarily suggest. Finally, you could follow the usual JavaScript
practice of leaving shimming to the end-consumer — the last project in
the dependency-chain; although I would suggest mentioning `bs-uchar`
in your README for users of BuckleScript versions before 6.0.0.

#### Original README follows:

Uchar — Compatibility library for OCaml's Uchar module
-------------------------------------------------------------------------------
Release %%VERSION%%

The `uchar` package provides a compatibility library for the
[`Uchar`][1] module introduced in OCaml 4.03.

The `uchar` package is distributed under the license of the OCaml
compiler. See [LICENSE](LICENSE) for details.

[1]: http://caml.inria.fr/pub/docs/manual-ocaml/libref/Uchar.html

Home page: https://ocaml.github.io/uchar/
Contact: Daniel Bünzli `<daniel.buenzl i@erratique.ch>`


# Installation

Uchar can be installed with `opam`:

    opam install uchar

If you don't use `opam` consult the [`opam`](opam) file for build
instructions.


# Documentation

Since OCaml 4.03 you can find the module in the documentation
of the [standard library][1]. Before this you can consult
[this page](https://ocaml.github.io/uchar/Uchar.html).


# Testing

The file `test/testpkg.ml` can be used to test the compilation of the
package. Once the package is installed the following invocation
should succeed both before and after 4.03

    ocamlbuild -X src -use-ocamlfind -pkg uchar test/testpkg.native
