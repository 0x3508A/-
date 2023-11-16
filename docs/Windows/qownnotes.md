# QOwnNotes

Home Page : <https://www.qownnotes.org/>

Main Repository : <https://github.com/pbek/QOwnNotes>

## Building QOwnNotes on Windows

First we need to get the `Qt6` Install done.
This needs to be specifically for the **MinGW 64-bit** tools.
Also need to use **LTS** versions only for better support.

Next, begin building on the PC.
First open a **Qt Terminal** they create shortcuts in *Start Menu* while installing `Qt`.
Use the **`Qt6 MinGW 64-bit`** one for this.
Since, `git` is already installed and made to be used with command prompts it should work here.

```powershell
git clone --depth 1 https://github.com/pbek/QOwnNotes.git

git submodule update --init

cd QOwnNotes\src

lrelease QOwnNotes.pro

qmake
```

`2023-05-05 10:51.27-577`
Faced and error here ` QT: websockets core5compat`

Open the **Qt Maintenance tool** And select **Add and Remove Components**.
Then click on next.

Under the **Qt 6.x** version that you have installed select - **Qt 5 Compatibility Module**
Then under the **Additional Libraries** select **Qt Websockets**.

And after this:

```powershell
make
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Windows Hub](./README.md)
- [Back to Root Document](../README.md)





