# hello-deb
Mini-tutorial on debian packaging for Golang applications.

## Credit / Thank You / Background

Before moving onto this mini-tutorial, I would like to thanks the contributors over at the [git-lfs](https://github.com/git-lfs/git-lfs) project for there work on packaging there Go code to the `.deb` packaging format. While I have made / documented this repo on my own I have incuded a couple of "Golden Nugget" comments from there repo into my own. Specifically the `debian/rules` files where most of the work is handled.

I wanted to package my Go applications in both `.deb` and `.rpm` formats however it was very hard to find tutorials for packaging Go apps in `.deb` format. Thankfully with the power of open source and the `git-lfs` project (And some internet searching) I was able to find bits and pieces to package Go apps to various CPU models under the `.deb` format.

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
_I am using a Vagrant VM (Ubuntu 17.10) to do this work, I highly advise you do the same (Just not your main workstation's OS.)_

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

4. Review `debian/rules`
```
$> vi hello-deb/debian/rules
```

5. Add `armhf` in `dpkg`

__Note__: This is pretty important as it makes 

```
$> sudo dpkg --add-architecture armhf
```

6. Build the packages. (Run `dpkg-buildpackage` using `sudo`)

```
$> cd hello-deb && sudo dpkg-buildpackage -us -uc -b --host-arch armhf -d
```

7. Pray it builds.

8. If you don't see any `error` messages then that means the `.deb` was made, you should see the package outside of the `hello-deb` directory with a couple of files. One of which being `hell-deb_1.0.0_armhf.deb`


## Caveats
