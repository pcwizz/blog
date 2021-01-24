+++
title = "Thoughts on Go"
date = 2020-12-04

[taxonomies]
tags = [ "Go", "programming" ]
+++


I was going to write a wall of text but that isn't really my thing so I'll just
present you with the points I wanted to share. Go is a practical languages I
enjoy using most days, although like everything crafted by the hands of humanity
it has imperfections.  Many of the thoughts listed here are made up for by the
community, proper code reviews, and additional tooling. Just keep in mind that
many developers work in Go without knowing/about (or wanting to know about) the
community, and therefor get lead astray by Go in the course of getting their job
done.

- `interface{}`
	- Negatives
		- Runtime type checking is expensive
		- Runtime type checking pushes failures into runtime
		- Makes function signitures less descriptive
	- Thoughts
		- Generics are one option but they aren't required
		- Code generation is already part of Go
			- Template Go code and generate for concrete types
			- Faster than calling reflect in a parser
			- Most use cases don't deal with unknown structures
- Error handling
	- The language is the fine the standard library error interface is lacking
	- Errors are recoverable and normal
		- Matching strings is slow
		- Matching strings is brittle
		- Standard error interface is comonly returned not a conrete error type
			- A superset interface would also achieve the same thing
				- Work arround lack enums with private types and a public interface
	- Panics are for wen the safest thing to do is stop everything
		- The aility to reover panics leads developers in the wrong direction
		- Recovering a panic requires switching on a `interface{}`
		- Panics are handled by the OS/container scheduluers restarting the process
- Implicit interface implementation
	- Negatives
		- Hard to navigate
		- Forces developers to read/remember the interface definition
		- Non intentional and misleading implementaiton
	- Thoughts
		- Should be easy to get a list of types that implement an interface
		- Developers should be able to user an interface without knowing its methods
			- i.e. know that they need a `Reader`from Y to pass to X
		- The shape might fit but the context can be completly different
- Concurrency model
	- Negatives
		- Go routines make it extremely easy to burn CPU doing nothing
		- Channels make code harder to read
		- Channels and Go routines are abused horribly in most codebases
		- Cancelation of routines is an unreliable horror show
	- Thoughts
		- Go routines aren't lazy they run even if not doing anything
		- Folloing a stack of function calls is much easier than folling a channel
		- Most channels yeild one item then close
			- That is better described as a future/promise
		- A handle should probalby be returned when a go routine is created
			- Passing in context and selecting on the canelation channel is fragile
			- Write busness logic not cancelation boiler plate
			- Interupt routine between any line, call defered calls exit
