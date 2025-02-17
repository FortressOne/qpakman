QPakMan 0.66 README
===================

by Andrew Apted.  October 2008


INTRODUCTION
------------

QPakMan is a command-line (text only) program for managing the
PAK and WAD files used by the games Quake, Quake II and Hexen II.


COMPILE
=======

```
cmake .
make
```


COMMON TASKS
============

1. Listing the contents of PAK and WAD files:

   qpakman -list pak1.pak
   qpakman -list gfx.wad

Shows all the lumps contained in the PAK or WAD file.  The
first big number (prefixed by +) is the offset of the lump
(in hexadecimal) and the second number is the lump size.

In the WAD format there is a type letter for each lump,
shown after the size and before the name.  Here is a list
of all the types (only 'M' and 'P' are common) :

      M  :  Miptex texture
      P  :  PIC image
      x  :  unknown / invalid

      C  :  Color palette
      L  :  Label
      S  :  Sound
      T  :  QTex


2. Unpacking a PAK file:

   qpakman -extract pak0.pak

The entire contents of the PAK file are extracted into the
current folder.  If a file already exists, it will NOT be
overwritten (unless the -force option is given).

Certain files (such as Quake LMP files, Quake II WAL files)
will be converted to PNG image format during unpack.  This
can be suppressed with the -raw option.


3. Creating a PAK file:

   qpakman FILES.... -o dest.pak

All the specified files are stored in a newly created PAK file.
Folders can be given and their contents are recursively added
to the PAK.

Certain files will be converted from PNG format to a custom
format during the packing process.  In particular, files in
the "gfx/" folder are converted to LMP (Quake and Hexen II
only), and files in the "textures/" folder are converted to
WAL format (Quake II only).  The -raw option prevents this.

[[TODO: palette.lmp & fontsize.lmp]]


4. Unpacking a WAD file:

   qpakman -extract textures.wad

The whole contents of the WAD file are extracted as PNG images
into the current folder.  If a file already exists, it will NOT
be overwritten (unless the -force option is present).

Certain Quake texture names begin with a symbol which is not
suitable in filenames, e.g. "*lava" or "+2shoot".  Therefore
QPakMan will convert them to a prefix (four letters and '_')
while unpacking, and reconvert to a symbol while packing, as
per the following table:

      *   star_
      +   plus_
      -   minu_
      /   divd_

There are also textures which contain "full bright" pixels,
palette colors in the range 224-255 which are never drawn
darker (e.g. in shadows).  To support this feature, QPakMan
saves textures containing full bright pixels with a "_fbr"
suffix (for example "tlight03_fbr.png"), and will only use
full bright pixels while packing if that suffix is present.


5. Creating a textures WAD:

   qpakman IMAGES... -o textures.wad

All the specified images are converted to MIPTEX (Quake's
texture format) and stored in a newly created WAD file.
Currently the input images must be in PNG format.

See the notes above about filename conventions.


6. Creating the GFX.WAD file:

   qpakman IMAGES... -o GFX.WAD

This stores the images as PIC format in a newly created
file called "GFX.WAD" (upper or lower case does not matter).
If you want to use a different output name, the -pic option
must also be given to enable conversion to PIC format.

Conversion to PIC format uses all colors in the palette
(no special treatment of full bright pixels).


7. Making a texture wad for editing:

   qpakman -maketex *.pak -o textures.wad

This creates a texture WAD which a quake editor (like Quark
or GTKradiant) needs to work properly.  You need to specify
the PAK files for the full game (Quake I or Hexen II).  It
can also process individual BSP files.


COMMON OPTIONS
==============

Options have a short form (like -g) and a long form (-game),
it doesn't matter which one you use.  Some options take a
value or keyword after them, which must be separated from the
option by a space (like: -game hexen2).


-g -game  XXX    This specifies which game the PAK or WAD
                 is for.  It sets the default palette and
                 might affect how lumps are converted.

                 Usable keywords are:
                    q1   quake1   h2   hexen2
                    q2   quake2   hak  haktoria

-c -colors  XXX  Lets you load a different palette than
                 the default one (even when -game is used).

                 Must be followed by a filename for the
                 palette.  It can be either raw format
                 (768 bytes) or text format (768 numbers).

-f -force        Forces extracted files to overwrite any
                 existing file with the same name.  Normally
                 QPakMan will not overwrite existing files
                 when extracting from a PAK or WAD.

-r -raw          Prevents conversion of lumps (especially
                 from/to PNG images) while packing or
                 unpacking a PAK file.

-p -pic          When creating a WAD, stores the images in
                 Quake PIC format instead of MIPTEX format.

                 NOTE: this option is automatically enabled
                 when the output file is called "gfx.wad"
                 (or "GFX.WAD", case is not significant).

                 NOTE 2: the CONCHARS and TINYFONT lumps
                 are special, and are always created as a
                 raw blocks of pixels.


LESS COMMON TASKS
=================

1. Creating an inverse palette lump for Hexen II:

   qpakman -game h2 -o invpal.lmp


2. Creating an inverse palette lump for Quake II:

   qpakman -game q2 -o 16to8.dat


3. Making the tint tables for Hexen II:

   qpakman -game h2 -o tinttab.lmp
   qpakman -game h2 -o tinttab2.lmp


COPYRIGHT and LICENSE
---------------------

Copyright (C) 2008  Andrew Apted

QPakMan is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published
by the Free Software Foundation; either version 2 of the License,
or (at your option) any later version.

QPakMan is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.


CONTACT DETAILS
---------------

Email: <ajapted@gmail.com>

Forum: http://openarena.ws/board/index.php?topic=1710.0

