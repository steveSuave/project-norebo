# Project Norebo

Norebo is a hack to run some _Project Oberon 2013_ software on the
Unix command line. Programs that use the GUI obviously won't work, but
e.g. the compiler runs.

I probably won't be maintaining this project, so feel free to fork
if you want to develop it further.

## Contents

* `Runtime/` RISC5 emulator and operating system interface.
* `Oberon/` Unmodified source code from Project Oberon 2013.
* `Norebo/` Norebo-specific and new modules.
* `Bootstrap/` Pre-compiled modules to bootstrap Norebo.
* `build.sh` Script to rebuild Norebo. See Norebo in action.

## PO2013 image build tools

This repository also contains tools to build fresh PO2013 filesystem
images.  Use it like so:

    ./fetch-sources.py upstream
    ./build-image.py upstream

...where `upstream` is the name of the directory where the sources
should live.  (Replace it with the name of your choice.) This will
download the project sources, compile them, create runnable disk image
`build/Oberon.dsk`.  The CSV build manifest controls which set of
files should define the resulting system.  The disk image can be run
on the [Project Oberon RISC emulator].

Supporting Oberon modules are stored in `Norebo`: a virtual file
system (`VDiskUtil`/`VFile`) and a static linker for the Inner Core.
All this is based on code from PO2013.

## File handling

New files are always created in the current directory. Old files are
first looked up in the current directory and if they are not found,
they are searched for in the path defined by the `OBERON_PATH`
environment variable. Files found via `OBERON_PATH` are always opened
read-only.

## Bugs

Probably many.

Files are not integrated with the garbage collector. If you don't
close a file, it will remain open until Norebo exits.

Most runtime errors do not print a diagnostic message. Here's a table
of exit codes:

 Exit code | Meaning
----------:|:------------------------------
      1..7 | possibly a Modules error
         5 | (also) unknown command
       101 | array index out of range
       102 | type guard failure
       103 | array or string copy overflow
       104 | access via NIL pointer
       105 | illegal procedure call
       106 | integer division by zero
       107 | assertion violated


---
This fork adds a couple of large font files in order to make the resulting Oberon image easier on the eyes.  
More fonts can be found on https://github.com/io-core/fonts
To create the multifont image after a fresh clone run:  
`$ ./build.sh`  
`$ python build-image.py multifont-source`  
Then you can run the resulting Oberon.dsk on pdewacht's risc emulator.

[Project Oberon RISC emulator]: https://github.com/pdewacht/oberon-risc-emu
