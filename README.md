image - track post-install file changes
=======================================

Concept
-------

I compile a ton of packages from source, and it can be a pain to try and
keep track of which file belongs to which package. This utility helps to
alleviate that condition somewhat.

The script maintains a list of directories to monitor for changes. These
can be edited if desired, but should be sufficient for most uses. By
default, these are:

* /usr/local/bin
* /usr/local/etc
* /usr/local/include
* /usr/local/lib*
* /usr/local/sbin
* /usr/local/share

This tool will snapshot these directories before and after a package
install, and then save the difference to a file named according to the
package. This image can then be used to for identifying which files
belong to which packages or ensuring that all files are deleted when a
package is removed.

Usage
-----

The build process for a source package will typically begin something like
this:

    cd /usr/local/src/package-1.2.3
    ./configure
    make

The last step is usually `make install`. Just prior to this, you'll want to
make a new baseline snapshot of the monitored directories. This is done with
the `reset` command:

    image reset

Then, install the package as normal. (This assumes you're installing to the
/usr/local directory.)

    make install

Now create a new image for this package with the `make` command, giving it
an appropriate name:

    image make package-1.2.3

You can verify that you've got an image with the `list` command:

    image list

And you can list the files that were created with the `show` command:

    image show package-1.2.3

If you later find yourself wondering what package owns the file
/usr/local/bin/something, you can run:

    image find /usr/local/bin/something
