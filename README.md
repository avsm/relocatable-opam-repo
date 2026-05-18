# relocatable-opam repository

A small opam overlay providing the relocatable variants of `ocamlfind`,
`ocamlbuild` and the OCaml 5.4 compiler, extracted from the `relocatable`
branch of <https://github.com/dra27/opam-repository>.

It is intended to be layered on top of the upstream `ocaml/opam-repository`
(at a higher priority), which already provides the `relocatable.packages`
and `compiler-cloning` virtual packages, plus all the unchanged base
dependencies (`base-*`, `flexdll`, `ocaml-config`, `ocaml-option-*`, etc.).
Only packages that genuinely differ from upstream are included here, so
nothing is duplicated.

Most users should not be using this, but instead going for @dra27's 
one!

- Anil Madhavapeddy
