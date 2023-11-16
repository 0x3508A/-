# Forth Language Hub

All about the wonder and lean Forth Language.

## Topics

- **[Starting Forth Notes](./starting-forth.md)** - Reading the basic book of Forth.
- **[Forth Ready Reckoner](./forth-ready-reckoner.md)** - Quick Revision on Forth Syntax.
- **[Forth Code snippets](./forth-snippets.md)** - Some useful words in Forth.
- **[`gforth` Notes](./gforth.md)** - Some notes on the most featured Forth.
- **[ForthWin Insights](./forthwin.md)** - Peter Forth's Windows Forth.
- **[ESP32Forth Notes](../../HW/TOOLS/esp32forth.md)** - ESP32 based Forth.
- **[Forth2020 Group](./Forth2020-group-website-links.md)** - Active group for help with Forth.
- [Forth One Liner using Python](../Python/forth-oneliner.md) - Script to compress the Forth Code into one line.
    - Forth version of the same work written by Vladimir Gumenuk
        - [Program Code](./README/Code-Compress.forth.txt)
- **Poor man's Case implementation** in Forth by Klaus Schleisiek - presented in Forth2020 Zoom meeting
    - [Program Code](./README/poor-mans-case.forth.txt)
- **[ChatGPT Explains Forth](./chagpt-explain-forth.md)** - AI Experiment to simplify explaining Forth to a novice.

## Others

??? note "Important note on Forth Variables/Values/Stack by Peter Forth"

    ```forth
    \ Declare
    variable  peter
    \ Write 10 to Variable
    10 peter !
    \ Read Values
    peter @ .
    ```

    `value` is a mix of a variable with a constant.
    It is a variable that does not need `@` like a constant, but you can change its value every time with `TO`.
    Peter prefer `VALUE` all the time

    but constant variable and `value`  are almost the same in Forth , there is no difference except  the `@` & `!` and `TO` &  `+!`  which is `+TO`

    ```forth
    \ Define
    10 value price
    \ Read
    price .
    \ Write
    12 TO price
    ```

### Forth Terminal

Howard ++ here a link to a practical shell for editing and loading forth source:

<https://sites.google.com/view/myffshell/>

Looking forward to a fulfilling session. Learning Forth the right way from luminaries.

Project Forth Works

<https://wiki.forth-ev.de/doku.php/pfw:welcome>

## [Flash Forth Programs for ATmega328P](../../HW/AVR/flash-forth-programs.md)

More details in this [document](../../HW/AVR/flash-forth-programs.md).

## [**My4th**](../../HW/Projects/my4th.md) - A Logic Gate only Computer running Forth

Here are [more details](../../HW/Projects/my4th.md) on this marvel.

## Forth editing support in [Notepad++](../../Windows/notepadpp.md#notepad-support-forth-source-code-file-with-custom-extension)

More details on how to get this done in [this document](../../Windows/notepadpp.md#notepad-support-forth-source-code-file-with-custom-extension).

----
<!-- Footer Begins Here -->
## Links

- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
