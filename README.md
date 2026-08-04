# Git first version (un official) repo

This repository is just a fixed version of [First commit of git by Linus Torvalds 2005 7 April](https://github.com/git/git/tree/e83c5163316f89bfbde7d9ab23ca2e25604af290) to this first system of the old git can works with current compilers (GCC and Make). 

**This repository it's just for those curious about trying out this old version of Git.**

*real "README" file created for ```Linus``` since 2005, was the [README](./README) file in this same repository.*

## Small chances was maked:
**Files added:** 
- ```.gitignore``` just to ignore the files than compiler create and some posibles editor or systems temp files.
- ```README.md``` this file, is just to clarify than the code isn't mine, and the changes was maked.

**Code changes:**
- Add the ```extern``` instruction for some structs of ```cache.h``` file.
- Add libs and compiler compatibility options to ```Makefile``` file.
- Comment lines than clean text of env things than cause the ```commit-tree``` command throw a *Segmention fault* error. (This is just a fast patch, probably after could be correct resolved)

## How to use:
```sh
git clone https://github.com/ojitoss/git-2005-small-fixed && cd git-2005-small-fixed && make
```

In this point yo can move the executable files to your bin folder to can execute globaly, but also was added (and ignored by .gitignore) the ```test``` folder if you don't want hace it globaly because only want test it a bit. (of curse, remember to use the relative path if use the ```test``` folder, like: ```./../init-db```)

## Commands guide:
Since the original version of git, the README don't had info about how use it (was only internals and desing to the future VCS/SMC), this is the guide of how use every command, considerations and some curiosities against current git.

### init-db
#### Why is this command?
This command is the ancestor of ```git init``` command, had the same fuction of just make hidden folder with the info of the repo and objects.

### Usage
```sh
./init-db
```
No had any parameters

### Considerations and curiosities
1. The folder if this version, no had ```.git``` name, instead was called as ```.dircache```
2. This only create the ```objects``` folder and ```index``` file.
3. The ```objects``` folder create the 255 folders inside when executed the command (names in hexa of groups of four bits like 00..ff), instead like current git, than make this folders as lazy when hash objects was generated. *I guess this is just because Linus want a funcional MVP and not add lazy instructions in the other commands*

### update-cache
#### Why is this command?
This command is the ancestor of ```git add <file>``` command, had the same fuction of register the file in the staging area (```.dircache/index```) file.

### Usage
```sh
./update-cache <file>
```
- ```file:``` This argument was to (based in your current directory) add a specific file to staging area.

### Considerations and curiosities
1. Can't use '.' in ```file``` arg to save every file in the directory to the staging area.
2. Only can be pasa one file per file, not like now can pass many files or folders as you need.
3. Can't pass folders.

### show-diff
#### Why is this command?
This command is the ancestor of ```git status``` command, had the same fuction of show in console the current state of staging area (```.dircache/index```).

### Usage
```sh
./show-diff
```
No had any parameters

### Considerations and curiosities
1. The register was displayed with this shape:
```[file name]: [state]```
  - ```file name```: was the name of the root of FILE (not per folder).
  - ```state```: this could be had two values:
    - ```Ok```: this means the file was correclty added and prepare to commit write a tree.
    - ```[rare bytes sequence]```: this meaning the file was not added to the staging area.
2. Don't use this command if you don't add a file with ```update-cache``` command before, because this corrupt the index and when re execute this command or update command, throw an error.

### write-tree
#### Why is this command?
This command is the ancestor of ```git commit``` in normal use, but as exact ancestro, is of ```git write-tree``` command, had the same fuction of write in ```.dircache/objects/``` a object of **tree** type.

### Usage
```sh
./write-tree
```
No had any parameters

### Considerations and curiosities
1. This not create a commit itself, this only create and "return" they tree hash, to create a commit you could be use after this, the ```commit-tree``` command.
2. This based to the fs than staging area was based, so you can't make like now, creating a virtual index and orphan object.