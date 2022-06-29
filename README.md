# ZOO Archiver 2.10 for macOS

## In brief
This repository contains patched source codes for ZOO Archiver 2.10 with simple updates from me to make it properly compile under macOS.
I've used original files downloaded from [Debian Packages](https://packages.debian.org/source/stretch/zoo).
I've decided to leave files (archives and those extracted) in this repository for informational purposes (for all those who want to check codes before use).
I've made a commit after applying each patch to make changes easily visible.

## Why to do this?
I decided to make it because I like old archiving software and really missed builds of `zoo` and `fiz` for modern macOS.
A year ago I managed to make it work, but had no time to make such repository with ready-made solution (for compilation, no executables here).
Now I decided to return to this really simple project and prepare it using original sources combined with great patches created by Debian maintainers.
But most importantly - for all who just want to use `zoo` and `fiz` on macOS but don't know how to use `patch` or don't know why after using it this still doesn't compile without errors.

## What's changed?
First of all, after extracting archives containing original source codes and Debian-specific files (with patches) I did patching. This made huge changes to most of source codes (if not all of them).
I then made some simple changes to Makefile so that hard-coded paths would not be used (files under them simply don't exists under macOS). It required me to create some additional files with simple file mappings (see `additional` directory inside `zoo-2.10`).
As it continued to not compile, I had to make simple change in `ar.h` file: I've added `#define` statement to always use code from first `#ifdef`.
Later, I've made simple update of Makefile to make it show macOS target. It is really simple update, because none of the compiler directives were changed (You can see -DLINUX, for example).
Finally, I've changed directory name from `zoo-2.10.orig` to just `zoo-2.10` as it no longer contain original source codes.

## Does it compile?
As far as I was able to test it - yes. At least under my installations of macOS and Linux (respectively: Monterey under Intel i5 and Mint).
However, I encountered problems under my Windows installation using MinGW. It might be because of my specific installation of developer tools, which may collide with `make` - tests and further discussion welcome.

## Where is the main source code?
In `zoo-2.10` directory. Other directories contain original archives, descriptions and Debian-specific files with patches.

## I use Linux or want to test this code under Windows, but can't see those targets. What to do?
As I made really simple updates to Makefile, using `mac` or `mac64` target should work fine. At least under Linux, as I've mentioned before.
I have not been able to test it properly under Windows, so I'd be happy to see Your tests and talk about them, if time permits.

However,
- if You use Linux, then consider using official package, which can be found [here](https://packages.debian.org/stretch/zoo) or in Your package manager
- if You use Windows supporting 16-bit programs (for example 32-bit Windows XP), You can find old MS-DOS distributions on the internet which works pretty good (sorry, I can't remember where, because I searched for them long time ago) 

It is far better to use officially supported versions rather than using codes provided here.

## Known problems (to me)
1. `Stack overflow in lzd()` on some data

Well, first of all, You have to know that this software is somehow old now. It was developed while much smaller amount of data were ever needed and typical storage media had far less than 1MB of space.
You can't expect reliability while using huge files. However, it's possible to use this software even with gigabyte files as long as they do not contain similar blocks of data which may result in creating block on stack with size greater than 8MB (as far, as I've been able to notice it).
As it is difficult to know if Your data contain such blocks, the best You can do, if You really need or want to use this software with big files, is to test any archive created using `zoo` - PLEASE DO THIS IN THAT CASE, BECAUSE OTHERWISE YOU MAY LOSS YOUR DATA!

- You may do this easily by just testing the archive's integrity: `zoo -test archive.zoo`
- Or just by extracting the files from it: `zoo x archive.zoo`; remember to do this in a different directory than your files

2. Using `*` or `.` wildcards with adding files does not add directories or just do nothing

Yup, it's quite uncomfortable, I think. You have to add directories manually. I think it's due to make path traversal attacks harder or even impossible.
Combining this with some script making list of files You wish to add to the archive would be great solution. As far as I know, it is possible to provide as many file as You want in command prompt.
If I'm not right, You may also add files partially to make full archive.

## Disclaimer
I did my best to test these codes and provide usable software without errors with hope it will be useful for somebody (for example, for historical or sentimental reasons).
However, I can't guarantee that everything will work good. At least, I can't assume that the software would compile on Your system, as I only tested it on those two mentioned before.
BY USING ANY PART OF CODES PROVIDED HERE, YOU AGREE THAT YOU ARE DOING IT ON YOUR OWN RISK, AS SOFTWARE IS PROVIDED HERE "AS IS" WITHOUT ANY GUARANTEE. I TAKE NO RESPONSIBILITY FOR ANY DAMAGE CAUSED BY USE OF THESE SOURCE CODES.

## Other notes
In fact, I've made all mentioned updates just for fun and for having ability to use ZOO Archiver on my Mac Mini.
I hope that providing these codes here does not violate any laws or licenses.
If You were ever involved in development of this software and You don't wish presence of any of the files here - please, contact me.
My intention was to provide codes prepared to compile for modern macOS and provide them for free for all interested with full respect to original authors of code, patches and other files.

## Licence
As far as I know, public-domain. If I'm wrong, please correct me. Thank You.
I've made my corrections for free, just for fun and for everybody wanting to use this software today on macOS.

## Copyright
The software was originally written by Rahul Dhesi between 1986 and 1991 year. Later, it was corrected by Debian maintainers.
I'd love to provide copyright list here (found in Debian-specific files) to respect those people and express my gratitude for making this software work - I really appreciate and like it.

Author of original software:

&copy; 1986-1991 Rahul Dhesi

Later maintainers:

&copy; 1997-1998 James Troup

&copy; 1998, 2000, 2001, 2004 Petr Cech

&copy; 2004 Niklas Vainio

&copy; 2005-2008 Jose Carlos Medeiros

&copy; 2011-2012 Jari Aalto

Me, doing simple corrections to make it compile under macOS:

&copy; 2021-2022 Bartłomiej "Magnetic-Fox" Węgrzyn
