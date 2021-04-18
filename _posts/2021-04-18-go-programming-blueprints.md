---
title: "Review: Go Programming Blueprints, 2nd Edition"
---

I learned to program in R, and for the first two years of my professional
career worked exclusively in python. I maintained legacy java code occasionally,
but didn't (and don't) have any desire to write new systems in java. I find
the language ridiculously clunky and unintuitive. The ecosystem is madness; poor
documentation pervades. So much code has been written in java that
new developers are expected to automatically adopt frameworks like Spring and
SpringBoot. I'm sure there are some programmers that
enjoy writing Java code, but I am not one of them. I'd rather do requirements
gathering
than try to open up IntelliJ and invalidate every Maven cache on my
system and then restart 4 times just to get imports to resolve. These are, of
course, personal problems. There are many developers who
write better code that I could ever imagine, and they do it in Java. I don't
understand these people. Perhaps they actually reason about the world as
though everything were an object with methods to call. Or maybe they enjoy
looking at 50 line stack
traces, and get excited when they see that `...` indicating another 50 lines
were truncated. Well written code should be elegantly beautiful. That's
something Java will never be. And to those who say that Java 1.8+ fixes
these issues, I say, "no". No matter how much it's redesigned, its past will
forever harbor the unslayable dragons of poor design.

Learning Go was an entirely different experience. Documentation is centralized
with [https://golang.org/pkg/](https://golang.org/pkg/) and
[https://pkg.go.dev/](https://pkg.go.dev/); and it's designed to
force you to write
simple, clear code. Unlike java, which
requires an extremely specific object oriented way of thinking about problems,
I didn't feel constrained by the language . With Go I
could use a function, a method, a closure, etc.
The language also includes the best set of tooling of any language I've worked
in, the `go` tool. It trivializes the accessory elements of software development
(running tests, handling dependencies, building, etc),
so that you can focus on programming, not on configuring Maven. Best of all, Go
programs compile into native machine code, so there's no need to deal with a
VM, like the JVM, which is usually fine until it isn't, at which point you need
to decide whether to tune it to get your program to run correctly or whether
you should just blow your brains out.

I recently read Mat Ryer's _Go Programming Blueprints, 2nd Edition_ in order to
familiarize myself with elements of the language that I don't encounter on a
day-to-day basis. The book is well written with chapters broken into
consumable projects. This leaves the reader with a sense of accomplishment at
the end of each chapter. Most chapters have a web server component, after all,
this is what Go was built for. Ryer covers a lot more than just web servers, with
sections devoted to cmd line utils, Google App Engine, and deploying apps
using Docker.

If there's one thing that I took away from the book, it's this: Go is the
language to use if you're writing a web server. This is why:

```go
package main

import "net/http"

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/hello/", func(rw http.ResponseWriter, r *http.Request) {
		rw.Write([]byte("Hello World"))
	})

	http.ListenAndServe(":8080", mux)
}
```

Saving that program in a directory named `api`, running `go build`, and then
`./api` is all you need to do to spin up a webserver at `http://localhost:8080`.
For someone that comes from the data engineering side of the industry
(read: doesn't build web servers
for a living), that's ridiculously simple, and it's all written in the standard
lib. No framework needed. There are actually websites that exist for
generating Java code which will do the same thing[^1]. With autocomplete and
autoimport enabled in my editor, this code took me maybe 45 seconds to write,
and it's a single, 12 line, 225 character, file. No config or project level
files needed.

[^1]: [https://start.spring.io/](https://start.spring.io/)

One of the things that's readily apparent from this code is how readable Go
code is (at least the std lib). Say you wanted to implement another route. You
could check the documentation, or you could just read the code and see that we
should add a new handler (with signature:
`func(rw http.ResponseWriter, r *http.Request)`) to
the multiplexer and register it at the route.

That readability hints at the thought that's gone into building this
language. It truly is a language built for making software engineering simpler,
and that makes it a joy to work in. Ryer's book highlights the important
components of the language, especially for building web servers, and teaches
the correct (i.e. simplest and cleanest) ways of leveraging those components.
