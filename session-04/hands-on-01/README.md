# Hands-On

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
