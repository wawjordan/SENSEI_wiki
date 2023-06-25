## Building SENSEI

Older versions of SENSEI used a standard GNU autotools build system

The first thing to do is in the top level of the SENSEI directory.
Run ./bootstrap to add some missing things. NOTE: if you are using PUTTY from Windows or if you have checked the code out using Windows, you may need to change the bootstrap file's permission to executable (i.e., `chmod +x bootstrap`).

Now choose/create a directory to build SENSEI in.
Normally, you will want a `debug` and `opt` directory.
Create these and cd into debug, then run the following:

```sh
<path-to-SENSEI-top-level>/configure prefix=`pwd` FC=gfortran FCFLAGS="-O1 -g -pg -fopenmp -Wall -Wextra -Wconversion-extra -fimplicit-none -fcheck=all -fbacktrace -ffpe-trap=zero,underflow,overflow,invalid"
```

NOTE: if you haven't properly set up your .bashrc file, you may need to specify the gfortran location by replacing "FC=gfortran" with "FC=/home/roycfd/Software/gcc/gcc.4.7.2/bin/gfortran"

This goes into the main source directory and runs the configure executable for your currently working directory,
in this case, the 'debug' directory.
The prefix command tells configure that if you run 'make install' later on,
SENSEI will get installed to the current working directory, in this case, debug.
FC sets the compiler as gfortran, and the FCFLAGS give all the compiler options.
Note that these are in quotes to protect the spaces from the shell.

If you want to enable and compile the unit testing for SENSEI you can add the option `--enable-pfunit=/path/to/pfunit` to the line above. It is recommended to perform the unit tests in this debug build as opposed to the optimized build to catch other unexpected errors as the corner cases are tested. NOTE: if compiling with this option must have $PFUNIT defined (see [[Getting Started]]) FIXME: should use the given path.

If you make a mistake while configuring, run `make distclean` to clean everything up.
If you accidentally configure in the top level of SENSEI, you'll end up getting errors later on along the lines of
the directory is already configured.  Go to the SENSEI top level and run 'make distclean' there to fix the problem.

Once the code has been succesfully configured, run 'make' to actually build the code.
You can run `make -jX`, where X is some number of processors to do a parallel make.
Currently, `-j4` seems to work well, but you may run into a case where the code compiles too quickly
and the build fails.
Just rerun with fewer processors.
Once this is done, you can run `make install` which should allow you to access the executable from any location.

Now cd out of debug, into opt, and run the following:

```sh
<path-to-SENSEI-top-level>/configure prefix=`pwd` FC=gfortran FCFLAGS="-O3 -fopenmp -march=core2"
```

The `-march` option sets optimizations for different processor architectures.

Use `core2` for Callisto, Europa, Titan, and Triton.

Use `corei7` for Charon.

Use `k8` for Tethys.

Now, run `make`, and then if you want, `make install` to allow you to access the executable from any location.

## Building SENSEI after updates
Sometimes extra directories, files, or macros are added to SENSEI and you will
find that `make` will no longer just work.
You need to reset some stuff in autotools to update everything so that the new
stuff will be seen by the build change.

This can be done by running `make distclean`. This will remove all of the stale build and .mod files.
Then in the top level of the SENSEI directory, run `./bootstrap` to remake the configure script to include the missing stuff. NOTE: if you are using PUTTY from Windows or if you have checked the code out using Windows, you may need to change the bootstrap file's permission to executable (i.e., `chmod +x bootstrap`).

Then move to the build directory (usually ./opt or ./debug) and build as you would above.

## Building Doxygen Documentation

First, make sure you've added Doxygen 1.8.X to the PATH variable in your .bashrc file.

- In the top level of your SENSEI repository, run `doxygen Doxyfile`.
- cd into doxygen/latex and run <make>, followed by `pdflatex refman.tex`
- Open refman.pdf to view the current source code reference.

You can also go into the doxygen/html folder and open index.html to view a web version.