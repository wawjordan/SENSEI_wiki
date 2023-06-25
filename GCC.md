Note: this is old information and needs to be updated!

## GCC

GCC version 4.7.4 or greater is required.
More information and source code can be found at http://gcc.gnu.org/

## General Compilation Information
* Download gcc version and extract the tarball
* Read the INSTALL/index.html documents or view the latest installation documents at http://gcc.gnu.org/install/
* Prerequisites are explained in the above mentioned document. The prerequisites can be compiled separately or compile with gcc. The later has has been found to be the easiest. To compile the prerequisites, gmp, mpfr, and mpc, download and extract the source folders into the main gcc source folder. All other prerequisites are already available.

## Compile GCC 4.7.4
### Download tarballs and extract or using svn
GCC and Prerequisites

It is recommended to use the exact version suggest in the documentation. MPFR and MPC websites will let you download the latest version so svn is used instead to get the correct version. GMP allows direct download of tarball. All prerequisites are moved into the gcc install folder assumed to be named gcc_4_7_4_release per the svn checkout command from above.

wget ftp://ftp.gnu.org/gnu/gcc/gcc-4.7.4/gcc-4.7.4.tar.bz2; tar -xvf gcc-4.7.4.tar.bz2

wget ftp://ftp.gmplib.org/pub/gmp-4.3.2/gmp-4.3.2.tar.bz2; tar -xvf gmp-4.3.2.tar.bz2; mv gmp-4.3.2 gcc-4.7.4/gmp

wget http://www.mpfr.org/mpfr-2.4.2/mpfr-2.4.2.tar.bz2; tar -xvf mpfr-2.4.2.tar.bz2; mv mpfr-2.4.2 gcc-4.7.4/mpfr

wget http://www.multiprecision.org/mpc/download/mpc-0.8.1.tar.gz; tar -xvf mpc-0.8.1.tar.gz; mv mpc-0.8.1 gcc-4.7.4/mpc


### Build GCC
It is recommended to build gcc in a separate directory. Attempts to build in the same directory as the source directory failed for unknown reasons. Multilib and shared libraries where disabled because gcc would not compile correctly without it. The following is the bash command to build gcc.

```js
../gcc-4.7.4/configure \-\-prefix={install path} \-\-enable-checking=release \-\-with-cpu=generic \-\-enable-languages=fortran,c,c++ \-\-disable-multilib \-\-disable-shared

make (or parallel using -j# where # is the number of processors)

make install
```

### Installation script
Copy the following lines to a text file, edit the install path, and make executable using: chmod u+x filename

```js
#!/bin/bash

PREFIX={install path}

wget ftp://ftp.gnu.org/gnu/gcc/gcc-4.7.4/gcc-4.7.4.tar.bz2; tar -xvf gcc-4.7.4.tar.bz2

wget ftp://ftp.gmplib.org/pub/gmp-4.3.2/gmp-4.3.2.tar.bz2; tar -xvf gmp-4.3.2.tar.bz2; mv gmp-4.3.2 gcc-4.7.4/gmp

wget http://www.mpfr.org/mpfr-2.4.2/mpfr-2.4.2.tar.bz2; tar -xvf mpfr-2.4.2.tar.bz2; mv mpfr-2.4.2 gcc-4.7.4/mpfr

wget http://www.multiprecision.org/mpc/download/mpc-0.8.1.tar.gz; tar -xvf mpc-0.8.1.tar.gz; mv mpc-0.8.1 gcc-4.7.4/mpc

mkdir gcc-4.7.4-build; cd gcc-4.7.4-build

../gcc-4.7.4/configure \-\-prefix=$PREFIX \-\-enable-checking=release \-\-with-cpu=generic \-\-enable-languages=fortran,c,c++ \-\-disable-multilib \-\-disable-shared

make

make install
```