# 04. JSON encoding and decoding

Go's standard library provides JSON encoding and decoding with the package
[`encoding/json`](https://golang.org/pkg/encoding/json).

## Encoding and decoding JSON

Let's first learn how to encode and decode JSON in a general way and we'll
see afterwards how to do it inside of an HTTP server.

### JSON and Go structs

The easiest way to encode and decode JSON objects with Go is to create a Go
type which matches the structure of the JSON object we want to decode.

So given a JSON object like this:

```json
{
	"name": "gopher",
	"age_years": 5
}
```

We would create a type containing the same fields:

```go
type Person struct {
	Name     string
	AgeYears int
}
```

Note that the all the identifiers (both type and fields) start with an uppercase
letter. This is because only identifiers starting with an uppercase are exported
outside of a package. So if the field `Name` was `name` the `encoding/json`
package wouldn't be able to even know it is there.

Fortunately we can use field tags to modify what name is used in the JSON form
for each Go field.

For instance we would add the following field tags to the previous example:

```go
type Person struct {
	Name     string `json:"name"`
	AgeYears int    `json:"age_years"`
}
```

_Note_: the backticks ```(`)``` are just a different way to write strings in Go.
They allow you to use double quotes `(")` and to expand across multiple lines.

For more info on structs read
[this section](https://tour.golang.org/moretypes/5) of the Go tour.

### Encoding Go structs to JSON

To encode a Go struct we use a
[`json.Encoder`](https://golang.org/pkg/encoding/json#Encoder), which provides
a handy `Encode` method.

```go
func encode() {
	p := Person{"gopher", 5}

	// create an encoder that will write on the standard output.
	enc := json.NewEncoder(os.Stdout)
	// use the encoder to encode p, which could fail.
	err := enc.Encode(p)
	// if it failed, log the error and stop execution.
	if err != nil {
		log.Fatal(err)
	}
}
```

This code snippet shows how to handle errors every time we encode a value,
and while in the example it seems impossible to have an error, consider that
the encoder output could be sent through a network connection.

You can try the code with the `go run` tool, or using the Go playground
[here](https://play.golang.org/p/rsO0Vk-9Xl).

### Decoding JSON objects into Go structs

The same way we have a `json.Encoder` we have a `json.Decoder` and its usage
is very similar.

```go
func decode() {
	// create an empty Person value.
	var p Person

	// create a decoder reading from the standard input.
	dec := json.NewDecoder(os.Stdin)
	// use the decoder to decode a value into p.
	err := dec.Decode(&p)
	// if it failed, log the error and stop execution.
	if err != nil {
		log.Fatal(err)
	}
	// otherwise log what we decoded.
	fmt.Printf("decoded: %#v\n", p)
}
```

Note that the parameter for `dec.Decode` is not `p` but `&p`. This is a
pointer to the variable `p` so the `encoding/json` package can modify the
value of `p`. Otherwise we would pass a copy of `p` and any modifications
would be without side effects.

You can read more about pointers in the [Go
tour](https://tour.golang.org/moretypes/1).


### Parsing JSON without knowing the schema

In above example, we know the structure of the JSON object beforehand. If this
is the case, you should always create a matching Go type. However, there are
cases where the JSON structure is not known before. So how can you decode such data?

In Go, you do that by parsing the JSON into `interface{}` instead of a struct
and then inspecting the data.
`interface{}` is known as the empty interface. It specifies zero methods and
thus all types implement the empty interface (every type implements at least
zero methods).
This means that it can hold any types.
So, to decode it, you need to walk the datastructure using Go's [type
assertions](https://tour.golang.org/methods/15). For example, the following
function walks through the underlying data structure of an `interface{}` and
detects its types:

``` go
func printJSON(v interface{}) {
	switch vv := v.(type) {
	case string:
		fmt.Println("is string", vv)
	case float64:
		fmt.Println("is float64", vv)
	case []interface{}:
		fmt.Println("is an array:")
		for i, u := range vv {
			fmt.Print(i, " ")
			printJSON(u)
		}
	case map[string]interface{}:
		fmt.Println("is an object:")
		for i, u := range vv {
			fmt.Print(i, " ")
			printJSON(u)
		}
	default:
		fmt.Println("Unknown type")
	}
}
```

## encoding/json + net/http = web services!

Let's have another look at the `http.HandlerFunc` type:

```go
type HandlerFunc func(ResponseWriter, *Request)
```

### Encoding JSON onto a http.ResponseWriter

As we mentioned before `http.ResponseWriter` implements the method `Write` and
therefore satisfies the `io.Writer` interface required by `json.NewEncoder`.

So we can easily JSON encode a `Person` on an HTTP response:

```go
func encodeHandler(w http.ResponseWriter, r *http.Request) {
	p := Person{"gopher", 5}

	// set the Content-Type header.
	w.Header().Set("Content-Type", "application/json")

	// encode p to the output.
	enc := json.NewEncoder(w)
	err := enc.Encode(p)
	if err != nil {
		// if encoding fails, create an error page with code 500.
		http.Error(w, err.Error(), http.StatusInternalServerError)
	}
}
```

### Decoding JSON from a http.Request

The `http.Request` type is a struct and it has a field named `Body` of type
`io.ReadCloser`, an interface with the methods `Read` and `Close`.

Since the signature of the method `Read` matches the one in `io.Reader` we can
say that `io.ReadCloser` is an `io.Reader` and therefore we can use the `Body`
of a `http.Request` as the input of a `json.Decoder`.

[embedmd]:# (examples/app.go /func decodeHandler/ /^}/)
```go
func decodeHandler(w http.ResponseWriter, r *http.Request) {
	var p Person

	dec := json.NewDecoder(r.Body)
	err := dec.Decode(&p)
	if err != nil {
		http.Error(w, err.Error(), http.StatusBadRequest)
		return
	}
	fmt.Fprintf(w, "Name is %v and age is %v", p.Name, p.AgeYears)
}
```

### Exercise 

1. Register above decodeHandler (as described in [part 2](../hands-on-02)) so
   you have a simple web service. You can then test it using curl:

```bash
$ curl -d '{"name": "gopher", "age_years": 5}' http://localhost:8080/
Name is gopher and age is 5
```

## Exercise - Shopping List

Living on a student budget can be tight sometimes. You want to build a service
that allows you to create a smart shopping list with the things you need and
the supermarket where you can find them for the cheapest price. So let's build
a web service for that:

The application should allow you to:

- Add a new item to the list
- Remove a single item
- Remove all items
- Return the total price for all your items
- Return all items that you need to buy in a single supermarket

You should have HTTP endpoints for all above.
For example you might have:
`/items` that returns all items currently on the list if you send a GET request
and allows you to add a new item when sending a POST request with a JSON object.

A JSON object for a shopping item might have the following structure:

```json
{
    "name": "milk",
    "supermarket" : "netto",
    "price" : 10.5
}
```

Note: It's fine to keep all the data in memory for this exercise.

# Congratulations!

You've successfully built a web application where the backend and the frontend
interact via JSON messages over HTTP requests: that's pretty much as RESTful as
it gets!

Continue to the [next section](../hands-on-05/README.md) to learn how to deploy your
application on Google's cloud platform.

