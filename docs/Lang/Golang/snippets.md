# Golang Snippets

## List all Serial Ports in PC

```go
package main

import (
  "fmt"
  "go.bug.st/serial.v1"
  "log"
)

func main() {
  ports, err := serial.GetPortsList()
  if err != nil {
    log.Fatal(err)
  }
  if len(ports) == 0 {
    log.Fatal("No serial ports found!")
  }
  for _, port := range ports {
    fmt.Printf("Found port: %v\n", port)
  }
}
```

## Import Collection Templates into the Current Template

```
{{ template "insert" . }}
```
Within an HTML template the above would search for the
template named `insert` and inject it into the current
template file.

## Define Layout for templates

This is specially useful for creating base layout for HTML templates.

The Base layout Looks like:
```html
{{ define "base" }}
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ block "title" . }}{{- end }}</title>
    <link rel="stylesheet" href="/static/css/style.css">
    {{- block "css" . }}
    {{- end }}
</head>

<body>
    {{ template "navbar" . }}
    {{- block "content" .}}
    {{ end -}}
    {{- block "footer" .}}
    {{ end -}}
    {{- block "js" .}}
    {{ end -}}
</body>

</html>
{{- end }}
```

Here there this base layout also includes a sub template `navbar`.
This is another level of partial parts that can help to create the main
structure of the page.

The `block` specified can be defined in the pages that employ this template. If the specific block is not defined in any template then
it would be blank in the generated HTML template.

Here is an example of a page using the base template:

```html
{{ template "base" .}}
{{ define "title" }}
Major's Suite at Fort Smythe
{{- end }}
{{ define "content" }}
{{/* Start Page Image */ -}}
<img src="/static/images/marjors-suite.png" class="img-fluid" alt="Major's Suite">
{{- /* End Page Image */}}
{{/* Start Main Content */ -}}
<main class="container">
    <header class="row mt-4">
        <div class="col text-center">
            <h1>Major's Suites</h1>
            <p>This is where the royal major's spend their leisure time.
            </p>
        </div>
    </header>
    <section class="row">
        <div class="col text-center">
            <a class="btn btn-success btn-lg" href="/make-reservation">Make Reservation Now</a>
        </div>
    </section>
</main>
{{- /* End Main Content */}}
{{- end }}
```

In this page the first line actually imports the template `base`.
This defines the layout of the document. Then it defines `title`
which is used by `base` to specify the `<title>` Tag.
Similarly for the content.

## Golang template sub-expressions

```
{{ $name := (index .StringMap "name") }}
```
In this expression the template variable `$name` is assigned
the value from the data passed to the template.
In the *Sub-expression* we are trying to fetch a value from a Map `StringMap map[string]string` in the data passed.
This statement can actually be written in go as:
```go
name := data.StringMap["name"]
```
And its doing exactly. In order to make sure that the *Sub-expression*
executes in the template we enclose it in `()`.

Here is another example:

```html
<a {{ if or (eq $name "generals") (eq $name "majors") }}class="nav-link dropdown-toggle active"{{- else }}class="nav-link dropdown-toggle"{{- end }} href="#" id="navbarScrollingDropdown" role="button"
                        data-bs-toggle="dropdown" aria-expanded="false">
                        Rooms
                    </a>
```
In this case we are using the *Sub-expression* to evaluate the comparison of values.

## Catching Ctrl+C or SIGINT or Break in Execution

We would use a `done` channel approach for this.

```go
  c := make(chan os.Signal, 1) // Signal Channel
  done := make(chan bool, 1)
  signal.Notify(c, syscall.SIGINT, syscall.SIGTERM)

	go func() {
		<-c           // Wait for the Ctrl + C
		fmt.Println() // Clear line below Ctrl + C
		done <- true
	}()

  <-done
  fmt.Println("Exiting")
```

Note that all channels are having a buffer of `1`.
This additional buffer would help to correctly accept the `Ctrl+C` in most OS. Additionally it helps to keep
the posting into the channel non-blocking.

## Quickly Host a directory Over Web

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	fmt.Println("Starting Sever on localhost:8080")
	http.ListenAndServe(":8080", http.FileServer(http.Dir(".")))
}
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Golang Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
