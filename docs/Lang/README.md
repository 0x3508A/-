# Computer Programming Languages Hub

This hub hosts computer languages related documents.

## Hubs

- **[Forth Hub](./Forth/README.md)**
- **[Golang Hub](./Golang/README.md)**
- **[Nim Hub](./Nim/README.md)**
- **[Lua Hub](./Lua/README.md)**
- **[Python Hub](./Python/README.md)**
- **[Nix Hub](./Nix/README.md)**

## Topics

- **[ASEM-51 the last 8051 Macro Assembler](./ASEM-51.md)** - Works both on Windows and Linux.
- **[Bash](./bash.md)** - Programming language of the Linux shell.
- **[C Programming](./c-notes.md)** - Some notes and tricks in C-Programming Language.
- **[CSS](./css.md)** - Styling of the Web
- **[gemini Protocol](./gemini.md)** - New Internet protocol
- **[Haskell](./haskell.md)** - The premier functional programming language.
- **[JavaScript](./javascript.md)** - Most complex web language.
- **[Rust](./rust.md)** - Secure programming language.
- **[UNICODE](./unicode.md)** - The UTF-8 that rules all emojis !!
- **[Markdown](./markdown.md)** - The documentation language of these notes.

## Bootstrap HTML 5

Quick Way to Include Boostrap in your project without download

Home: <https://getbootstrap.com/>

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" crossorigin="anonymous">
```

JS (No Popper):

```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.min.js" integrity="sha384-nsg8ua9HAw1y0W1btsyWgBklPnCUAFLuTMS2G72MMONqmOymq585AcH49TLBQObG" crossorigin="anonymous"></script>
```

## Single Command to convert Markdown files to Org Mode

[Link](https://emacs.stackexchange.com/q/5465)

Reference:

- <https://emacs.stackexchange.com/q/5465>
- <https://emacs.stackexchange.com/questions/5465/how-to-migrate-markdown-files-to-emacs-org-mode-format>

Here is a single Command that can convert all Markdown files into Org Mode format:

```sh
find . -name \*.md -type f -exec pandoc  -f markdown -t org -o {}.org {} \;
```

Of course you would need to have `pandoc` installed first.

On Windows it would become really difficult.

## Zig Programming Language

Home page : <https://ziglang.org/>

Getting Started with Zig Programming : <https://ziglearn.org/>

## `clang-format` - Code Formatting of the open source

Home Page : <https://clang.llvm.org/docs/ClangFormat.html>

Video explaining how to go about installing in **Windows**:
<https://www.youtube.com/watch?v=rd8-aVc3Krg>

- Need LLVM Builds page for downloading LLVM tools <https://llvm.org/builds/>

- Got to `clang-format plugin for Visual Studio` on the builds page.

In `ubuntu` **Linux** :
```sh
sudo apt install clang-format
```

VS Code extensions for `clang-format` :

<https://marketplace.visualstudio.com/items?itemName=xaver.clang-format>
by Xaver Hellauer

Alternate from LLVM group:

<https://marketplace.visualstudio.com/items?itemName=LLVMExtensions.ClangFormat>

## [Free Developer Resources](https://freestuff.dev/)

<https://freestuff.dev/>

This is a great resources to find more about Free stuff for *software/cloud development needs*.

## Free Serverless services

- [ ] <https://yepcode.io/>
- [ ] <https://vercel.com/>
- [ ] <https://www.cyclic.sh/>
- [ ] <https://unhosted.org/>
- [ ] <https://5apps.com/>
- [ ] <https://appwrite.io/>
- [ ] <https://supabase.com/>

## Rob Pike's 5 Rules of Programming

<https://users.ece.utexas.edu/~adnan/pike.html>


- Rule 1.

    You can't tell where a program is going to spend its time. Bottlenecks
    occur in surprising places, so don't try to second guess and put in a
    speed hack until you've proven that's where the bottleneck is.

- Rule 2.

    Measure. Don't tune for speed until you've measured, and even then
    don't unless one part of the code overwhelms the rest.

- Rule 3.

    Fancy algorithms are slow when n is small, and n is usually small.
    Fancy algorithms have big constants. Until you know that n is
    frequently going to be big, don't get fancy. (Even if n does get big,
    use Rule 2 first.)

- Rule 4.

    Fancy algorithms are buggier than simple ones, and they're much harder
    to implement. Use simple algorithms as well as simple data structures.

- Rule 5.

    Data dominates.

    If you've chosen the right data structures and
    organized things well, the algorithms will almost always be self-evident.

    Data structures, not algorithms, are central to programming.

Pike's rules 1 and 2 restate **Tony Hoare's** famous maxim

***"Premature optimization is the root of all evil."***

Ken Thompson rephrased Pike's rules 3 and 4 as
**"When in doubt, use brute force."**.

Rules 3 and 4 are instances of the ***design philosophy KISS***.

Rule 5 was previously stated by *Fred Brooks* in **The Mythical Man-Month**.

Rule 5 is often shortened to ***"write stupid code that uses smart objects"***.

----
<!-- Footer Begins Here -->
## Links

- [Back to Root Document](../README.md)