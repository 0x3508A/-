# Golang : The Go Programming Language Hub

The web enabled simple language of the 20th Century.

## Topics

- **[Snippets](./snippets.md)** - Quick tricks to help with building Applications.

## Optimal way for Shrinking down Go Binary Sizes

```sh
GOOS=linux go build -ldflags="-s -w" cmd/*.go &&\
  upx --brute go
```

First during build we shrink down using `ldflags` ,
in the next stage we use `upx` to push further.

Reference:

<https://blog.filippo.io/shrink-your-go-binaries-with-this-one-weird-trick/>

## net/http Timeouts

<https://blog.cloudflare.com/the-complete-guide-to-golang-net-http-timeouts/>

## net/http TLS Harding

<https://blog.gopheracademy.com/advent-2016/exposing-go-on-the-internet/>

## Vercel Golang Settings

<https://vercel.com/docs/projects/environment-variables>

```
GO_BUILD_FLAGS = -ldflags '-s -w'
```

This would remove Debug Info and make Executable smaller.

## Listing all modules in a package

```sh
go list -m all
# all - Here means all the Modules
# This can be replaced with wild card searches like -
# github.com/boseji/...
```

## Replacing a Package with another using aliases

```sh
# To get help with the command
go help mod edit -replace
# Alias dependency with version
go mod edit -replace github.com/xyz/package@v1.9.1=github.com/boseji/package@v1.9.2
# This would replace the packages with alternate location.
# Though to the code it would appear that the code has changed
# but in reality its just aliased.
# Later when the 'xyz' software maintainer accepts this pull requeest
# this 'replace' can be removed.
```

## Crawling into the Website data

- `Colly` : <https://go-colly.org/>
    - `Colly` provides a clean interface to write any kind of `crawler/scraper/spider`.
- Goquery <https://github.com/PuerkitoBio/goquery> goquery brings a syntax and a set of features similar to [jQuery](http://jquery.com/) to the [Go language](http://golang.org/).
    - It is based on Go's [net/html package](https://pkg.go.dev/golang.org/x/net/html)
- The CSS Selector library [`cascadia`](https://github.com/andybalholm/cascadia).

## Naming Conventions in Golang : Go Bootcamp Course

Here is the Document: **[PDF](./README/udemy-go-bootcamp=naming+things.pdf)**

This is a shared material from the *Udemy Go Bootcamp Course* that I had bought some time earlier.

## Package and Scope Cheatsheet : Go Bootcamp Course

Here is the Document: **[PDF](./README/udemy-go-bootcamp=cheatsheet+for+packages.pdf)**

This is a shared material from the *Udemy Go Bootcamp Course* that I had bought some time earlier.

## Embedding Files into Golang Binary

<https://github.com/jteeuwen/go-bindata>

<https://scene-si.org/2017/08/22/embedding-data-in-go-executables/>

<http://www.edzynda.com/single-executable-web-apps-with-go-binary-assets/>

## For File Server Interface

<https://github.com/elazarl/go-bindata-assetfs>

## Web server Graceful termination

<https://github.com/facebookgo/grace>

----
<!-- Footer Begins Here -->
## Links

- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)


