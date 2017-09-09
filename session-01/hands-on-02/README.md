# 2. Setup your Editor

## gofmt

You just read about `go fmt`. There is also the `gofmt` command. Both are used
to format source code according to the Go source code style. Opposed to other
programming languages, there is no discussion about tabs vs spaces in Go.
Instead, basically all projects are following the formating provided by
`gofmt`.

### Questions

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

You can use whatever editor you like to edit your .go source files. Most
editors work well together with the Go toolchain and, with some plugins,
provide autocompletion and error checking. There is usually no need to use a
full-blown IDE.
Here are some examples for editors that work well:

### Visual Studio Code

An easy to use editor that works well with the go toolchain is [Visual Studio
Code](https://code.visualstudio.com/). Remember to install
[vscode-go](https://github.com/Microsoft/vscode-go), the go language extension.

### Atom

[Atom](https://atom.io) can be enhanced with Go support by installing
[go-plus](https://github.com/joefitzgerald/go-plus).

### Sublime Text
[Sublime Text](https://www.sublimetext.com) can be enhanced with Go support by
installing [GoSublime](https://github.com/DisposaBoy/GoSublime).

### Vim/Neovim

Another option is to use [vim](http://www.vim.org/) or
[neovim](https://neovim.io) together with
[vim-go](https://github.com/fatih/vim-go).


**A more complete list of editors and IDEs can be found
[here](https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins).**

### Questions
1. Choose an editor. Can you setup your editor so it runs `gofmt` when you save
   the current file?




