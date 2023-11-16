# Lua Programming Language Hub

All about the small embed-able computer scripting programming language *Lua*

## Reference Materials

This document follows the [Lua 5.4 Manual](https://www.lua.org/manual/5.4/manual.html).

- Full Lua Programming Crash Course - Beginner to Advanced (Steve's teacher)
    - <https://www.youtube.com/watch?v=1srFmjt1Ib0>
- **Udemy Course** : <https://www.udemy.com/course/learn-lua-scripting-roblox/>
- Learn Lua in One Video
    - <https://www.youtube.com/watch?v=iMacxZQMPXs>
    - Website : <http://www.newthinktank.com/2015/06/learn-lua-one-video/>
    - An old but good tutorial to start with.
- Lua in 15 minutes old but a great refresher
    - <https://www.youtube.com/watch?v=kgiEF1frHQ8>
- Falling in LÖVE with Lua (For Game development)
    - <https://www.youtube.com/watch?v=3k4CMAaNCuk>

## Lua Binaries Locations

Main Archive <https://sourceforge.net/projects/luabinaries/files/>

Specifically for Lua 5.4.2 <https://sourceforge.net/projects/luabinaries/files/5.4.2/>

This also contains the source code and documentation.

In case of Linux :

```sh
pacman -S lua
```

## Comments

```lua
-- This is a line comment
--[[
    This is a multi line comment.
    That can extend many lines.

    **This would be Bold**

    #This is a Topic at level 1 like Markdown !!
]]
live = true -- This is a line comment behind a statement
```

## Basic Statements for Printing

```lua
-- This would print on screen
print('Hari Aum')
```

## Variables Types in Lua

- They need to start with either `_` or any Letter not a Number
- The are type sensitive hence `VarA`  is not the same as `varA`
- Directly used variables or dynamic variables are generally **Global**
- Variables defined by `local` have limited scope
- Basic Data types
    - Numbers (Only Floats are supported both real and whole)
    - Strings
    - Boolean `true` == String or Not nil values, `false` == `nil`
    - `nil` Empty or Undefined variables
    - Functions
- Higher Data types
    - Tables
        - Can have Numeric, String or Floating indexes
        - Can have multiple data types stored
    - Meta Tables - that define behavior of the original value under certain events
        - Meta methods are the functions attached to the meta table.
        - `getmetatable` and `setmetatable` functions help to query meta tables
    - Co-routines - special functions that can pause their execution or yield values
- `type` function allows to print the Type of a variable
    - `local age = 12 ; print(type(age))`
    - Note that the `;` here is separate 2 statements.

### Strings

```lua
 a = 'alo\n123"' -- Escape Characters and single quotes to form the string
 a = "alo\n123\"" -- Double quotes to escaped last double quote
 a = '\97lo\10\04923"' -- more escaped sequences in decimal ASCII and Octal
 -- Multi line string defintion

 -- Type 1
 -- Note that each new line in the string would be relicated as is.
 a = [[alo
 123"]]
 -- Note In this way the `escape sequence` can't be used

 -- Type 2
 -- Another way of defining multi line string definition
 a = [==[
 alo
 123"]==]
  -- Note In this way the `escape sequence` can't be used
```

### Numbers

```lua
-- Valid integer constants
    3   345   0xff   0xBEBADA

-- Valid Float constants
     3.0     3.1416     314.16e-2     0.31416E1     34e1
     0x0.1E  0xA23p-4   0X1.921FB54442D18P+1
```

### Boolean

There are only two *explicit* Boolean values `true` and `false`.

In terms of *inferred* Boolean values
- `nil` is also a `false` value
- Any value **other than `nil` or `false`** is *inferred* as `true`

### Variable Definition

```lua
-- a is Global in scope
a = 15
-- b is Local in scope
local b = 12
```

### Referencing for Tables and Meta tables

```lua
-- Bracket way to referncing the table members
t1 = { 11 , 21 , 31 }
print(t1[1]) -- This would print 11
-- Dot way of Refernce
t2 = {
 ["alpha"] = 12,
 ["beta"] = 22
}
print(t2["alpha"])
print(t2.alpha)
```

Note: Tables can also be heterogeneous.

```lua
-- This is also valid
student = {}
student.name = "Navin"
student.age = 10
print(student.name .. " is " .. student.age .. " years old.")
```

### Predefined Values

- `_VERSION` - This stores the Version **string** of the current Lua interpreter Virtual Machine
- `_ENV` - **Table** of all Global values

## Lua Keywords

```
     and       break     do        else      elseif    end
     false     for       function  goto      if        in
     local     nil       not       or        repeat    return
     then      true      until     while
```

## Lua Operands and Operators

```
     +     -     *     /     %     ^     #
     &     ~     |     <<    >>    //
     ==    ~=    <=    >=    <     >     =
     (     )     {     }     [     ]     ::
     ;     :     ,     .     ..    ...
```

**Note** that `#`, `..`, `::` and `...` are special operands.

### Comparison Operators

```
==    ~=(For Not Equal)    <=    >=    <     >
```

### Arithmetic Operators

```
+     -     *
/  (Arithmetic float division)
// (Arithmetic floor / Integer division)
%  (Arithmetic MODULUS or Reminder operator)
^  (Power-Of or Exponent Operator)
-  (unary Minu)
```

### Bitwise Operations

```
&   bitwise AND
|   bitwise OR
~   bitwise exclusive OR or XOR
>>  right shift
<<  left shift
~   unary bitwise NOT
```

### Logical Operations

```
or    and    not (Inversion of logic)
```

### Length Operator

```
#
```
Used to find length of various types of values.

### Concatenate Operator

```
..
```

This is used to Join string values.

### Label Operator

```
::
```

This is used to create the Labels used in case of [`goto` statement](#labeled-jump-or-infamous-goto).

### Operator Precedence in Lua

```
 or
 and
 <     >     <=    >=    ~=    ==
 |
 ~
 &
 <<    >>
 ..
 +     -
 *     /     //    %
 unary operators (not   #     -     ~)
 ^
```

## Lua Statements

### Blocks

- Typically statements are setup in Blocks which contain multiple statements
- Block of statements can be on the same line using `;` - Semi Colon separator
- Block is automatically created when defined in functions or control structures

### Chunks

- A set of Lua code statements in a Block that can be compiled is called a Chunk
- The unit of compilation of Lua is called a chunk. Syntactically, a chunk is simply a block
- These can have local variables and can access Globals using `_ENV` or `_G`
- One can pre-compile Chunks using `string.dump` function or `luac` tool
- Again these pre-compiled Chunks can be loaded using `load` function

### Assignments

- Lua supports move type assignments
- Lua also supports de-structuring

```lua
-- Simple Assignment
i = 3
-- Multi assignment
i, a[i] = i+1, 20
-- Swap assignmments
x, y = y, x
-- Multiple swap assignments
x, y, z = y, z, x
```

## Control Structure `if`

```bnf
	stat ::= if exp then block {elseif exp then block} [else block] end
```

- We have `if` as the main control structure
- The Block scope of `if` starts with `then`
- The `if` can have `else` or `elseif` followed by `then` blocks
- The `if` always ends in `end`
- Nested `if` will have multiple `end` denoting the scope of each `if` level.

## Labeled Jump or infamous `goto`

```bnf
	stat ::= goto Name
	stat ::= label
	label ::= ‘::’ Name ‘::’
```

- `goto` transfers control to a label
- Labels can be defined using `::` operand
- Labels can be empty statements or *void* statements if they do not perform any action

```lua
-- Trivial loop implemented using Goto
m = 2
:: upper ::
if m == 0 then
  goto endloop
else
  m = m - 1
  goto upper
end
:: endloop ::
```

## Loop Construct `while`

```bnf
stat ::= while exp do block end
```
- `while` loop is similar to common C variant and equivalent to other languages
- It tests the expressing to be true before even executing the code block
- It repeated computes the express to be true after every loop
- each `while` block begins with `do` and ends with `end`

```lua
-- Simple loop to print from 1 to 4
limit = 1
while limit < 5 do
	print(limit)
	limit = limit + 1
end
print("Done!")
```

## Loop Construct `repeat` `until`

```bnf
stat ::= repeat block until exp
```

- `repeat` `until` loop works like `do-while` loop in C
- This loop would execute the code block at least once before evaluating the expression
- The end expression must evaluate to false to continue the loop
- At the end of the code block the expression is evaluated for every loop

```lua
-- Print the count from 1 to 10
limit = 1
repeat
 print(limit)
 limit = limit + 1
until limit > 10
-- Notice that the above expression is false in beginning
print("done!")
```

## Loop Construct `for`

There are multiple ways of using `for` loop construct.

### Numerical `for` Loop

Here is how the syntax looks like:

```bnf
stat ::= for Name ‘=’ initial ‘,’ limit [‘,’ step] do block end
```

- `Name` is an numerical value *iterator local variable* or simply **`Control Variable`**
- `for` loop block always starts with `do`
- `for` loop block always ends with `end`
- The numerical `for` loop expressions contains 3 parts : *`initial`*, *`limit`* and *`step`*
    - **`Initial` Value** (Mandatory)
    - **Final Value** or **`Limit`** (Mandatory)
    - **Increment or Decrement `Step` value**. This is by default *increment by 1* (Optional)
    - The loop starts by evaluating once the three control expressions.
    - Their values are called respectively the *`initial`* value, the *`limit`*, and the *`step`*. If the *`step`* is absent, it *defaults to 1*.
    - If both the *`initial` value* and the *`step`* are **integers**, the loop is done with integers
    - Note that the *limit may not be an integer*.
    - Otherwise, the *three values are converted to floats* and the *loop is executed with floats*. **Beware of floating-point accuracy in this case**.
    - *`Step`* can be *positive* to give *progressive sequence* and *negative* to generate *decreasing sequence*
- First the *`limit`* is checked before executing the `block`. If the `initial > limit` then the whole loop is *skipped*.
- After the loop is completes an iteration the *`step`* is performed and `Name` is updated, then *limit* is checked.
- If the value(`Control Variable`) is still *below or equal*to the *`limit`* (**Special case for integer `for` loop**) the loop `block` is again executed. Else it exits. This **Special** case only occurs for default or *`+1` or `-1`* values for *`step`*.
- In case of *float* `for` loop the *`limit`* is not exceeded, means the loop exits if `Control Variable >= limit`.
- **IMPORTANT** You **should not change the value** of the *`control variable`* during the loop. If you need its value after the loop, assign it to another variable before exiting the loop.

Here are few Examples:

```lua
-- This one counts from 1 to 10 including it
-- It shows the *Integer* based loop
for i = 1, 10 do
 print(i)
end
-- Note that 10 is also incuded and the loop exits only after `i`
-- exceeds 10 as the Limit.
-- Also, note that the default increment is + 1
print(i)  -- This would `nil` since the iterator is always a local variable

-- Now a floating point loop
for i = 1, 2, 0.1 do
 print(i)
end
-- Note that the increment is 0.1 but the `initial` and `limit` are both integers.
-- Also, Note that this time the loop exits at 1.9 and not the 2.0 since its a Floating point

-- Now a Reverse loop
for i = 10, 1, -2 do
 print(i)
end
-- Note that the special case of `Control Variable < limit` is not followed
-- The loop exits if the `Control Variable <= limit`
```

### Generic `for` Loop

Here is how the syntax looks like:

```bnf
stat ::= for namelist in explist do block end
namelist ::= Name {‘,’ Name}
```

Or better

```lua
for var_1, ···, var_n in explist do body end
```

- All variables `var_1` ... `var_n` will be local variables become the `namelist`
- The `explist` can be of multiple types. The important characteristic is `explist` should be or have an `iterator function`.
- Each iteration the Lua calls the `iterator function` to obtain values for the `namelist`.
- If the `iterator function` returns `nil` then the loop terminates else it continues.
- **IMPORTANT** You **should not change the value** of the *`control variable`* in the `namelist` during the loop. If you need its value after the loop, assign it to another variable before exiting the loop.

Here are a few Examples :

```lua
-- Lets comare a few parameters
for student, score in pairs({["Kiran"]=10, ["Pratek"]=15, ["Mahesh"]=20}) do
 print("Student Name: ", student)
 print("Score       : ", score)
end
-- The `pairs` function creats the `iterator function` for the associated table.
```

## Loop Termination

- Lua only has one type of loop terminator `break`.
- This allows to exit the running loop to immediate next block.
- A `break` ends the innermost enclosing loop.

```lua
-- We would exit the loop if the value is greater than 5
for i = 1, 10 do
 if i > 5 then
  break
 end
 print(i)
end
print('Out of the Loop')
-- Only numbers till
```

## Functions


----
<!-- Footer Begins Here -->
## Links

- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
