# 02. Web Programming in Go

This session gives a re-cap on web foundations and architectures of web systems
and web services. In the hands-on part I, you will develop web clients and
servers, work with JSON for encoding and PostgreSQL. In part II you deploy your
application to Google Cloud and learn how to store data in Google's Cloud
Datastore and retrieve remote resources.

## Outline

- Web foundations (Client-Server, HTTP)
- Architectures of Web Systems
- Web Services:
    - Data Encoding (XML, (binary) JSON, protocol buffers)
    - Data Access Styles (REST, SOAP, RPC)
    - Data Communication (HTTP, [web-]sockets)
- Database Connections
<!--- Logging over network to service-->
<!--- Errors and Failures-->
<!--- Different ways to do routing-->

## [Slides](http://go-talks.appspot.com/github.com/jf87/scalable_web_systems/sessions/02/slides/session02.slide)

## Readings
Go Web Programming chapter 1 and 2


### Questions

1. What steps happen when a client performs a GET request to a web server?


## Hands-on: Part I

1. [Web Clients*](hands-on-01/README.md)
2. [Web Servers*](hands-on-02/README.md)
3. [Input validation and status codes*](hands-on-03/README.md)
4. [JSON encoding and decoding*](hands-on-04/README.md)
5. [Data Storage: Postgresql and MongoDB](hands-on-05)


## Hands-on: Part II

6. [Deploying to App Engine*](hands-on-06)
7. [Storing data in the Datastore*](hands-on-07)
8. [Retrieving remote resources with urlfetch*](hands-on-08)

*Based on Francesc's great [go-web-workshop](https://github.com/campoy/go-web-workshop).


## Further Reads
- https://golang.org/doc/articles/wiki/
