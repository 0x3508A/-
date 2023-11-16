# `git` SCM or Version Control System

SCM - Source control and Management

## [Git and GitHub for Beginners - Crash Course](https://www.youtube.com/watch?v=RGOj5yH7evk)

Great video from freeCodeCamp.org channel:

<https://www.youtube.com/watch?v=RGOj5yH7evk>

Got it reference from Hackaday:

<https://hackaday.com/2023/10/17/linux-fu-deep-git-rebasing/>

## [Pro GIT(https://git-scm.com/book/en/v2)] Book

<https://git-scm.com/book/en/v2>

**[PDF](./git/progit.pdf)** Copy Locally.

## Squash Multiple Commits in Git

```sh
git reset --soft HEAD~3 && \
git commit --edit -m"$(git log --format=%B --reverse HEAD..HEAD@{1})"
```

Here the `HEAD~3` indicates how many previous commits will be squashed together.
In this case it shows that `3` commits from the current `HEAD`.

## Ignore Non tracked files for status

```sh
git config --local status.showUntrackedFiles no
```
This means that new files or ignored files.

## Change the Default branch name to `main`

Due to all folks including **Github** switching the default branch to `main` there have been lot of troubles.
Here is a quick fix for the same by adding the following to `.gitconfig` file:

```sh
[init]
      defaultBranch = main
```

## Generate Empty Commits to Initiate the CI or Action

Reference: <https://www.freecodecamp.org/news/how-to-push-an-empty-commit-with-git/>

```sh
git commit --allow-empty -m "Empty-Commit"
# To add Signing
# git commit --allow-empty -sm "Empty-Commit"

# Finally Push the Commit
git push origin main
```

## Git make the repository Super Compact

```bash
# Expire all the unreachable content and pack the repo in the most
# compact way.
git gc --aggressive --prune=now

# also clear the reflog as well
git reflog expire --expire=now --all
```

This would force to compress the whole data blocks again.

### Normal way to perform this

```bash
git gc
```

This would just remove some trailing ends but not compress the whole repo.

## Commit specific files

```sh
git commit -m "current notes." file1 file2 fileN
```

## List files that are deleted

```sh
git ls-files --deleted
```

## List files that are changed

```sh
git ls-files --modified
```

## List files that are added

```sh
git ls-files --others
```

## Add files to staging

```sh
# Add deleted files to staging.
git ls-files --deleted -z | xargs -r -0 git rm

# Add modified files to staging.
git ls-files --modified -z | xargs -r -0 git add
# or
git add -u

# Add new files to staging.
git ls-files --others -z | xargs -r -0 git add
```

## Commit what are in staging

```sh
git commit -m 'Commit comment here....'
```

## List files in staging area

```sh
git diff --name-only --cached
```

## Un-stage all files

```sh
git reset HEAD -- .
```

## Remove a single file from staging

```sh
git reset HEAD -- <file>
```

## Git Bare Repository

### What is Exactly ?

This is new Trick that can be used to store a copy of the repository on any disk.

Like your local **github.com**.

Reference:

- <https://www.atlassian.com/git/tutorials/dotfiles>
- <https://www.youtube.com/watch?v=tBoLDpTWVOM> Video by DT on the same topic

### Creating a Git Bare Repository

```sh
git init --bare
```

This would create a source repository in *current directory*.
It would not contain the `.git` directory but all its constituents.
This can be location referenced as the local version of **github.com**.

Wonderful isn't it.

You can use this technique to Backup or keep *local copy* of all things.
Just take the bare repositories and not all the files.

If you want to do it in some other place than current directory :

```sh
git init --bare $HOME/dotfiles.git
```

### Important Note about Bare Repository Naming

Its considered a better idea to name the bare repository directories with `.git` suffix. So the above command should be really.

```sh
git init --bare $HOME/dotfiles.git
```

This convention helps to denote the specific directory is a **Bare Repository**.

### Adding Alias for working with bare repository

You can create a alias for operating with this bare directory :

```sh
alias cfg='/usr/bin/git --git-dir=$HOME/dotfiles --work-tree=$HOME'
```

In you `$HOME/.bashrc` file.

Allows you use `cfg` command to work with **git** on *home directory*.

!!! note

    You might need to change the `/usr/bin/git` based on your PC or Linux setup.

By using the *git bare repository*, you can have **nested git repositories** in your *home directory* and there will not be any issue with keeping things straight.
That is the reason for the *git bare repository* and having an alias (`cfg`).

### Ignore Non tracked files for status on bare repository

Typically in a **git** repository one would get to see the non tracked or new files that are not part of the repository with the `git status` command.

To avoid that:

```sh
cfg config --local status.showUntrackedFiles no
```

This is configuring the *bare git repository configuration* to ignore the *untracked* or non-tracked files.
So, when you want to actually add the files you can do something like:

```sh
cfg add /path/to/file
cfg commit -m “A short message”
cfg push
```

This would be very akin to common **git** flow.

### Fix Common Problems with `ref` in Bare Repository

Reference: <https://stackoverflow.com/a/21877681>

This is query posted in `stackoverflow` having the same problem.

Problem starts with :

```
warning: remote HEAD refers to nonexistent ref, unable to checkout.
```

This would lead to problems in cloning this Bare Repository.
The reason is that the `HEAD` file points to a wrong location.
Typically it would point to `master` but if you have changed this then it should actually point to that.

To fix this go to the `remote_repo.git` bare repository directory.
Edit the `HEAD` file.
Change `ref: refs/heads/master` to `ref: refs/heads/your_branch`.
This should fix the issue.

In many of the latest Github repositories we have started using `main` instead of master.
This was the cause of the problem.

Another automatic fix is by adding the following to `.gitconfig`:

```sh
[init]
      defaultBranch = main
```

### Commands for using Bare Repository

In order to run basic `git` commands with a Bare Repository one needs to add the `--git-dir=` to the git command.
Like we did in the [earlier section](#adding-alias-for-working-with-bare-repository).

## Signing commits with SSH Key - NOT WORKING !!

Reference: <https://calebhearth.com/sign-git-with-ssh>

!!! warning

    This is not working !!

It's normal to sign commits using `gpg` keys however
it is also possible to sign commits using `SSH` keys.

First to enable `Commit Signing`:

```sh
git config --global commit.gpgsign true
```

Next change the `GPG signing` method:

```sh
git config --global gpg.format ssh
```

Find more about your `SSH` Keys loaded in the system using
the *listing* command:

```sh
ssh-add -L
```

If you need help generating the `SSH` Key here is the [Github instructions](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) for it.

Now, set the signing key:

```sh
git config --global user.signingkey "ssh-ed25519 <your key id>"
```

In our case:

```sh
git config --global user.signingkey \
        "ssh-ed25519 AAAAC3...8Af/iOnh 0x3508a@users.noreply.github.com"
```

You can do an empty commit to add a signed commit:

```sh
# do this inside a Git repository only
git commit --allow-empty --message="Testing SSH signing"
```

Next, signatures can also be verified and Listed:

```sh
# Show the Commit Signature
git show --show-signature #Inside a git Repository

# First time this might fail so add it this way
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
touch ~/.ssh/allowed_signers
# Adding Identity to Signing
echo "caleb@calebhearth.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIM9V5SC0UdggJItk8StyYrJTj4eSArjuz4kgqXRy8hnf" \
> ~/.ssh/authorized_signatures
# The show signature command will work.
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
