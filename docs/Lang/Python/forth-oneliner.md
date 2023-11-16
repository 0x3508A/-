# Forth One Liner

A python script to make Forth word definition into a Single line.

A small tool to help in converting long Forth words or well described Forth words into single line form. This helps to reduce the transmission time when used with low bandwidth Serial connections.

To Use the script paste the definition of Forth word into the string and run the program, it would print the single line representation of the Forth word.
Currently this works with single Forth word.

[Script Revision V1.0](./forth-oneliner/forth-compress-v1.py.txt)

## Example
Example Forth Code:
```forth
: Guess-num2 ( num guess -- num[wrong] )
   2DUP =               \ n g (g=n)
   IF ." CORRECT" 2DROP \ (Empty)
    EXIT
   THEN
   OVER SWAP <          \ n (n<g)
   IF ." TOO HIGH"      \ n
    EXIT
   THEN
   ." TOO LOW"          \ n
;
```
Output:
```python
>>> %Run forth-compress.py
: Guess-num2  2DUP = IF ." CORRECT" 2DROP EXIT THEN OVER SWAP < IF ." TOO HIGH" EXIT THEN ." TOO LOW" ;
>>>
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Forth Hub](../Forth/README.md)
- [Back to Python Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
