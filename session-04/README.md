# 04. Concurrency

This lecture provides a background on the OS process abstraction. It then
introduces the concept of concurrency and concurrent programming. Finally it
presents Go's CSP concept using several examples in Go.

## Outline

1. Processes and Threads
    - OS Basics
    - Synchronization Mechanisms
2. Concurrency
3. Concurrent Design in Go
4. goroutines and channels
5. Examples
    - Load Balancer
    - Query a Replicated Database
6. Beyond Single Nodes


## [Slides](http://go-talks.appspot.com/github.com/jf87/scalable_web_systems/session-04/slides/session04.slide)

## Readings
- Go Web Programming chapter 9
- [Hoare, C. A. R. (1978). "Communicating sequential processes". Communications of the ACM.](https://dl.acm.org/citation.cfm?id=359585)

### Questions

1. What is the difference (besides the syntax) of the concurrency model
   presented in Hoare's paper and the one implemented in Go?
2. Investigate how the HTTP server in Go is implemented. How does it handle multiple requests?

## Hands-on

1. [Hands-on-01](hands-on-01/README.md)


## Further Readings

- [Herb Sutter (2005). The Free Lunch Is Over: A Fundamental Turn Toward Concurrency in Software](http://www.gotw.ca/publications/concurrency-ddj.htm)
- [Herb Sutter (2012). Welcome to the Jungle](https://herbsutter.com/welcome-to-the-jungle/)

### On Concurrency in Go

- https://blog.golang.org/context
- https://blog.golang.org/pipelines
- https://blog.golang.org/advanced-go-concurrency-patterns
- https://blog.golang.org/share-memory-by-communicating
- https://blog.golang.org/go-concurrency-patterns-timing-out-and
