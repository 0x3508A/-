# Enlarge Scroll bar Width - XFCE / Gnome

Reference: <https://forum.manjaro.org/t/enlarge-scroll-bar-width-how-to/47579>

Create the file `~/.config/gtk-3.0/gtk.css` if it doesn’t already exists and paste the following CSS rules in it:

```css
scrollbar, scrollbar button, scrollbar slider {
    min-width: 10px;
    min-height: 10px;
}
```

The `min-width` value is for the vertical scrollbar and the `min-height` value is for the horizontal scrollbar. You can change the values according to your liking.

**After saving the file, you have to logout/login in order to see the changes.**

!!! note
    Bare in mind that the above CSS rule will change the *scrollbars* in *all Gtk-based applications*.

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
