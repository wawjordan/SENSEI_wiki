## Contents

- [Cloning on Linux](#cloning-on-linux)
- [Cloning on Windows](#cloning-on-windows)
- [Building SENSEI](#building-sensei)

## Obtaining the source code

SENSEI uses the Git distributed version control system ( DVCS ).
With Subversion (SVN), you 'check out' a repository.
With Git, you 'clone' your repository.
The difference is that SVN uses one central repository that everyone commits to while in Git,
each clone is it's own repository.
This means that you can commit to your local repository before pushing to the global repository, allowing you
to check your work and only push information when you're ready.

For more information about git and setting git up please search online.

### Cloning on Linux

To clone SENSEI, open a terminal and run the following:

```js
git clone https://github.com/cjroy/SENSEI.git
```

This will create a directory called SENSEI within your 'path-to-local-repository'.
If you leave <path-to-local-repository> blank, Git will create the SENSEI directory in your current working directory.

### Cloning on Windows

If you try to clone the repository using TortoiseGit for Windows, the default settings will screw up the line endings. To fix this, do the following:
* From the start menu, go to Git, then select Git Bash
* In the terminal, type the following:

```js
git config --global core.autocrlf input
```

## Building SENSEI
### CMake system
SENSEI now uses a CMake build system. It requires CMake version >= 3.7 (needed for submodule support).
This is installed on roycfd and should be included in your path.

You will really want to build in another folder!
There is no distclean so you can really dirty up your source directory if you build there.
See [this](https://cmake.org/Wiki/CMake_FAQ#Out-of-source_build_trees) for more details.
It is suggested to have a directory for your optimized build and a directory for the debug build.
These are typically `release` (or `opt`) and `debug` in the top level of the SENSEI directory.

There are two steps for building SENSEI with CMake.
In the build directory run the following:
```sh
cmake /path_to_SENSEI/  # the path can be relative (typically ../)
make
```

The first step is the configuration or setup step which you should only ever need to do once.
In this step you will want to set all the options that you want.
The first option to set is `-DCMAKE_BUILD_TYPE`, which chooses the type of build that will be performed.
There are currently two options `Release` and `Debug`.
Setting this option will enable the correct respective flags for gfortran (TODO: enable flags for other compilers).
If you want to enable profiling you can add -DPROFILE=ON (default is OFF).
If you want to enable more flags you can set them with the `-DCMAKE_Fortran_FLAGS_DEBUG` or `-DCMAKE_Fortran_FLAGS_RELEASE` (not sure this still works... might be `-DUSER_DEBUG_FLAGS` and `-DUSER_RELEASE_FLAGS`).
Another option that you may or may not have to set, depending on the defaults for cmake, is `-DCMAKE_Fortran_COMPILER=gfortran` (assuming you have set your path correctly so gfortran points to a version >= 5.2.0).

The final option that you may want to set is whether or not you want to have unit testing compiled.
This should be compiled with the debug build so give some extra testing capability (check array bounds, etc).
Testing can be enabled with `-DPFUNIT=/path_to_pfunit` (this must be the precompiled install directory for pfunit version >= 3.2.7).
If you are compiling with pFUnit make sure to use the same compiler used to build pFUnit.
There are a lot more options that you can set but that is up to you to figure out if you want them.
Once these are set you should not have to set them again since they are saved in `CMakeCache.txt` and you can just run `cmake ../` again if you want (but you shouldn't have to since make will do this if necessary).

Currently CMake will search the library paths for LAPACK.
This means that you will need to have LAPACK and BLAS installed on your system and accessible through your library path.
LAPACK is not installed by default on the AOE computers so there is a version compiled on roycfd.
To set your library path add the following to your `~/.bashrc`
```sh
export LD_LIBRARY_PATH=/path_to_LAPACK:$LD_LIBRARY_PATH
```

The setup step will create the Makefiles necessary to build SENSEI.
SENSEI can then be built by running `make`.
Note: cmake will enable the use of parallel compilation with dependency checks so you can perform a parallel build with `make -jN` where N is the number of threads you want to use.
If you have made changes to the content of the files (including adding a new file to CMakeLists.txt) you should be able to just recompile with `make`.
However, if you have renamed a file, moved a file, or deleted a file it is important to `make clean` before you do anything else (otherwise the old .o and .mod files will still be there to build against).
If necessary then the `make` command will re-run `cmake` for you and regenerate the makefiles.
`make` will put libraries in a lib folder inside the build directory and will put the executables in a bin folder inside the build directory.

Below is an example of a full debug build:
```sh
cd /path_to_SENSEI
mkdir debug
cd debug
cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_Fortran_COMPILER=gfortran -DPFUNIT=$PFUNIT ../
make -j16
```

Then SENSEI can be run through the command `/path_to_SENSEI/debug/bin/SENSEI`.

If you want to use MPI for SENSEI, then an example of a full release build is given below:
```sh
cd /path_to_SENSEI
mkdir release
cd release
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_Fortran_COMPILER=mpifort ../
make -j16
```

Be sure that you have included the path of OpenMPI to your library path.

## Help! I broke the build!
If something has gone wrong with the build (very uncommon) or you just want to do a clean build you can run `make clean`.
This will remove all of the build files and executables so you can do a clean build.
If something has really gone wrong you can delete all of the files and folders in your build directory (except CMakeCache.txt if you want to keep settings) and rerun `cmake ../`.
This is another reason to not build in the SENSEI directory since the full clean can be performed by deleting the build directory.

## Old build system
If attempting to run old versions of SENSEI before CMake was used it may be necessary to use the old autotools build system. For instructions on how to build with autotools see [[Old Build System]].