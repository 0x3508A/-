# Writing Golang programs For LinkIt Smart 7688

This was inspired by the Post:

- <https://zyfdegh.github.io/post/202002-go-compile-for-mips/>
- Here is the **[PDF version](./golang-linkit/Building-Go-Programs-for-MIPS.pdf)**.

## Starting with A basic Program

```go
package main

/**
 * Example Program for Golang running on MT7688 MIPS platform
 */

import "fmt"

func main() {
	fmt.Println("Hari Aum !")
}
```

We would like to compile this to a version that would run on the **LinkIt Smart 7688**.

## Build Commands

```sh
#!/usr/bin/env bash

# Example program for Golang compilation targeting MIPS on MT7688

# Build command
GOOS=linux GOARCH=mipsle GOMIPS=softfloat go build -trimpath -ldflags="-s -w" -o HariAum

# Compression Command
upx -9 HariAum
```

**Note:** We are using the `mipsle` instead of the normal MIPS core. This is to make the program compatible to **LinkIt Smart 7688**.

Finally we are also shrinking it with `upx` tool.

## Archive

Here is the **whole project **along with the *executable* :

**[Project archive + Executable 7-Zip file](./golang-linkit/goMipsTrial.7z)**

----
<!-- Footer Begins Here -->
## Links

- [Back to main LinkIt Article](./linkit-smart-7688.md)
- [Back to OpenWrt and MT7688 Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)



