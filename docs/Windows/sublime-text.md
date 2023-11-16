# Sublime Text Editor

## Why Sublime

Sublime Text is a sophisticated text editor for code, markup and prose.

Download Sublime Text (4) from: <https://www.sublimetext.com/download>

## Sublime Package Control (optional)

For a quick and easy way to setup plugins and other handy things for Sublime install **Sublime Package Control**.

This is optional but comes in handy if you'd like to preview your markdown files in your browser.

<https://packagecontrol.io/>

After installing Sublime Package Control you press `C-P` or `Ctrl+Shift+p`.

## Markdown to PDF in Sublime Text

Sublime setup to quickly convert Markdown files to PDF through LateX with [Pandoc](#what-is-pandoc).

### Install Markdown Preview

Search for *Markdown Preview* and click it, this will install a handy tool for you to preview your markdown files in your browser without much effort.

To use simply press `C-P` or `Ctrl+Shift+p` again and type in `markdown`, select *Markdown Preview* and select browser.

## What is `pandoc` ?

If you need to convert files from one markup format into another, `pandoc` is your swiss-army knife.
`pandoc` can convert documents in markdown, reStructuredText, textile, HTML, DocBook, LaTeX, MediaWiki markup, TWiki markup, OPML, *Emacs Org-Mode*, Txt2Tags, Microsoft Word docx, LibreOffice ODT, EPUB, or Haddock markup.

You download `pandoc` from here: <http://pandoc.org/installing.html>

You will need `pandoc` to convert your Markdown files to PDF files.

Heck, you might even want to try and search for `pandoc` in the *Sublime Package Control* and install the plugin.  This should allow you to convert your files as well. Since this wasn't working for me at all and since I really couldn't get my head around it, I decided to make a custom build option for Sublime.

## Custom Sublime Build

First off we need to create a new **Sublime-Build**.
Go to `Tools > Build System > New Build System ...`

This opens a file, replace the content with:
```json
{
    "shell_cmd": "pandoc -o \"$file.pdf\" \"$file\" && open -a Preview \"$file.pdf\"",
    "selector": "text.html.markdown",
    "path": "/usr/texbin:$PATH"
}
```

If you're not on a *Mac*:
```json
{
    "shell_cmd": "pandoc -o \"$file.pdf\" \"$file\"",
    "selector": "text.html.markdown",
    "path": "/usr/texbin:$PATH"
}
```

Save your new custom build to `Sublime Text 4/Packages/User` and name it as you desire. Now after saving go back to `Tools > Build System` and select your new build.

Now whenever you are writing your markdown and wish to convert it to PDF just press `C-b` or `<f7>` and it will convert it on the spot.

**PS:** If you wish to edit your custom build, you can find your build at this location on a *Mac*:
`~/Library/Application Support/Sublime Text 4/Packages/User/`

## Custom template (optional) LaTeX

Grab your custom LaTeX template and edit your custom build that you made in the previous step.
```json
{
    "shell_cmd": "pandoc --template=\"/Users/User/.pandoc/default.latex\" -o \"$file.pdf\" \"$file\" && open -a Preview \"$file.pdf\"" ,
    "selector": "text.html.markdown",
    "path": "/usr/texbin:$PATH"
}
```

Where `User` is your Username of your Macbook and 'default.latex' is the name of your LaTeX template.  I grabbed my template from:
<https://github.com/jgm/pandoc-templates/blob/master/default.latex>

After the first line starting with `\documentclass`,
you can start writing your custom packages / fonts as you desire.

Here's an example of what was added on line 2 to start off with:

```latex
%%%%%%%%%%% HELVETICA %%%%%%%%%%%%%%%
%\usepackage[scaled=0.86]{helvet}
%\renewcommand\familydefault{\sfdefault}
%\usepackage[T1]{fontenc}

%%%%%%%%%%% OPEN SANS %%%%%%%%%%%%%%%
%\usepackage[default,osfigures,scale=0.8]{opensans}
%\usepackage[T1]{fontenc}

%%%%%%%%%%% QUATTROCENTO %%%%%%%%%%%%%%%
%\usepackage[sfdefault]{quattrocento}
%\usepackage[T1]{fontenc}

%%%%%%%%%%% LAYOUT %%%%%%%%%%%%%%%
\usepackage[english]{babel}
\usepackage{blindtext, fontenc, setspace}
\usepackage[margin=2.5cm]{geometry}
```

Just remove the `%` from the font you wish to use. For more fonts go to:
<http://www.tug.dk/FontCatalogue/sansseriffonts.html>

### Example

An example showing how the file looks after converting to PDF is added to the repository with the name `readme.md.pdf`.

----
<!-- Footer Begins Here -->
## Links

- [Back to Windows Hub](./README.md)
- [Back to Root Document](../README.md)
