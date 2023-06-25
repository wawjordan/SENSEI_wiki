Note: This page is out of date and refers to the old versions of pFUnit.
The latest versions of pFUnit do not work this way and TODO this page will need to be updated.
In the meantime you can look in the `SENSEI/test/unittest` folder for examples of the unit tests and how to add them to the build system.

## Info 

pFunit is unit testing software required for testing of various functions and subroutines. More details can be found here http://opensource.gsfc.nasa.gov/projects/FUNIT/index.php#software. 

The software can be downloaded at the project page http://sourceforge.net/projects/pfunit/files/

## Installation
Download the latest software from http://sourceforge.net/projects/pfunit/files/

- Extract files: tar -xf pFUnit.tar.gz
- Check F90_Vendor argument in GNUmakefile to makesure compiler is correctly specified. 

Example:
SENSEI is compiled using gfortran; however, the default gfortran is the default gfortran installed on the given system. If you haven't set up your .bashrc file, then you'll have to do the following four steps:
- Open GNUmakefile in your favorite text editor
- Locate if statement specific to gfortran:  ifeq ($(F90_VENDOR),GFortran)
- Modify F90 variable to point to correct gfortran executable: F90 ?={path}/gfortran
- Save changes

To compile and install:
- Compile: make tests F90_VENDOR=GFortran
- Install: make install INSTALL_DIR={install_path}
- Add to path: modify .bashrc
export PFUNIT=[install_path]

## Usage
pFunit has two methods of implementing unit test routines. An automatic method is available which will search the subdirectories for unit tests, and a manual method which requires the user to code the primary test routines. SENSEI uses the manual method which is then automated for simple integration with various libraries and requires a specific naming standard. The following file structure is required:

- src/libname/libfile.f90
library files: files which contain subroutines or functions all contained in a fortran module 
- src/libname/pfunit/driverlibname.90
test driver: a subroutine which sets up and executes the tests for the specific library contained inside a module
- src/libname/pfunit/testlibname.f90
unit tests: subroutines with units tests for the subroutines and functions included in the library files inside a module
- src/pfunit/pfunitmain.f90
pfunit program: program which uses the library modules and driver modules to execute all unit tests. This program is generated automatically.

A simple example implementing pfunit can be found in resources/examples/pfunit_example1.tar

A more complicated example using automake will be added in the future...




