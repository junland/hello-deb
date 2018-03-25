# hello-deb
Mini-tutorial on debian packaging for Golang applications.

## Credit / Thank You / Background

Before moving onto this mini-tutorial, I would like to thanks the contributors over at the [git-lfs](https://github.com/git-lfs/git-lfs) project for there work on packaging there Go code to the `.deb` packaging format. While I have made / documented this repo on my own I have incuded a couple of "Golden Nugget" comments from there repo into my own. Specifically the `debian/rules` files where most of the work is handled.

I wanted to package my Go applications in both `.deb` and `.rpm` formats however it was very hard to find tutorials for packaging Go apps in `.deb` format. Thankfully with the power of open source and the `git-lfs` project (And some internet searching) I was able to find bits and pieces to package Go apps to various CPU models under the `.deb` format.

## Some reading

[Debian Policy Manual](https://www.debian.org/doc/debian-policy/)

[Debian FAQ](https://www.debian.org/doc/manuals/debian-faq/ch-pkg_basics.en.html)


## Getting started

## Caveats
