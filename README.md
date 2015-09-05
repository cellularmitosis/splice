# splice

`splice` is tool which eases the task of maintaining a built-from-source local repository of software on unix-like systems.  The typical use-case would be creating a `/usr/local`-style hierarchy of symlinks from an `/opt`-style directory tree.

`splice` is intended as a replacement for [graft](http://peters.gormand.com.au/Home/tools/graft/graft-html).

`splice` is open-source software released under the terms of the MIT license.

`splice` is written in Python.

# Usage

`splice` consists of three utilities: `splice`, `unsplice`, and `prune`.

## splice

Example: installing foo-1.0:

```
wget -O - http://website.domain.tld/foo-1.0.tar.gz | gunzip | tar x
cd foo-1.0
./configure --prefix=~/opt/foo-1.0
make
make install
splice ~/opt/foo-1.0 ~/local
```

## unsplice

Example: upgrading to foo-2.0:

```
wget -O - http://website.domain.tld/foo-2.0.tar.gz | gunzip | tar x
cd foo-2.0
./configure --prefix=~/opt/foo-2.0
make
make install
unsplice ~/opt/foo-1.0 ~/local
splice ~/opt/foo-2.0 ~/local
```

## prune

`unsplice` will remove symlinks, but not regular files.  `prune` is a more powerful / less safe tool for removing files which have been directly installed into ~/local.  If you have been in the habit of installing software directly into `~/local`, you will need to use `prune` as you transition towards splicing installations from `~/opt` into `~/local`.

```
wget -O - http://website.domain.tld/foo-2.0.tar.gz | gunzip | tar x
cd foo-2.0
./configure --prefix=~/opt/foo-2.0
make
make install
prune ~/opt/foo-2.0 ~/local
splice ~/opt/foo-2.0 ~/local
```

This would create a directory `~/local/.pruned/home_<user>_opt_foo-2.0~<datestring>~<uniquestring>` which would contain all of the files which would have conflicted with foo-2.0.

This arrangement makes it easy to "undo" the prune later (just rsync the `.pruned` directory back into its original location).

# Latest release:

* [0.9](https://github.com/cellularmitosis/splice/releases/tag/0.9) (September 5th, 2015)

# Installation (bootstrapping):

```
cd /tmp
wget -O - https://github.com/cellularmitosis/splice/archive/0.9.tar.gz | gunzip | tar x
mkdir -p ~/opt
mv splice-0.9 ~/opt/
cd ~/opt/splice-0.9/bin
./splice ~/opt/splice-0.9 ~/local
```

Make sure `~/local/bin` is in your `$PATH`.

# Pre-github release history:

Here is the pre-github changelog:

## 0.2 (Apr 28th 2008)

Changes:
* First public release

## 0.3 (Jun 20th 2008)

Changes:
* `unsplice` no longer attempts to remove mountpoints

## 0.4 (Oct 13th 2008)

Changes:
* Explicitly use python2.5

## 0.5 (Oct 15th 2008)

Changes:
* Fixed bug in how symlinks which point to dirs are handled

## 0.6 (Nov 19th 2008)

Changes:
* Fixed another symlink-related bug.

## 0.7 (Jan 8th 2009)

Changes:
* Decided on policy of not touching symlinks to dirs in unsplice.

## 0.8 (Apr 13th 2009)

Changes:
* Fixed incorrect handling of '/' as the destination directory.

