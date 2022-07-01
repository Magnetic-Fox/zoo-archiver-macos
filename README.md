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

## What's **NOT** changed?
Well, I haven't changed `install` procedures in Makefile, so using them may not work correctly under macOS (haven't tried this, so keep in mind, that the original Makefile was designed to be used under Linux, not macOS - there may be some key differences).
I decided not to change these procedures, because the main goal of my work here was to provide as little changes as it was needed to make it compile under macOS.
Shortly: use this Makefile CAREFULLY! In fact, the best You can use it for is to just compile this software. I recommend making further operations manually or by writting simple scripts to do so.

## Does it compile?
As far as I was able to test it - yes. At least under my installations of macOS and Linux (respectively: Monterey under Intel i5 and Mint).
However, I encountered problems under my Windows installation using MinGW. It might be because of my specific installation of developer tools, which may collide with `make` - tests and further discussion welcome.

## Where is the main source code?
In `zoo-2.10` directory. Other directories contain original archives, descriptions and Debian-specific files with patches.

## How to compile it?
Well, it should be really simple. However, the first question should be "what do I need to compile it?".
As far as I know and as this is really old software, You only need good C compiler (`gcc` will be great) and good configured and working `make` processor - shortly, some UNIX-like developer tools.

The recipe is quite short:
1. Open your terminal
2. Go to the directory with sources and a Makefile (go to `zoo-2.10`)
3. Use Makefile to do the rest (so, for example, type `make mac64` and hit enter) - this should start compilation process, which should take a while

And that's all. You should encounter no problems during compilation except some warnings which (let's say) can be safely ignored.
After a while `zoo` and `fiz` binaries should arrive in the directory and be ready to use.

4. (optional operation after compiling) You may create symbolic links or move compiled software to Your `/usr/bin`, etc.

## Which target to use - `mac` or `mac64`?
Great question. If I understand it correctly, the key difference between those targets is just size of some integers used in code. As far as I was able to test it (using different compilations on many Linux distributions), this affects the most the maximum size of file which can be used with this software.
It should not be a problem while using small files (meaning sizes adequate to the time these programs were created) or at least not a gigabyte ones. However, please note, that problems may arise otherwise. Once, a year ago, I had a situation in which I packed huge file under Linux Mint with official 64-bit integer compilation of `zoo` and then had problems unpacking it under another Linux distribution which probably had 32-bit integer version. Can't remember what kind of error message I got back then and what size had that file.
I think that nowadays, the `mac64` target should be used to provide a substitute of big files support (PLEASE READ THE **KNOWN PROBLEMS** SECTION!), but keep in mind that creating really huge archive may render it incompatible (at least, partly) with older versions/compilations (probably more common) of this software. It's really old stuff and should be used like that with all these historical background. 

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
If I'm wrong, You can always add files partially to make whole archive.

3. Troubles exchanging huge files in archives between 32-bit and 64-bit integer versions

See **WHICH TARGET TO USE** section, as it was shortly described there.

## Disclaimer
I did my best to test these codes and provide usable software without errors with hope it will be useful for somebody (for example, for historical or sentimental reasons).
However, I can't guarantee that everything will work good. At least, I can't assume that the software would compile on Your system, as I only tested it on those two mentioned before.
BY USING ANY PART OF CODES PROVIDED HERE, YOU AGREE THAT YOU ARE DOING IT ON YOUR OWN RISK, AS SOFTWARE IS PROVIDED HERE "AS IS" WITHOUT ANY GUARANTEE. I TAKE NO RESPONSIBILITY FOR ANY DAMAGE CAUSED BY USE OF THESE SOURCE CODES.

## Other notes
As far as I would love to do it, I'm not a maintainer of this software nor a Debian maintainer of its package, because I simply don't have time for it.
I can't exclude that if I have some spare time I'll make some minor fixes to resolve problems mentioned before, but it's more likely that I'll not.
You have to know that, in fact, I've made all those simple updates just for fun and for having ability to use ZOO Archiver on my Mac Mini.

Speaking of a code and all files shared here: I hope that providing these all here does not violate any laws or licenses.
If You were ever involved in development of this software and don't wish presence of any of the files here - please, contact me.
My only intention was to provide codes prepared to compile for modern macOS and provide them for free for all interested with full respect to original authors of code, patches and other files.

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
