## Forth Snippets

[Program Code](./forth-snippets/forth-words.forth.txt)

## Clear Stack - `do loop`

```forth

: .clearStack ( n >>>> n -- )
  depth ( Total size of Stack )
  0 ( Start with 0 index )
  do
    drop
    ( Remove item from stack )
  loop ;

: .C ( n >>>> n -- )
  .clearStack
  ( Shortcut to clean Stack )
  ;

\ Found another Way
: .c1 depth dup 0 > if for drop next else drop then ;
: .c2 depth dup 0 > if 0 do drop loop then ;
```
> A reason why this is better than `for next` is that in this loop comparison to zero does not effect
> the actual dept. But in case of `for next` the index *is subtracted and then checked*, hence a chance of
> inversion - which would be invalid.
> The last version `.c2` is the best of all worlds, it makes sure that we don't loop unless we have some thing in stack.

## Wait for Any Key - `begin until` loop

```forth
: waitForAnyKey ( -- )
  begin
    key ( Stores Key ASCII value at top of stack )
  until ( any value other than -1 in stack stops loop )
  ;
```

If the **TOS** contains `-1` before `until` then the loop would continue.

## Wait for any key 2 - `begin again` is Infinite loop

```forth
: waitForAnyKey2 ( -- )
  begin
    key ( Wait for Key )
    if ( Check for True Value )
      EXIT
    then

  again ;

```

## Downward Counter - `FOR` `NEXT` the Reverse Loop
```forth
: downCounter ( n -- )
  depth 0= if
    ." Need to give some size in stack "
  else
   dup
   0 <=
   if
    ." Need Positive non zero numbers "
   else
     1 -
     for
     i . cr
     next
   then
  then
  ;

```

## `2OVER1` Implemented using Return Stack
```forth
: 2OVER1- ( x y z -- x y z x y)
  over \ DS# x y z y   RS#
  >R   \ DS# x y z     RS# y
  ROT  \ DS# y z x     RS# y
  DUP  \ DS# y z x x   RS# y
  >R   \ DS# y z x     RS# y x
  ROT  \ DS# z x y     RS# y x
  ROT  \ DS# x y z     RS# y x
  R>   \ DS# x y z x   RS# y
  R>   \ DS# x y z x y RS#
  ;

\ Shorter Defintion
: 2OVER1 over >R ROT DUP >R ROT ROT R> R> ;
```

## Better `DO` loop for Avoid problems
```forth
: count_up ( n -- n )
  depth 1 >= invert if quit then
  dup 0 > invert if quit then
  1            \ seed the stack
  begin
    cr dup .   \ display current value
    1+ \ increment stack value
    over over <= \ test for stop value
  until \ if true, exit loop
  cr ." Stop Value= "
  . cr cr \ display stop value
;
```

There are two main problem this averts:
1. With no parameters
2. -1 or 0 as parameters

## Fast embedded Forth `FOR NEXT` Loops

```forth
: count_up2 10 for r@ . next cr ;
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Forth Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
