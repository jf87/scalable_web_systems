# Hands-On

The concurrency model in Go is based on Communicating Sequential Processes
(CSP). The basic idea is that values are based between independent activities.
In Go these activities are called goroutines and we can pass values between
them by the means of channels.

## Goroutines

You can think of goroutines as lightweight threads.
To start a new goroutine, you simply put `go` in front of a function call.

```
f()    // call f(); wait for it to return
go f() // create a new goroutine that calls f(); don't wait
```


The following is a TCP server that is listening on port 8000 and prints the
current time to connected clients.

```go
package main

import (
	"io"
	"log"
	"net"
	"time"
)

func main() {
	listener, err := net.Listen("tcp", "localhost:8000")
	if err != nil {
		log.Fatal(err)
	}
	for {
		conn, err := listener.Accept()
		if err != nil {
			log.Print(err) // e.g., connection aborted
			continue
		}
		handleConn(conn) // handle one connection at a time
	}
}

func handleConn(c net.Conn) {
	defer c.Close()
	for {
		_, err := io.WriteString(c, time.Now().Format("15:04:05\n"))
		if err != nil {
			return // e.g., client disconnected
		}
		time.Sleep(1 * time.Second)
	}
}

```

### Questions
1. Build the application.
2. Use netcat or another utility to connect to the server. What happens if you connect with two clients simulation sly?
3. Can you enable the server to serve multi clients?


## Channels


```go
c := make(chan int) // c has type 'chan int'

c <- x // a send statement

x=<-c  // a receive expression in an assignment statement
<-c    // a receive statement; result is discarded
```


```go
package main

import "fmt"

func main() {
	ch := make(chan string)

	go func() {
		ch <- "Hola!"

	}()
	s := <-ch
	fmt.Println(s)
}
```

### Unbuffered and Buffered Channels

```go
ch = make(chan int)    // unbuffered channel
ch = make(chan int, 0) // unbuffered channel
ch = make(chan int, 3) // buffered channel with capacity 3
```
- A send operation on an unbuffered channel blocks the sending goroutine until
  another goroutine executes a corresponding receive on the same channel, at
  which point the value is transmitted and both goroutines may continue.
- Conversely, if the receive operation was attempted first, the receiving
  goroutine is blocked until another goroutine performs a send on the same
  channel.

#### Question
1. Which type of channel should you use to synchronize two goroutines and why?


<!--### Unidirectional Channels-->

<!--- The type chan<- int, a send-only channel of int, allows sends but not receives.-->
<!--- The type <-chan int, a receive-only channel of int, allows receives but not sends.-->

<!--=> checked at compile time.-->



<!--## Goroutine leak-->

## Select

```go
select {
case <-ch1:
    // ...
case x := <-ch2:
    // ...use x...
case ch3 <- y:
    // ...
default:
    // ...
}
```


## A Concurrent URL Checker

### Main Idea
You want to monitor the state of a bunch of URLs to possibly react when a
resource goes down or becomes too unresponsive.
This means that you are interested in the following:

- HTTP response codes (is the URL available or not)
- Latency (how long does the request take)

The idea is to write a simple program that polls a list of URLs, checks their
HTTP response codes and response time regularly and periodically prints their
state.


### Requirements

Your application should be able to handle:

- `n` number of URLs
- `c` number of concurrent URL checkers
- the setting of a checking interval (i.e., what's the period a URL should be
  checked)
- the setting of a status interval (i.e., how often does your application
  provide a status update to the user)


### Some Hints

- You probably want to have `c` concurrent URL checkers that receive URLs that
  need to be checked through a channel.
- You probably want to have some data structure that stores the state of all
  URLs and prints it regularly (status interval).
- You can use a HEAD request instead of a GET request, because we are not
  interested in the body of the response in this case.
