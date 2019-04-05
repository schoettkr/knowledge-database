+++
title = "Usage of tar"
author = ["eo shiru"]
date = 2018-10-27
lastmod = 2019-04-06T00:22:54+02:00
draft = false
+++

## Usage of tar {#usage-of-tar}

GNU's `tar` is an archiving utility to store files into an archive and manipulate such archives.


### Compressing {#compressing}

To compress files into an archive with tar use:

```sh
tar -cvzf <name of tarball archive>.tgz /filepath/or/path/to/source/folder # creates ".tgz" archive
tar -cvzf <name of tarball archive>.tar /filepath/or/path/to/source/folder # creates ".tar" archive
tar -cvzf <name of tarball archive>.tar.gz /filepath/or/path/to/source/folder # creates ".tar.gz" archive
```


### Extracting {#extracting}

To extract or decompress archives use:

```sh
tar -xvzf <name of archive>.tgz # decompresses the archive into the current directory
```


### Flags {#flags}

The flags stand for:

-   **c** &rarr; **C** ompress/create
-   **x** &rarr; e **X** tract
-   z &rarr; zee (compression as in filter through gZip, Zlib, Zip)
-   f &rarr; file
-   v &rarr; verbose


### Mnemonic {#mnemonic}

To remember this more easily:

-   `tar -czf` &rarr; "tar Create Zipped File"
-   `tar -xzf` &rarr; "tar eXtract Zipped File"
