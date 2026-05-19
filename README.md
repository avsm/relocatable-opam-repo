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

## OxCaml one-shot compiler overlay

This repository also overlays a single package,
`oxcaml-compiler.5.2.0minus31`, intended to be layered *above*
[`oxcaml/opam-repository`](https://github.com/oxcaml/opam-repository)
at a higher priority. It is a drop-in replacement for the upstream
package of the same name: the dependency, `setenv` and `conflicts`
contract is byte-for-byte identical, but the hand-written multi-step
bootstrap (build OCaml 5.4.0, then dune, then menhir, then autoconf +
`configure --enable-middle-end=flambda2`, then `make`, then install)
is replaced by the single Makefile entry point of the self-contained,
pre-unpacked and pre-patched source bundle published by
[`oxcaml-pkgs`](https://oi.thicket.dev/repo/) at
`https://oi.thicket.dev/repo/src/oxcaml-5.2.0minus31-3416edee6.tar.gz`.

Everything else in the OxCaml base-compiler chain
(`ocaml-variants.5.2.0+ox`, `oxcaml.latest`/`oxcaml.archived`, and all
the `oxcaml-*-patches` guard packages) continues to come from
`oxcaml/opam-repository` unchanged, so nothing is duplicated. It still
compiles OxCaml from source — the win is a vastly simpler opam
definition, not a binary install (the prebuilt `.deb`/`.rpm`/pacman
packages from `oxcaml-pkgs` install system-wide under `/opt`, not into
an opam switch prefix). Restricted to Linux x86_64/arm64, since the
bundle drops the macOS/OpenBSD handling and OxCaml is glibc-only there.

Use it by adding this repo at a higher priority than the OxCaml one,
for example:

```sh
opam switch create oxcaml oxcaml-compiler \
  --repos relocatable=git+https://github.com/avsm/relocatable-opam-repo.git,ox=git+https://github.com/oxcaml/opam-repository.git,default
```

- Anil Madhavapeddy
