---
title: "Emacs packages"
subtitle: "How the pieces fit together"
slug: "emacs-packages"
date: 2024-08-02T21:38:00-04:00
tags: ['emacs']
---

Now that [Emacs
29](https://www.masteringemacs.org/article/whats-new-in-emacs-29-1)
moved so many useful things into Emacs proper, it's time to declare
config bankruptcy and start over, with a newer, cleaner `init.el`.

So, I need to understand how Emacs packages work.

Like, what does
[use-package](https://www.gnu.org/software/emacs/manual/html_node/use-package/index.html)
do? What is [Elpaca](https://github.com/progfolio/elpaca) for? And is
it worth straying from the vanilla path?

Here's what I've put together so far...

---

## Packages

Emacs packages serve roughly the same function as packages in other
languages. The organized package repositories are named some variant
of "ELPA" ("Emacs Lisp Package Archive")[^cxan].

[^cxan]: The Comprehensive Perl Archive Network
    ([CPAN](https://www.cpan.org)) is probably the most well-known
    "Package Archive" package system, and was inspired by TeX's
    [CTAN](https://ctan.org/?lang=en).

<aside>

### Aside: A recurring theme: startup time

Several aspects of the package system appear to have been motivated by
wanting Emacs to start more quickly. Until Emacs 26, Elisp was
single-threaded[^threading], and until Emacs 28, it was only interpreted.

[^threading]: Multi-threading: it's not clear whether you should use
    it (yet?). See Troy Hinckley's "[A vision of a multi-threaded
    Emacs](https://coredumped.dev/2022/05/19/a-vision-of-a-multi-threaded-emacs/)",
    and Tom Tromey's [Reddit
    comment](https://www.reddit.com/r/emacs/comments/utzxir/comment/i9diqxr/),
    both from May, 2022.

The most striking example is the way `use-package` loads packages only
on use, rather than on startup.

</aside>

## Step one: installing packages manually

In the beginning, packages were installed manually. Xah Lee has [a
guide](http://xahlee.info/emacs/emacs/emacs_installing_packages.html).

## Step two: use a package manager

As near as I can figure, in 2007, Tom Tromey created `package.el` and
ELPA. [Both](https://tromey.com/blog/?p=342) are
[mentioned](https://tromey.com/blog/?p=345) in blog posts dating to
April of 2007.

In version 24, Emacs started shipping with `package.el` included.
`M-x list-packages` will bring up the package manager, and let you
browse and install.

### Package archives

There are several flavors of ELPA:

GNU ELPA
: [elpa.gnu.org](https://elpa.gnu.org) - requires copyright
  assignment. Essentially, packages here are considered part of Emacs,
  but not distributed by default. 443 packages.

NonGNU ELPA
: [https://elpa.nongnu.org](elpa.nongnu.org) - requires
  GPLv3-compatible code and GNU FDL v1.4-compatible documentation. 235 packages.

MELPA
: [melpa.org](https://melpa.org) - anything goes. Just open a
  PR to add a package. 5,796 packages.

`package.el` defaults to showing
packages from GNU ELPA and NonGNU ELPA.

## Step three: more powerful package managers

From there, a variety of different package managers exist, with
varying capabilities and aras of focus. (Get more control over package
sources? Install from git? Help you _develop_ packages?) The
`straight.el` [comparison to other package
managers](https://github.com/radian-software/straight.el?tab=readme-ov-file#comparison-to-other-package-managers)
docs and following [TL;DR
section](https://github.com/radian-software/straight.el?tab=readme-ov-file#tldr-1)
give an excellent overview.

## Step four: `use-package`

`use-package`, created by John Wiegley in 2012, offers concise, tidy
syntax for using packages. It seems to have won, and was merged into
Emacs in 2022, for Emacs 29.

A `use-package` call lets you easily register a package, installing if
necessary. In that same single call you can also:

* Configure the package
* Set up keybindings or entire keymaps
* Map file types to modes
* Add hooks
* more...

Modern package managers integrate with `use-package`, replacing its
default installation mechanism.

More information on `use-package`:

* [User Manual (info)](https://www.gnu.org/software/emacs/manual/html_node/use-package/index.html)
* [github README](https://github.com/jwiegley/use-package#readme)
* [Mastering Emacs
  article](https://www.masteringemacs.org/article/spotlight-use-package-a-declarative-configuration-tool)

## Step five: package managers so powerful, it's getting silly

If you search for modern advice on which package manager to use,
you'll see a lot of articles on using
[`straight.el`](https://github.com/radian-software/straight.el). But,
the current maintainer of `straight.el` then went on to write
[Elpaca](https://github.com/progfolio/elpaca). Their [reddit
comparison](https://www.reddit.com/r/emacs/comments/19bs8w9/comment/kiu27mo/)
is useful. I believe Elpaca is the current state of the art. It can
install packages asynchronously, in parallel![^parallel]

[^parallel]: Installing packages asynchronously, in parallel, is neat,
    although you don't really _install_ packages that often... 🤷‍♂️

## Final comments

I'm currently using Elpaca, although since I don't do anything fancy,
I should probably just use `package.el`. Elpaca's async loading
notifications look cool...

I asked emacs stackexchange to "Explain Elpaca Like I'm 5" and the
[reponses](https://emacs.stackexchange.com/questions/81730) provided
interesting perspectives and a good starting point.

---

That's it! If you see a mistake, or think something should be added,
clarified, or corrected, please let me know. I'd like this post to be
the thing I wish I'd found at the
start. [zellyn.com/about#contact](http://zellyn.com/about#contact)
