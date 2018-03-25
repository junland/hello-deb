# hello-deb
Mini-tutorial on debian packaging for Golang applications.

## Credit / Thank You / Background

Before moving onto this mini-tutorial, I would like to give a HUGE shoutout to the contributors over at the [git-lfs](https://github.com/git-lfs/git-lfs) project for there work on packaging there Go code to the `.deb` packaging format. While I have made / documented this repo on my own I have incuded a couple of "Golden Nugget" comments from there repo into my own. Specifically the `debian/rules` files where most of the work is handled.

I wanted to package my Go applications in both `.deb` and `.rpm` formats however it was very hard to find tutorials for packaging Go apps in the `.deb` format. Thankfully with the power of open source and the `git-lfs` project (And some internet searching) I was able to find bits and pieces to package Go apps to various CPU models under the `.deb` format.

## Some reading

[Debian Policy Manual](https://www.debian.org/doc/debian-policy/)

[Debian FAQ](https://www.debian.org/doc/manuals/debian-faq/ch-pkg_basics.en.html)

## A small primer on Debian packaging.

To package your application, your repo must have a `debian` directory in it or during your CI/CD process (Travis, CircleCI, Jenkins, etc.) in that directory you MUST (E.g required) have 4 files:

`control` - Plain text, used to describe the package.

`copyright` - Plain text, used to place source code license info.

`changelog` - Plain text (However needs special format described in [here](https://www.debian.org/doc/debian-policy/#document-ch-source), I would highly read this since the `changelog` file is very sensitive and if you do it wrong you will get lots of warnings in your build.), used to describe the changes made to your code.

`rules` - Makefile, used to build the package.

For more information please visit this [section](https://www.debian.org/doc/manuals/maint-guide/dreq.en.html#copyright) in the Debian maintaniers guide.

For this repo, I also have `compat` which just tell's the build tool to use a certain version for building packages. (Just helps with avoiding some warnings on the output.)

## Getting started
_I am using a Vagrant VM (Ubuntu 17.10), I highly advise you do the same (Just not your main workstation's OS.)_

1. Install Go language + some packages for dev work. (1.8.x from the official Ubuntu repos)

```
$> sudo apt install golang-go git
```

2. Install native dependencies.

```
$> sudo apt install dh-make devscripts dh-make-golang dh-golang build-essential fakeroot
```

3. Clone repo.

```
$> git clone https://github.com/junland/hello-deb.git
```

4. Review `debian/rules`, you don't need to really edit it, but it's good to understand what's going on.
```
$> vi hello-deb/debian/rules
```

5. Build the packages. (Run `dpkg-buildpackage` using `sudo`)

```
$> cd hello-deb && sudo dpkg-buildpackage -a armhf -us -uc -b 
```

6. Pray it builds.

7. If you don't see any `error` messages then that means the `.deb` was made, you should see the package outside of the `hello-deb` repo directory with a couple of files. One of which being `hello-deb_1.0.0_armhf.deb`

Now that the package has been built under `armhf` you can do the same process for `i386`, `amd64`, and `arm64` by substituating the paramter for the `-a` flag for `dpkg-buildpackage` (Step 5) by replacing it with the arch you want.

For example, if I wanted to build for `i386`, I would issue this command inside my repo:
```
$> sudo dpkg-buildpackage -a i386 -us -uc -b 
```

That's pretty much it, with that knowledge you can now build your `.deb` packages for various architectures. 
 
## Caveats

* This tutorial won't give you 100% compliance to upload your own packages to the Debian repos as that requires some extra work and your depedencies have to be packaged seperatly in a `.deb` format (It sounds stupid, but there is some method to the madness if you read the Debian Policy Manual). But this tutorial will give you a basic understanding of how to packge your software under the `.deb` format and package it for various architectures other than your host architecture.

* There is still a lot of stuff that's not here, like how to add post / pre installation and removal steps, which are just scripts placed inside the `debian` directory. Also this tutorial does not contain anything on how to add config files or other dependinces such as man pages, images, init scripts, etc. I would advise you search online (Or view the Debian Policy Manual) as it's pretty easy to find info on this.

* This project / tutorial is not using `go get` to pull down dependencies (As much as I love and default to this tool a lot), instead it's using a `vendor` directory to house dependinces. I'd also like to preface lots of Go projects are using this approach and also there has been signifigant work with `vgo` spear headed by the Golang team. Also it seems that you would have to hack your way into the `rules` files to get `go get` working properly. (I've tried, but don't have much patience to hack it.)

