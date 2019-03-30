# scripts

A small collection of shell scripts for personal use. You can copy or symlink these to a folder on your `$PATH` to run 'em from anywhere. As [recommended](https://askubuntu.com/a/308048) for user scripts, I use `~/bin`.



## get-ffdev

```
get-ffdev [DIR]
```

Downloads and extracts the latest copy of Firefox Developer Edition into the specified directory. I set this to `~/moz-dev/` by default so I don't have to bother with root, but you can always run it with `sudo` if you want it in a system directory (eg `sudo get-ffdev /opt`).

It creates a `firefox` folder inside the specified directory, so the path to the executable will be `$DIR/firefox/firefox`.

This is an alternative to finagling with `apt` PPAs if you only want Developer Edition for debugging and don't really care about having it as your regular browser. Otherwise, as far as I can tell, the correct PPA is [here](https://launchpad.net/~mozillateam/+archive/ubuntu/firefox-next).




## ginit

```
ginit [OPTIONS...] [PROFILE]
```

Automates git initialization with user profiles. Useful if you have multiple Github accounts and you're sick of setting `git config --local` properties every time. `ginit` is able to set the **user.name**, **user.email**, and **SSH remote origin** for your repository.

All arguments are optional. If you don't input a profile name, it will prompt you for one. You can still skip it; `ginit` will then initialize and configure the repository with only the arguments given. If you give it a profile that doesn't exist, `ginit` will create it and then run through the config options NPM-style. If you give it a profile that *does* exist, it will **update it** with whatever other arguments you include, and use the result to configure the repository.

Profiles are stored in the `~/.ginit` directory. To remove a profile or any of its settings, you must delete or modify its file in `~/.ginit/`.


### Options

#### `-n NAME`

Used to set `git config --local user.name`. Needs to be in double-quotes if there are spaces in it.

#### `-e EMAIL`

Used to set `git config --local user.email`.

#### `-s SSH_HOST`

Used to autoconfigure the remote URL for `origin`. To use SSH with multiple Github accounts on the same machine, you need to set up unique host entries for them in `~/.ssh/config`, like so:

```
Host whatever.github.com
    User foo
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gh-foo
```

Accordingly, every time you grab your repo's remote URL from Github and stick it in `git remote add origin`, it must be doctored to match: `git@github.com:foo/my-repo.git` becomes `git@whatever.github.com:foo/my-repo.git`. This gets kinda tedious, so `ginit` handles it for you when you set the necessary options. The `SSH_HOST` option is the `whatever.github.com` part.

Note that `ginit` **doesn't touch your SSH config**; that part is up to you. It only configures the remote URL for the repository you run it in.

#### `-u USERNAME`

Also used to autoconfigure the remote URL for `origin`. The `USERNAME` and `REPOSITORY` options are necessary to build the rest of the URL. This is the `foo` part in the above example.

#### `-r REPOSITORY`

Also used to autoconfigure the remote. This one is not saved in the profile; `ginit` will prompt you for it if you don't include it in the command, assuming `USERNAME` and `SSH_HOST` are set.



## rand

```
rand [[MIN] MAX]
```
Simple (pseudo)random number generator for integer ranges.

Takes one or two integer arguments. If one argument is given, returns a random number from 1 to `$1`. If two are given, returns a random number from `$1` to `$2`. If no arguments are given, it just returns bash's `$RANDOM`, which outputs a random positive (signed) 16-bit integer (0 to 32767).



## tt

```
tt [CMD...]
```
Open a new `xfce4-terminal` tab to run a foreground process. Runs everything after `tt` (for **t**erminal **t**ab) as though it were a normal command, and names the tab after the full argument string. Intended for use with Xfce4-terminal's drop-down mode (and probably not that practical otherwise), to make it easier to create tabs dedicated to foreground processes with meaningful names.



## unrar-all

```
unrar-all [-p PASSWORD] [DIR [TO_DIR]]
```

Extracts all `*.rar` files in the given directory. If a single directory argument is given, it's used as both the source and destination for the extracted files. If two are given, it uses the first as the source, and the second as the destintion. If no directory argument is given, it defaults to the current directory for both source and destination.

If the `-p` flag is passed with an argument, it will attempt to use that argument as the password for all archives. Otherwise, it calls `unrar` with `-p-` (disable password prompting) to avoid prompting for each password-protected archive in the directory.

It always calls `unrar` with `-o-`, meaning it will never attempt to overwrite files.
