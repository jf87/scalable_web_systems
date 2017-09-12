# 1. Install and Setup Go

The easiest way to install Go is to download binaries provided by Google. You
can find architecture specific binaries and instructions on
https://golang.org/doc/install.

On Mac OS, I prefer to use homebrew to install Go: `brew install go`. Note,
that the Go versions that you can install through Linux Package Management
systems are usually outdated (so don't use apt-get or similar to install Go).

You can verify your installation by running `go version` in your terminal:

```
 go version go1.8.3 darwin/amd64
```

**Make sure to install a recent Go version > 1.8!**


## Get familiar with $GOPATH

This environment variable defines the workspace directory that contains all the
Go code on your machine. In other words, this directory contains code for
different projects and their dependencies.

There are three directories in it:

    $GOPATH/src    for all source files, including dependencies.
    $GOPATH/bin    for binaries.
    $GOPATH/pkg    cache for compiled packages (suffix .a)

The `go` tool builds source packages and installs binaries to `\pkg` and
`\bin`. `\src` contains usually multiple version controlled (Git, Mercurial...)
repositories of you and 3rd=party packages (e.g., dependencies for your
projects).

As a developer, you will spend most of your time in $GOPATH/src. The idea is
that your folder structure is aligned with the URL structure of the source-code
repository.
For example, if your code lives on a github.com, you might have:
``$GOPATH/src/github.com/USERNAME/REPONAME``.
Note: This does not mean that you need to put your source code under version
control from the start, but should still follow this structure.

### Setting $GOPATH

From Go 1.8 on, if not set, that path is automatically assumed to be:

```
$HOME/go            on Unix-like systems
%USERPROFILE%\go    on Windows
```

This means that as long as you work in that folder, you would not need to set
it manually. However, in my opinion it is better to set your `$GOPATH`
explicitly. On unix-like systems:

```
export GOPATH=${HOME}/go
```

To check your current `$GOPATH`:

```
go env GOPATH
```

### Questions
1. What version of Go are you running?
2. What is your $GOPATH?
3. What is the purpose of $GOPATH?
4. Can you have more than one $GOPATH?

## Go Tool

The `go` tool can do more than just compile your source code.
Lets survey some of the more important functions:

Use `go help [command]` to understand `build, doc, fmt, get, install, run,
test`.

### Questions
1. What is the difference between `go build, install` and `run`?
2. Get the present tool from golang.org/x/tools/present using the go tool.
What happens when you do that? Can you do the same without using the go tool (if yes, how can you)?
3. Can you find where the source code is stored? Where is the binary? Can you run it?
How can you run the present tool from anywhere on the command line without specifying its absolute path?
4. How can you update the present tool?
5. How can you see the documentation of the present tool in the command line? What does the tool do?
6. Can you use the go tool to get the repository where this tutorial is hosted (https://github.com/jf87/go-web-workshop)?
Why not? How can you fix it?

Move on to the next [section](../hands-on-02).
