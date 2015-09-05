# splice

`splice` is tool which eases the task of maintaining a built-from-source local repository of software on unix-like systems.  The typical use-case would be creating a `/usr/local`-style hierarchy of symlinks from an `/opt`-style directory tree.

`splice` is a replacement for [graft](http://peters.gormand.com.au/Home/tools/graft/graft-html).

`splice` is open-source software released under the MIT license (previously under the GNU GPL version 2).

`splice` is written in Python.

# Usage

`splice` consists of three utilities: `splice`, `unsplice`, and `prune`.

## splice

Example: installing foo-1.0:

```
wget -O - http://website.domain.tld/software/foo-1.0.tar.gz | gunzip | tar x
cd foo-1.0
./configure --prefix=~/opt/foo-1.0
make
make install
splice ~/opt/foo-1.0 ~/local
```

## unsplice

Example: upgrading to foo-2.0:

```
wget -O - http://website.domain.tld/software/foo-2.0.tar.gz | gunzip | tar x
cd foo-2.0
./configure --prefix=~/opt/foo-2.0
make
make install
unsplice ~/opt/foo-1.0 ~/local
splice ~/opt/foo-2.0 ~/local
```

## prune

If foo-1.0 had been installed directly into `~/local` instead of `~/opt/foo-1.0`, you would need to prune the old files out of the way first:

```
wget -O - http://website.domain.tld/software/foo-2.0.tar.gz | gunzip | tar x
cd foo-2.0
./configure --prefix=~/opt/foo-2.0
make
make install
prune ~/opt/foo-2.0 ~/local
splice ~/opt/foo-2.0 ~/local
```

This would create a directory `~/local/.pruned/home_<user>_opt_foo-2.0~<datestring>~<uniquestring>` which would contain all of the files which would have conflicted with foo-2.0.

This arrangement makes it easy to "undo" the prune later if needed (just rsync the `.pruned` directory back into its original location).
