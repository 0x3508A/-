# Firefox Browser

## Firefox Profile using `Profilemaker`

Home Page: <https://ffprofile.com/>

Steps:

1. Download the `profile.zip` containing everything (You might already have this)
2. Go to the Firefox Excitable Directory in Windows typically `Program Files\Mozilla Firefox` and open an Shell.
    This would ensure that the Firefox exe is in PATH.
3. Now open the **Profile Manager**: `firefox -no-remote -ProfileManager`
4. Create a new profile in the desired directory.
5. Let Firefox to open for the first time using the new profile just created.
6. Type `about:support` into the URL bar.
7. Press the `open profile folder` button. This would open a Explorer window.
8. Close Firefox window.
9. Delete everything from the new profile (you will lose all existing data from the profile).
10. Unzip the `profile.zip` archive into the folder.
11. Now the Profile is installed and can be used to open Firefox with the same.
12. Remember to Update all extensions once the Firefox opens with the new Profile.

## Directly opening Firefox with the desired profile

- Add Firefox EXE to the PATH
- Type the command `firefox -no-remote -P profilename`

## Good Firefox Privacy Script by `simeononsecurity`

<https://github.com/simeononsecurity/FireFox-Privacy-Script>

Just download and Install.

Common:

```sh
git clone https://github.com/simeononsecurity/FireFox-Privacy-Script.git
cd FireFox-Privacy-Script
```

Adding the [Repo-archive](./firefox-browser/FireFox-Privacy-Script-master.zip)
just in case the things go down.

Windows:

```powershell
.\sos-firefoxprivacy.ps1
```

Linux:

```sh
sudo chmod +x ./sos-firefoxprivacy.sh
sudo bash ./sos-firefoxprivacy.sh
```

## Firefox Profile Modification using `arkenfox/user.js`

Sources: <https://github.com/arkenfox/user.js>

Wiki with Documentation: <https://github.com/arkenfox/user.js/wiki>

## Firefox Profile Modification Help (Outdated)

<https://chrisx.xyz/blog/yet-another-firefox-hardening-guide/>

----
<!-- Footer Begins Here -->
## Links

- [Back to Misc. Hub](./README.md)
- [Back to Root Document](../README.md)
