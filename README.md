# OCaml rules for Bazel

## Rules

* [ocaml_native_binary](#ocaml_native_binary)
* [ocaml_bytecode_binary](#ocaml_bytecode_binary)
* [ocaml_interface](#ocaml_interface)

## Overview

Build OCaml with Bazel. Very experimental. API is expected to change.

## Setup

This is a wrapper around `ocamlbuild`. Ensure that the OCaml toolchain (`ocamlbuild`, `ocamlfind`, etc) is reachable in your `PATH`. TODO: have `ocaml_repositories` download the toolchain.

Add the following to your `WORKSPACE` file.

```bzl
git_repository(
    name = "io_bazel_rules_ocaml",
    remote = "https://github.com/jin/rules_ocaml.git",
    commit = "18685e1bc5ad22e425f502355087747a6638f51a",
)

load("@io_bazel_rules_ocaml//ocaml:repo.bzl", "ocaml_repositories")
ocaml_repositories() 
# this downloads the OPAM precompiled binaries into your bazel cache, but doesn't use them directly yet.
```

and this to your BUILD files.

```bzl
load("@io_bazel_rules_ocaml//ocaml:ocaml.bzl", "ocaml_native_binary", "ocaml_bytecode_binary")
```

## Examples

### ocaml_native_binary

Generates a native binary.

```bzl
ocaml_native_binary(
    name = "hello_world",
    srcs = glob(["examples/*.ml"]),
    opam_packages = ["pkg_foo", "pkg_bar"],
    src_root = "examples/hello_world.ml", # Optional, defaults to the first main.ml found while loading the sources.
)
```

### ocaml_bytecode_binary

Generates a bytecode binary.

```bzl
ocaml_bytecode_binary(
    name = "hello_world",
    srcs = glob(["examples/*.ml"]),
    opam_packages = ["pkg_foo", "pkg_bar"],
    src_root = "examples/hello_world.ml", # Optional, defaults to the first main.ml found while loading the sources.
)
```

### ocaml_interface

Generates a `.mli` file of the source.

```bzl
ocaml_interface(
    name = "hello_world_interface",
    src = "examples/hello_world.ml",
)
```

## Projects using rules_ocaml

- [https://github.com/jin/scheme.ml](https://github.com/jin/scheme.ml)
