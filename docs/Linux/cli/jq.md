# `jq` json processor command

Lightweight and flexible command-line JSON processor

Sources - <https://github.com/stedolan/jq>

Documentation - <https://stedolan.github.io/jq/>

Online JSON **`jq` testing and Play Area** - <https://jqplay.org/>

***`jq`*** is like `sed` for JSON data - you can use it to slice and filter
and map and transform structured data with the same ease that `sed`,
`awk`, `grep` and friends let you play with text.

**`jq`** is written in portable C, and it has zero run-time dependencies.
You can download a single binary, `scp` it to a far away machine of the
same type, and expect it to work.

**`jq`** can mangle the data format that you have into the one that you
want with very little effort, and the program to do so is often shorter
and simpler than you'd expect.

Install:
```sh
sudo pacman -S jq
```

## Transform a list of strings to JSON array using `jq`

<https://stackoverflow.com/questions/68859957/transform-a-list-of-strings-to-json-array-using-jq>

```sh
cat list.txt | jq -Rn '[inputs]'

# Eliminate any empty Lines
cat list.txt | jq -Rn '[inputs|select(length>0)]'

# Work on Coma Separated Strings
cat list.txt | jq -R 'split(",")'
```

## Transform a JSON Array back to a List using `jq` and [`sed`](./sed.md)

```sh
jq .[] userwords.json | sed 's/"//g'
```

This would first print the JSON array in the file `userwords.json`.
Then using [`sed`](./sed.md) remove the quotes.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)

