# Windows File type Association Issues

In Windows 10 and Windows 11 some of the files might not have the correct handlers associated.
And most difficult part is when Windows does not allow to change this association.

There are several ways to go about this.

However here is one that would surely solve the issue.

## Set File Type Association application

Reference 

<https://kolbi.cz/blog/2017/10/25/setuserfta-userchoice-hash-defeated-set-file-type-associations-per-user/>

Local **[PDF](./file-type-assoc-sfta/Set-File-Type-Assoc-SFTA-Danysys.pdf)** copy.

Source <https://github.com/DanysysTeam/SFTA>

Local copy of the executable (need to rename) :

**[SFTA-1.3.1.zip.archive](./file-type-assoc-sfta/SFTA-1.3.1.zip.archive)**

Apart from these you would need to visit the following registry path to find out the exact application names:

```reg
HKEY_CLASSES_ROOT
```

Scroll down to tree where `7-Zip.7z` or some thing that does not have dot `.` in front. From there we can find the actual application name we needed.

Finally we can alter the association with this:

```
SFTA,exe 7-Zip.rar .rar
```

This would associate the `.rar` extension to open directly with `7-Zip` application.

Here are some more documentation for the same:

- [change-file-type-association-in-W11using-PowerShell_Microsoft-Community.pdf](./file-type-assoc-sfta/change-file-type-association-in-W11using-PowerShell_Microsoft-Community.pdf)


----
<!-- Footer Begins Here -->
## Links

- [Back to Windows Hub](./README.md)
- [Back to Root Document](../README.md)

