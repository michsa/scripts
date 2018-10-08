# scripts

A small collection of shell scripts for personal use.

## rand

```
rand [[MIN] MAX]
```
Simple (pseudo)random number generator for integer ranges.

Takes one or two integer arguments. If one argument is given, returns a random number from 1 to `$1`. If two are given, returns a random number from `$1` to `$2`. If no arguments are given, it just returns bash's `$RANDOM`, which outputs a random positive 16-bit integer (0 to 32767).

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
