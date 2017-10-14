# Assignment 01

These are just some additional notes regarding assignment 01 ([link to
assignment](https://docs.google.com/document/d/1x2Xd1g40frn6TXd2HciX5RICvysu9Jfn2DJ-8a76cHY/edit?usp=sharing)).

**Please feel free to make pull requests if you have something that might be
worth sharing with other students :-)**

## Potential Pitfalls


### Appengine Go 1.6 vs. 1.8

Using standard AppEngine, you can choose between Go 1.6 and 1.8.
You do that in your project yaml file:

```
runtime: go                    # the runtime (python, java, go, php)
api_version: go1.8             # go1.6 or go1.8, the runtime version

handlers:
- url: /.*                     # for all requests
  script: _go_app              # pass the request to the Go code

```



### AppEngine context vs. context.Background()

A Context carries a deadline, a cancelation signal, and other values across API boundaries.

In a local Go program, you can use
[context.Background()](https://golang.org/pkg/context/#Background): 

```go
ctx := context.Background()
```

(and then e.g., pass to a new BigQuery client)

However, you cannot use this context when running on AppEngine.
Here you need to use an [AppEngine context](https://cloud.google.com/appengine/docs/standard/go/reference#NewContext):

```go
ctx := appengine.NewContext(r)
```
Which is based on a HTTP request.


### Authenticating for BigQuery

To use the BigQuery API, your application must be authenticated.
There is different ways to achieve this (e.g., using Oauth).

It's possible to create a Service account key and download it in JSON format (for more see [here](https://developers.google.com/identity/protocols/application-default-credentials)).
However, when the Google Cloud SDK is installed, there is an easier option.
Just execute:
```
gcloud beta auth application-default login
```

to establish the link to your Google cloud account.


### Querying BigQuery

There is a BigQuery client library (`cloud.google.com/go/bigquery`) that makes
it really easy to query.

See code below.

```go
import (
	
    "cloud.google.com/go/bigquery"

	"golang.org/x/net/context"
)

...

// query returns a slice of the results of a query.
func query(proj string) (*bigquery.RowIterator, error) {
	ctx := context.Background()

	client, err := bigquery.NewClient(ctx, proj)
	if err != nil {
		return nil, err
	}

	query := client.Query(`SELECT * FROM ...;`)
	// Use standard SQL syntax for queries.
	// See: https://cloud.google.com/bigquery/sql-reference/
	query.QueryConfig.UseStandardSQL = true
	return query.Read(ctx)
}

```

