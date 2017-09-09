# 01. Go Intro
This session introduces Go as a programming language for web system
development. The hands-on focuses on getting Go up and running on your machine
and teaches you some basics.

## Outline

- Why Go?
- Design Principles
- Features, Overview
- Installation
- Hello Go!


**You can find the slides [here.](http://go-talks.appspot.com/github.com/jf87/scalable_web_systems/session-01/slides/session01.slide)**


## Hands-on

### 1. Install and Setup Go

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


#### Get familiar with $GOPATH

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

##### Setting $GOPATH

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

##### Questions
1. What version of Go are you running?
2. What is your $GOPATH?
3. What is the purpose of $GOPATH?
4. Can you have more than one $GOPATH?

#### Go Tool

The `go` tool can do more than just compile your source code.
Lets survey some of the more important functions:

Use `go help [command]` to understand `build, doc, fmt, get, install, run,
test`.

##### Questions
1. What is the difference between `go build, install` and `run`?
2. Get the present tool from golang.org/x/tools/present using the go tool.
What happens when you do that? Can you do the same without using the go tool (if yes, how can you)?
3. Can you find where the source code is stored? Where is the binary? Can you run it?
How can you run the present tool from anywhere on the command line without specifying its absolute path?
4. How can you update the present tool?
5. How can you see the documentation of the present tool in the command line? What does the tool do?
6. Can you use the go tool to get the repository where this tutorial is hosted (https://github.com/jf87/go-web-workshop)?
Why not? How can you fix it?

#### gofmt

You just read about `go fmt`. There is also the `gofmt` command. Both are used
to format source code according to the Go source code style. Opposed to other
programming languages, there is no discussion about tabs vs spaces in Go.
Instead, basically all projects are following the formating provided by
`gofmt`.

##### Questions

1. What is the difference between `go fmt` and `gofmt`?
2. Run `gofmt` on a go file with the following content:
```go
package main
import "fmt"
import "time"
func main(){fmt.Println("Hello...")
time.Sleep(1000 * time.Millisecond)
fmt.Println("...and goodbye!")}
```
What happens?
How can you use `gofmt` to see the diff between old and new format? Write the
file using the correct formating using `gofmt`.
3. Does the new file use tabs or spaces?

### 2. Setup your Editor

You can use whatever editor you like to edit your .go source files. Most
editors work well together with the Go toolchain and, with some plugins,
provide autocompletion and error checking. There is usually no need to use a
full-blown IDE.
Here are some examples for editors that work well:

#### Visual Studio Code

An easy to use editor that works well with the go toolchain is [Visual Studio
Code](https://code.visualstudio.com/). Remember to install
[vscode-go](https://github.com/Microsoft/vscode-go), the go language extension.

#### Atom

[Atom](https://atom.io) can be enhanced with Go support by installing
[go-plus](https://github.com/joefitzgerald/go-plus).

#### Sublime Text
[Sublime Text](https://www.sublimetext.com) can be enhanced with Go support by
installing [GoSublime](https://github.com/DisposaBoy/GoSublime).

#### Vim/Neovim

Another option is to use [vim](http://www.vim.org/) or
[neovim](https://neovim.io) together with
[vim-go](https://github.com/fatih/vim-go).


**A more complete list of editors and IDEs can be found
[here](https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins).**

#### Questions
1. Choose an editor. Can you setup your editor so it runs `gofmt` when you save
   the current file?

### 3. Hello Go

Now it's time to get your hands a bit dirty and write your own first Go program.  
The [How to Write Go Code](https://golang.org/doc/code.html) tutorial by Google
is a great start for that.

In the tutorial, you will create an application, a simple library and Go unit
tests.

**Follow the whole tutorial.**

#### Questions
1. Can you explain what a package is?
2. What is "fmt"?
3. Rename the `Reverse` function into `reverse`. What happens? Why is that?

### 4. Go Tour

Follow the [official Go tour](https://tour.golang.org/welcome/1) in your
browser to get a more complete overview over the language.

Note: You can also grab the tour locally:

```
go get golang.org/x/tour/gotour
```

Then you can run `gotour` and visit http://127.0.0.1:3999/welcome/1

#### Questions
1. What is the difference between the types `int` and `int64`?
2. What is a pointer?
3. What is the difference between an array and a slice in Go? How are they stored in memory?
4. Can you have memory leaks in Go? How?

##### Go Playground
Note: The same application that powers the Go Tour is also available to as a service
to tryout or share code snippets: https://play.golang.org

### 5. Moving on

There are two slightly more advanced reads. If have not time for this during
class, you probably want to go over these until next week ;-)

1. Go over the [Go Programming Language Specification](https://golang.org/ref/spec)  
(You don't need to read all in detail).
2. Read [Effective Go](https://golang.org/doc/effective_go.html)

#### Questions
1. What is an interface?
2. How are errors handled in Go?
