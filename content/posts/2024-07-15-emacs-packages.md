---
title: "Emacs packages"
slug: "emacs-packages"
date: 2024-07-15T21:20:00-04:00
tags: ['emacs']
draft: true
---

Now that [Emacs
29](https://www.masteringemacs.org/article/whats-new-in-emacs-29-1)
moved so many useful things into Emacs proper, it's time to declare
config bankruptcy and start over, with a newer, cleaner `init.el`. So
I need to understand how Emacs packages work.

## The question

This exploration started with the question:

> What exactly is [elpaca](https://github.com/progfolio/elpaca) for,
> and does it do enough to justify straying from the
> straight-and-vanilla path?

## The long answer

Here's what I've put together so far.

### Packages

They serve more or less the same function as packages in other
languages. The package repositories are named some variant of ELPA
("Emacs Lisp Package Archive"), so I'd guess this stuff dates from
[CPAN](https://www.cpan.org) days. Or maybe
[CTAN](https://ctan.org/?lang=en).


## `use-package`

* [User Manual (info)](https://www.gnu.org/software/emacs/manual/html_node/use-package/index.html)
* [github README](https://github.com/jwiegley/use-package#readme)
* [Mastering Emacs
  article](https://www.masteringemacs.org/article/spotlight-use-package-a-declarative-configuration-tool)


## Package managers

The straight.el [comparison to other package
managers](https://github.com/radian-software/straight.el?tab=readme-ov-file#comparison-to-other-package-managers)
docs offer a good overview.

## Package archives

* GNU ELPA: [elpa.gnu.org](https://elpa.gnu.org) - requires copyright
  assignment. Essentially, packages here are considered part of Emacs,
  but not distributed by default. 443 packages.
* NonGNU ELPA: [https://elpa.nongnu.org](elpa.nongnu.org) - requires
  GPLv3-compatible code and GNU FDL v1.4-compatible documentation. 235 packages.
* MELPA: [melpa.org](https://melpa.org) - anything goes. Just open a
  PR to add a package. 5,796 packages.

## `package-initialize`, autoloads, `package-enable-at-startup`

`package-initialize` is no longer needed as of Emacs 27.2: [NEWS file](https://github.com/emacs-mirror/emacs/blob/emacs-27.2/etc/NEWS#L204-L214)

https://emacs.stackexchange.com/questions/81730/can-someone-eli5-elpaca?noredirect=1#comment137434_81730
