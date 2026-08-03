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