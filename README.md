# Git first version (un official) repo

*DISCLAIMER: The original repository of Linus Torvalds, don't contain a LICENSE file, so i can't put a LICENSE, bit was Open source anyway and i add this to avoid any kind of "license troll"*

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
1. [init-db](#init-db)
2. [update-cache](#update-cache)
3. [show-diff](#show-diff)
4. [write-tree](#write-tree)
5. [commit-tree](#commit-tree)
6. [cat-file](#cat-file)
7. [read-tree](#read-tree)

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
- ```file```: This argument was to (based in your current directory) add a specific file to staging area.

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
2. Don't use this command if you don't add a file with [update-cache](#update-cache) command before, because this corrupt the index and when re execute this command or update command, throw an error.

### write-tree
#### Why is this command?
This command is the ancestor of ```git commit``` in normal use, but as exact ancestro, is of ```git write-tree``` command, had the same fuction of write in ```.dircache/objects/``` a object of **tree** type.

### Usage
```sh
./write-tree
```
No had any parameters

### Considerations and curiosities
1. This not create a commit itself, this only create and "return" they tree hash, to create a commit you could be use after this, the [commit-tree](#commit-tree) command.
2. This based to the fs than staging area was based, so you can't make like now, creating a virtual index and orphan object.

### commit-tree
#### Why is this command?
This command is the ancestor of ```git commit``` command, had the same fuction of create a object of **commit** type.

### Usage
```sh
echo <message> | ./commit-tree <tree-hash> [-p <tree-hash>]
```
- ```message```: was not necesary itself to use the pipe, but without this, the commit object had a empty description.
- ```tree-hash```: this arg is used to set the tree object to the commit was target about.
- ```parent```: this flag target and add to the commit a parent prop.
  - ```tree-hash```: this sub-arg was to indicate was hash was a parent of this commit.

### Considerations and curiosities
1. You need to set a global env or shell variables before use this command:
```sh
set GIT_AUTHOR=<name>
set GIT_EMAIL=<email>
```
2. Of follow a current "block-chain" than git has with commits, you may a put **manualy** the parent prop in every commit of the last commit hash.
3. A commit can target of any **tree** object and not only last tree created with [write-tree](#write-tree)

### cat-file
#### Why is this command?
This command is the ancestor of ```git cat-file``` command, had the same fuction of display a content of a object storage in ```.dircache/objects/```.

### Usage
```sh
./cat-file <object-hash>
```
- ```object-hash```: this arg you pass the commit of a EXISTING object in your dircache.

### Considerations and curiosities
1. Against current cat-file command, not throw content directly, the original create a file with this structure as name: ```temp_git_file_XXXXXX``` and you be see it with a ```cat``` command or variants of file readers.
2. This files was never cleaned for git, you could be manualy, remove it, preference use: ```rm -rf temp_git_file_*``` to remove all with one command.

### read-tree
#### Why is this command?
This command is the ancestor of ```git ls-tree``` command, had the same fuction of display a structure of, name, permiss, and blob hash of the tree fields.

### Usage
```sh
./read-tree <tree-hash>
```
- ```tree-hash```: this arg refer to the hash of the tree you can check.

### Considerations and curiosities
1. Instead of [cat-file](#cat-file) command, this command show in console directly.
2. the structure of how show it is:
```[file permiss] [name file and root] ([blob hash])```

## Internals
The [README](./README) file of Linus Torvalds, was focused in the desing itself of the internals, not mention or focuss in practic uses of the internals, so this is the practice uses about it.

### Folder DB structure
```
.dircache/                   # Folder db name
  index                      # Staging area
  objects/                   # Folder of than contain in binary files (compress with zlib)  
    [two chars in hexa]/     # The first byte or two firts chars, create a 255 folders
       [38 other hexa chars] # The rest of chars to form one file (loose object)
```