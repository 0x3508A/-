# `xh` web tool

The better `curl` and `wget` alternative in rust-lang.

## Website <https://github.com/ducaale/xh>

`xh` is a friendly and fast tool for sending #HTTP #requests. It re-implements as much as possible of HTTPie's excellent design.

Written in #Rust !

## Installation in Manjaro
```sh
sudo pacman -S xh
```

## Usage Examples

```sh
# Send a GET request using xh
xh httpbin.org/json

# Send a POST request with body {"name": "ahmed", "age": 24} using xh
xh httpbin.org/post name=ahmed age:=24

# Send a GET request with querystring id=5&sort=true using xh
xh get httpbin.org/json id==5 sort==true

# Send a GET request and include a header named x-api-key with value 12345
# using xh
xh get httpbin.org/json x-api-key:12345

# Send a PUT request and pipe the result to less using xh
xh put httpbin.org/put id:=49 age:=25 | less

# Download and save to res.json using xh
xh -d httpbin.org/json -o res.json
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)

