# RooBarbMod
All of the RooBarb  (sub)Modules in a monorepo format

This repo just provides a convenient monorepo for checking out the RooBarb collection of tools. It provides a single Sconstruct file for building all of them individually or as a single static lib and isntalling to a PREFIX location.


## Get the codez:
```sh
git clone https://github.com/jdbrice/RooBarbMod.git
cd RooBarbMod

git submodule update --init --recursive
```

## Build (requires [scons](https://scons.org/))

by default it builds and installs.
the default `PREFIX` is the working directory.
```sh
scons -j 8
```

if you want to install to a different `PREFIX` then :
```sh
scons -j 8 prefix="/some/path/"
```

this will build the static library for each module which can be used individually, and another "libRooBarbComplete" that can be used to easily use all moduals. The headers are installed to `PREFIX/include/<MODULE>/` while the libs are installed to `PREFIX/lib/`

So if you want to include `XmlConfig.h` do this (assuming you added `PREFIX/include` to `CPPPATH`:
```c++
#include "XmlConfig/XmlConfig.h"
```

For details about the usage of each Module, look at the repo for that module.

NOTE: if you use other software written by me that depends on this, the build files generally expect an evironment variable `JDB_LIB` which points to PREFIX. Probably should choose a better name... 
