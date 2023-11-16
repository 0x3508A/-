# Starting Forth Notes

These notes were generated while reading the Book:
***Starting Forth* by Leo Brodie**

## Reading & TODO

TODO :

- [ ] Going Through the video of DonGolding Second part of Starting Forth Tutorial <https://www.youtube.com/watch?v=kKbbQmAzjKE>
- [ ] Next need to go through <https://www.youtube.com/watch?v=sPVz-qdCUd8>
- [ ] Watch this recommended by *Peter Forth* for `ForthWin` install <https://www.youtube.com/watch?v=vgfPqLBaUSE>

**Forth2020 Meeting Link <http://zoom.forth2020.org/>**

## `ROT` and `-ROT` operation

### `ROT` operation:
```
3 <- TOS  => 1 <- TOS
2            3
1            2
```
- Top of Stack to Second location in Stack
- Second of stack to third
- Third of Stack to Top
- Its more like **"Push Down the Stack"** like logical shift operations.

### `-ROT` operation
```
3 <- TOS => 2 <- TOS
2           1
1           3
```
* Top of stack to Third location in Stack
* Second of Stack to Top
* Third of Stack to second
* This corollary to `ROT` and is like **"Push Up the Stack"** operation.

## Stacks in Forth

### Parameter Stack
Parameter Stack or Data Stack also called as *"the Stack"* is used to hold computational parameters in Intreactive mode or for use with other words.

### Return Stack
The Forth system stores pointers to locations in *Dictionary* and *Word* definitions. This helps to move the context and return to the called location. Alternatively *within a Word* this can be used as *temporary location* to hold values.

#### Return Stack Operations

1. `>R` Takes a value off the *Parameter Stack* and pushes it on the *Return Stack*.
2. `R>` Takes a value off the *Return Stack* and pushes it on the *Parameter Stack*.
3. `I` or `R@` Copies the content from *Return Stack* and Pushes into *Parameter Stack*.
4. `I'` To get Second item in *Return Stack* and Pushes into *Parameter Stack*.
5. `J` To get Third item in *Return Stack* and Pushes into *Parameter Stack*.

**Note: Return Stack only works inside a WORD definition.**

## Loops in Forth

### Different types of Loops

#### Definite Loop

A loop that has a start, and termination condition known at the time of writing.

#### Indefinite Loop

A loop that has a start, and termination condition. However the termination time is dependent on the execution and various stimulus.

#### Infinite Loop

A loop that has a start but no termination condition, one can only eject out optionally based on certain conditions.

## Solutions

```forth
\ Chapter 5 Solutions
: sol1 ( a b c -- -ab/c ) */ NEGATE ;
: sol2 ( a b c d -- n-max ) max max max . ;
: sol3-f-to-c ( deg F -- deg C ) 32 - 5 9 */ ;
: sol3-c-to-f ( deg C -- deg F ) 9 5 */ 32 + ;
: sol3-c-to-k ( deg C -- deg K ) 273 + ;
: sol3-k-to-c ( deg K -- deg c ) 273 - ;

\ Chapter 6 Solutions
: c6STARS ( n -- ) 0 ?DO ." *" LOOP ;
: c6BOX ( n r -- ) 0 ?DO ( Main Loop )
  DUP c6STARS
  CR
  ( Main Loop End ) LOOP
  DROP ;
: c6\STARS ( r -- ) 0 ?DO ( Main Loop )
  I SPACES
  10 c6STARS
  CR
  ( Main Loop End ) LOOP ;
: c6/STARS ( r -- ) DUP 0 ?DO ( Main Loop )
  DUP I - SPACES
  10 c6STARS
  CR
  ( Main Loop End ) LOOP
  DROP ;
: c6/STARS2 ( r -- ) BEGIN ( Main Loop )
  1- ( Reduce the Count )
  DUP SPACES ( Adjusted as per the Current state of counter )
  10 c6STARS
  CR
  DUP 0= ( Checking at UNTIL  - false to continue )
  ( Main Loop End ) UNTIL
  DROP ;
: c6DIAMONDS ( r -- ) 0 ?DO ( Outer Loop )
  10 0 DO ( Top Loop )
  10 I - SPACES
  I 2 * 1 + c6STARS
  CR
  ( Top Loop End ) LOOP
  11 0 DO ( Bottom Loop )
  I SPACES
  10 I - 2 * 1 + c6STARS
  CR
  ( Bottom Loop End ) LOOP
  ( Outer Loop End ) LOOP ;


```

----
<!-- Footer Begins Here -->
## Links

- [Back to Forth Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
