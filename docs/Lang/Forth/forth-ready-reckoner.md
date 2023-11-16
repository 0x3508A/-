# Forth Ready-Reckoner

This is a quick and dirty way to keep Forth in my head.
Inspired by the famous [Forth LearnXinYminutes](https://learnxinyminutes.com/docs/forth/).
And another resource [Simple Forth](http://www.murphywong.net/hello/simple.htm)

## 1. Comments

```forth
\ This is a comment
\ Note: The space after slash "\ " Its important !.
\ Note: Any thing after "\ " is Ignored.

( This is also a comment but it's only used when defining words )
( Note: This comment requires space on both sides means )
(       It Starts with bracket space "( " and )
(        ends with space bracket " )" )
( Note: Any thing after " )" end will still be compiled and executed. )
```

## 2. Data Stack

```forth
\ All 'Numbers' stored into the "Data Stack". It's also the "Parameter Stack"
\ All programming in Forth is done by manipulating the parameter stack (more
\ commonly just referred to as "the stack").
5 2 3 56 76 23 65    \ ok

\ Those numbers get added to the stack, from left to right.
.s    \ <7> 5 2 3 56 76 23 65 ok

\ In Forth, everything is either a word or a number.
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Forth Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
