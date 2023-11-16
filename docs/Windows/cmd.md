# Windows Command Line

Here are a few tips and tricks collected over time.

## References

- **Primary Reference: <https://ss64.com/nt/>**
- <https://ikrima.dev/dev-notes/win-internals/cmd/>

## `robocopy` - Robust File and Folder Copy Command.

**[PDF File](./cmd/Robocopy-Robust-File-Copy-Windows-CMD-SS64-com.pdf)** Details about the `robocopy` Command

### Fast Directory Copy using `robocopy`

```batch
robocopy "<src>" "<dst>" /MIR
```

Where

- `<src>` Source directory Path in quotes
- `<dst>` Destination Directory Path in quotes
- `/MIR` - Mirror directory tree - Means it includes all files and directories.

### Remove empty directories using `robocopy`

```batch
robocopy "%FOLDER%" "%FOLDER%" /S/PURGE
```

- Initially use `set FOLDER="c:\path\to\folder"` to setup the path.
- The `/S` option works on all subdirectories but *not empty* ones.
- And `/PURGE` option cleans up all not listed by `/S`.

## Fast Directory Erase

Reference: <https://stackoverflow.com/questions/186737/whats-the-fastest-way-to-delete-a-large-folder-in-windows/6208144#6208144>


```batch
@rem del: needed bc 'rmdir' cannot erase directory containing files
@rem  /f: force delete read-only files
@rem  /s: deletes specified files from all subdirectories
@rem  /q: quiet mode, suppress confirmation to delete with global wildcard
@rem rmdir:
@rem  /s: recursive erase of directory including files
@rem  /q: quiet mode, suppress confirmation to delete directory tree
del /f/s/q "%FOLDER%" > nul
rmdir /s/q "%FOLDER%"
```

- Initially use `set FOLDER="c:\path\to\folder"` to setup the path.
- The above would remove the files first in all subdirectories and then folders.

## List directory in detail

```batch
@rem list all symlinks
@rem  /al: display files with Reparse Points attribute
@rem  /s: recurse
@rem  /b: leave out heading information/summary
dir /al/s/b "<tgt>"
```

- Folder full path `"<tgt>"` in quotes.

## Windows SymLink or Windows Shortcuts

| Link Types           | To Files? | To Folders? | Across Volumes?      | May Not Exit? | Command                                                       |
| -------------------- | --------- | ----------- | -------------------- | ------------- | ------------------------------------------------------------- |
| Shortcut             | Yes       | Yes         | Yes                  | Yes           | `<Executable> -t target -a args`                              |
| Symbolic link        | Yes       | Yes         | Yes                  | Yes           | `mklink /D LinkDir TargetDir` or `mklink LinkFile TargetFile` |
| Hard link            | Yes       | No          | No                   | No            | `mklink /H LinkFile TargetFile`                               |
| Junction (soft link) | No        | Yes         | Yes on same computer | Yes           | `mklink /J LinkDir TargetDir`                                 |

- **Symbolic Links/Directory Junctions**: implemented using reparse points
- **Junction/Symlink**: main difference is when looking at a remote server
    - **Junctions** are processed at the server
    - **Directory symlinks** are processed at the client
- **Hard Links**: implemented with multiple file table entries pointing to same `inode`
    - If original filename is deleted, the hard link will still work because it points directly to the data on disk.
