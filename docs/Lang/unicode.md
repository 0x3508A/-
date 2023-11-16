# UNICODE

Home Page : <https://home.unicode.org/>

Charts : <https://www.unicode.org/charts/>

Specification : <https://www.unicode.org/versions/Unicode13.0.0/>

## Important Code Pages

Devanagari : <https://www.unicode.org/charts/PDF/U0900.pdf>

Vedic Extensions: <https://www.unicode.org/charts/PDF/U1CD0.pdf>

## Examples

```
For Aum: U+0950 = ॐ

Precise Aum : U+090A U+0901 U+1CEC U+1CDE = ऊँᳬ᳞
              |Devanagari|     |Vedic|
```

## Brahmi <https://www.unicode.org/charts/PDF/U11000.pdf>

## Math Symbols

- Arrows <https://www.unicode.org/charts/PDF/U2190.pdf>
- Letters <https://www.unicode.org/charts/PDF/U1D400.pdf>
- Operations <https://www.unicode.org/charts/PDF/U2200.pdf>
- Geometric Shapes <https://www.unicode.org/charts/PDF/U25A0.pdf>
- Technical Symbols <https://www.unicode.org/charts/PDF/U2300.pdf>

### Math Example

```
Hisenburg's Uncertainty principle:
∆𝓟 x ∆𝓋 ≳ 1

My Name in special Chars:
𝓐𝓫𝓱𝓲𝓳𝓲𝓽 𝓑𝓸𝓼𝓮

```

## Windows Unicode

Article <https://unicode-explorer.com/articles/how-to-type-unicode-characters-in-windows>

Entering Unicode characters in Windows is easy if you know the exact, hexadecimal Unicode code-point number (e.g. [U+1F600](https://unicode-explorer.com/c/1F600)):

- Press and hold `Alt`,

- type `+` (on the numeric keypad),

- now type in the hexadecimal code-point sequence: `1`, `F`, `6`, `0`, `0`,

- and finally release `Alt`.

Now the typed-in text (`+1F600`) should disappear, and be replaced by the actual Unicode character, e.g. [😀](https://unicode-explorer.com/c/1F600).

You may need to set a registry key to enable this feature:
in `HKEY_Current_User/Control Panel/Input Method`, set `EnableHexNumpad` to `"1"`.
If the registry key does not exist yet, create it with type `REG_SZ` or `String` Type.

## Linux Unicode

Press **Shift+Ctrl+U**, release U, enter the hexadecimal (`0123456789abcdef`) Unicode character code point, then release **Shift+Ctrl**. An *underlined* `u` followed by the number will be displayed as you type.

Alternatively, press (and release) **Shift+Ctrl+U**, then, while *underlined* `u` is displayed, enter the hexadecimal Unicode character code point followed by `<Return>`.

```kyb
Shift+Ctrl+U 00f4   ô   (&ocirc;)
Shift+Ctrl+U 2203   ∃  (&exist;)
```

## Compose key

Keying the combination **Shift+AltGr** (in that order), releasing these keys, then entering two other keys will produce a special character. Many of these will be the reasonable result of `overtyping` the character keys, eg.

```kyb
Shift+AltGr  ~  a -->  ã  (&atilde; in HTML)
Shift+AltGr  /  o -->  ø  (&oslash; in HTML)
Shift+AltGr  o  c -->  ©  (&copy; in HTML)
Shift+AltGr  c  o -->  ǒ  (&#334; in HTML)
```

In some cases where the option for this *Compose Key* is not available
Use the **Pause/Break** key which is no longer used.

### Manjaro XFCE Compose Key Setting

- *Keyboard Settings* -> *Layout*
- **Compose** Key = Pause

### Reference

- <https://help.ubuntu.com/community/ComposeKey>
- <https://www.x.org/releases/X11R7.7/doc/libX11/i18n/compose/en_US.UTF-8.html>
- <https://help.ubuntu.com/community/GtkComposeTable>
- <https://tstarling.com/stuff/ComposeKeys.html>
- <https://fsymbols.com/keyboard/linux/compose/>
- <https://www.linuxquestions.org/questions/linux-newbie-8/full-list-of-entering-special-characters-with-compose-key-922021/>
- <https://www.pixelbeat.org/docs/xkeyboard/>

----
<!-- Footer Begins Here -->
## Links

- [Back to Computer Programming Languages Hub](./README.md)
- [Back to Root Document](../README.md)
