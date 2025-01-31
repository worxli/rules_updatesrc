# Update Source File Rules for Bazel

[![Build](https://github.com/cgrindel/rules_updatesrc/actions/workflows/bazel.yml/badge.svg)](https://github.com/cgrindel/rules_updatesrc/actions/workflows/bazel.yml)

This repository contains [Bazel](https://bazel.build/) rules and macros that copy files from the
Bazel output directories to the workspace directory.

Have you ever wanted to copy the output of a Bazel build step to your source directory (e.g.,
generated documentation, formatted source files)? Instead of recreating the logic to perform this
trick in every Bazel project, try using the
[updatesrc_update](/doc/rules_and_macros_overview.md#updatesrc_update) rule and the
[updatesrc_update_all](/doc/rules_and_macros_overview.md#updatesrc_update_all) macro.

## Quickstart

The following provides a quick introduction on how to use the rules in this repository. Also, check
out [the documentation](/doc/) and [the examples](/examples/) for more information.

### 1. Configure your workspace to use `rules_updatesrc`

Add the following to your `WORKSPACE` file to add this repository and its dependencies.

```python

http_archive(
    name = "cgrindel_rules_updatesrc",
    sha256 = "18eb6620ac4684c2bc722b8fe447dfaba76f73d73e2dfcaf837f542379ed9bc3",
    strip_prefix = "rules_updatesrc-0.1.0",
    urls = ["https://github.com/cgrindel/rules_updatesrc/archive/v0.1.0.tar.gz"],
)

load(
    "@cgrindel_rules_updatesrc//updatesrc:deps.bzl",
    "updatesrc_rules_dependencies",
)

updatesrc_rules_dependencies()
```

### 2. Update the `BUILD.bazel` at the root of your workspace

At the root of your workspace, create a `BUILD.bazel` file, if you don't have one. Add the
following:

```python
load(
    "@cgrindel_rules_updatesrc//updatesrc:updatesrc.bzl",
    "updatesrc_update_all",
)

# Define a runnable target to execute all of the updatesrc_update targets
# that are defined in your workspace.
updatesrc_update_all(
    name = "update_all",
)
```

### 3. Add `updatesrc_update` to every Bazel package that has files to copy

In every Bazel package that contains source files that should be updated from build output, add a
[updatesrc_update](/doc/rules_and_macros_overview.md#updatesrc_update) declaration. For more
information on how to configure the declaration check out the [documentation](/doc) and the
[examples](/examples).

```python
load(
    "@cgrindel_rules_updatesrc//updatesrc:updatesrc.bzl",
    "updatesrc_update",
)

updatesrc_update(
    name = "update",
    # ...
)
```

### 4. Execute the Update All Target

To execute all of the [updatesrc_update](/doc/rules_and_macros_overview.md#updatesrc_update)
targets, run the update all target at the root of your workspace.

```sh
$ bazel run //:update_all
```

## Learn More

- [How to Use `rules_updatesrc`](/doc/how_to.md)
- Check out [the examples](/examples)
- Peruse [the documentation](/doc)