- **References** [here](https://ss64.com/nt/mklink.html) and [here](https://superuser.com/questions/343074/directory-junction-vs-directory-symbolic-link)

### `mklink` Command - for Creating Symbolic and Hard links under Windows

**[PDF file](./cmd/MKLink-Windows-CMD-SS64-com.pdf)** detailing the `mklink` command.

From <https://ss64.com/nt/mklink.html>

## Batch file Parameters

- `%0`: pathname of batch script itself
- `%1`....`%9`: reference argument by number
- `%*`: refers to all the arguments e.g. `%1 %2 %3 %4 %5 ...%255`
- expansion modifiers
    - `%~f1`:     expand `%1` to a fully qualified path name e.g. `c:\utils\MyFile.txt`
    - `%~d1`:     expand `%1` to a drive letter only e.g. `c:`
    - `%~p1`:     expand `%1` to a path only including trailing `\` e.g. `\utils\`
    - `%~n1`:     expand `%1` to a file Name without file extension or path - `MyFile`  or if only a path is present, with no trailing backslash, the last folder in that path.
    - `%~x1`:     expand `%1` to a file eXtension only - `.txt`
    - `%~s1`:     change the meaning of `f`, `n`, `s` and `x` to reference the *Short 8.3 name* (if it exists)
    - `%~1`:      expand `%1` removing any surrounding quotes `"`
    - `%~a1`:     display the file attributes
    - `%~t1`:     display the date/time
    - `%~z1`:     display the file size
    - `%~$PATH:1` search the PATH environment variable and expand `%1` to the fully qualified name of the first match found
- expansion modifiers can be combined
    - `%~dp0` expand to full path of the batch script itself
    - `%~dp1` expand `%1` to a drive letter and path only
    - `%~sp1` expand `%1` to a path shortened to *8.3 characters*
    - `%~nx2` expand `%2` to a file name and extension only

## `cmd` Executable Parameters and Scripts

`#!batch cmd [options] "command" [parameters]`

- Starts new shell in same window, optionally running `program/command/batch`
    - environment is **inherited** but changes not persisted back
    - `/c`: runs command and auto terminates
    - `/k`: runs command and remain open
        - This is useful for testing, e.g. to examine variables
    - If `/c or /k`, remainder of command line processed as an immediate command in new shell
    - For multiple commands, surround with quotes and use command separator `&` or `&&` - e.g. `#!batch cmd /c "foo.cmd && bar.cmd"`
- [Even more usecases](https://ss64.com/nt/cmd.html)

**[PDF File](./cmd/CMD-exe-(Command-Shell)-Windows-CMD-SS64-com.pdf)** Detailing the `cmd` parameters.

## `start` Command Parameters and Scripts

`#!batch start "title" [options] "command" [parameters]`

- Start a program/command/batch in a new window
    - environment is **inherited** but changes _not_ persisted back

!!! info "behavior is different depending on context/command"
    if **command** is shell command or batch file: processed with `cmd.exe /K` i.e. window remains open
    inside batch script, a `start` without `/wait` launches program and continues script execution.

- [more usecases](http://ss64.com/nt/start.html)

**[PDF File](./cmd/Start-Start-a-program-Windows-CMD-SS64-com.pdf)** Detailing the `start` command parameters.

## `call` Command Parameters and Scripts

`#!batch call [parameters]`

- Invoke a batch script or subroutine
    - environment is **inherited** but changes are persisted back
- [more usecases](https://ss64.com/nt/call.html)

!!!warning
    Invoking batch script from another without `call` or `start` terminates first script and allows second one to take over

**[PDF File](./cmd/Call-Windows-CMD-SS64-com.pdf)** Detailing the `call` command.

### Variables Scope to Local
  - use `setlocal` and `endlocal` to keep variables in different files separate

## Keyboard Pause at end

- `pause`,`puase >nul`: to pause execution until key press
- [Reference](https://stackoverflow.com/questions/988403/how-to-prevent-auto-closing-of-console-after-the-execution-of-batch-file)

## Launching Commands / Scripts

| Window | OnExecute | OnFinish   | Changes        | Example                                                |
| ------ | --------- | ---------- | -------------- | ------------------------------------------------------ |
| new    | continue  | auto close | non-persistent | `#!batch start "title" cmd /c bar.exe arg1 arg2`       |
| new    | continue  | keep open  | non-persistent | `#!batch start "title" cmd /k bar.exe arg1 arg2`       |
| new    | block     | auto close | non-persistent | `#!batch start "title" /wait cmd /c foo.cmd arg1 arg2` |
| new    | block     | keep open  | non-persistent | `#!batch start "title" /wait cmd /k foo.cmd arg1 arg2` |
| same   | block     | auto close | non-persistent | `#!batch cmd /c foo.bat arg1 arg2`                     |
| same   | block     | keep open  | non-persistent | `#!batch cmd /k foo.bat arg1 arg2`                     |
| same   | block     | keep open  | persistent     | `#!batch call foo.bat arg1 arg2`                       |

## Escaping path names and parameters in Commands

| Scenario                                        | Example                                                                                       |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Run a program and pass a Filename parameter     | `#!batch cmd /c write.exe c:\docs\sample.txt`                                                 |
| Run a program and pass a Long Filename          | `#!batch cmd /c write.exe "c:\sample documents\sample.txt"`                                   |
| Spaces in Program Path                          | `#!batch cmd /c "c:\Program Files\Microsoft Office\Office\Winword.exe"`                       |
| Spaces in Program Path + parameters             | `#!batch cmd /c "c:\Program Files\demo.cmd" Parameter1 Param2`                                |
| Spaces in Program Path + parameters with spaces | `#!batch cmd /k ""c:\batch files\demo.cmd" "Parameter 1 with space" "Parameter2 with space""` |
| Launch Demo1 and then Launch Demo2              | `cmd /c ""demo1.cmd" & "demo2.cmd""`                                                          |

## Command Redirection and Pipe

- Redirect command output to a file

    `#!batch command > filename`

-  APPEND into a file

    `#!batch command >> filename`

- Type a text file and pass the text to command

    `#!batch command < filename`

- Pipe the output from `commandA` into `commandB`

    `#!batch commandA | commandB`

-  Run `commandA` and then run `commandB`

    `#!batch commandA & commandB`

- Run `commandA`, if it succeeds then run `commandB`

    `#!batch commandA && commandB`

- Run `commandA`, if it fails then run `commandB`

    `#!batch commandA || commandB`

- If `commandA` succeeds run `commandB`, if it fails `commandC`

    `#!batch commandA && commandB || commandC`

- [More Info](https://ss64.com/nt/syntax-redirection.html)

**[PDF File](./cmd/Command-Redirection-Pipes-Windows-CMD-SS64-com.pdf)** Details about command redirection and pipes.

## Batch file Execute as Administrator (Auto)

Adding this piece of code before any `batch` file would allow it to automatically escalate privileges to get into the *Administrator* mode.

Means you get a **UAC** prompt when the `batch` file is run.

```batch
@echo off
:: ----------------------------------------------------------
:: Ensure admin privileges
fltmc >nul 2>&1 || (
    echo Administrator privileges are required.
    PowerShell Start -Verb RunAs '%0' 2> nul || (
        echo Right-click on the script and select "Run as administrator".
        pause & exit 1
    )
    exit 0
)
```

!!! note "PowerShell"

    This needs PowerShell to be available in windows.

----
<!-- Footer Begins Here -->
## Links

- [Back to Windows Hub](./README.md)
- [Back to Root Document](../README.md)
