# FLASH-code-installation
Installation procedure of FLASH code for MHD simulation

All of the software is open source except FLASH4.X, you should go to http://flash.uchicago.edu/site/flashcode/ to request an account.

This installation guide is espescially for GCC version > 4.4.7 Red Hat server, other versions may lead to unsuccessful installation.

My PATH-TO-INSTALL is /flash/software/hypre and /flash/software/hdf5, and my tar-files store in /flash/, you can change your path by yourself. You'd better split the two path in case of you need to reinstall some part due to some error.

## Install Open MPI

Suggest to use mpi have been installed with system, because it will be convenient to submit your missions.

Check the path of system mpi and remember this path.
```
which mpicc
```
And remember your install-path and skip to install Hypre.
```
cd flash/software/hypre
pwd
```
Maybe it will show like below (up to your server name)
```
/abcde/usrname/flash/software/hypre
```

If you want to install another one, you can install openmpi or mpich, or both

You can download mpich at ：www.mpich.org/downloads/

You can download openmpi at : https://www.open-mpi.org/software/ompi/v4.0/
```
tar -zxvf filename.tar.gz
```
```
cd openmpi-x-y-z
```
```
./configure --prefix=/abcde/usrname/flash/software/openmpi
```
if no error, then
```
make
make install
```
#### Add environment variable
```
cd ~
vi .bashrc
```
add the environment variable at the end of the document
```
export PATH=/abcde/usrname/flash/software/hypre/bin:/abcde/usrname/flash/software/hdf5/bin:/abcde/usrname/flash/software/openmpi/bin:$PATH
export LD_LIBRARY_PATH=/abcde/usrname/flash/software/hdf5/lib:/abcde/usrname/flash/software/openmpi/lib
```
make the variable effect
```
source .bashrc
```
or restart the connection.


## Install Hypre

You can download hypre at : https://computation.llnl.gov/casc/hypre/software.html 
```
tar -zxvf filename.tar.gz
cd hypre-x-y-z/src
```
```
./configure --prefix=/abcde/usrname/flash/software/hypre CC=mpicc FC=mpif90
```
if report error like:
```
cannot run C compiled programs
```
then add '--host=x86_64' when configure,if no error, then
```
make
make install
```


## Install HDF5 and links

You can download hypre at : www.hdfgroup.org/downloads/hdf5/ , hdf5-1.8.x is prefered.
```
tar -zxvf filename.tar.gz
cd hdf5-x-y-z
```
```
CC=mpicc FC=mpif90 ./configure --enable-parallel --enable-fortran --enable-shared --prefix=/abcde/usrname/flash/software/hdf5
```
if no error, then
```
make
make install
```
Check h5pcc to make sure the installation succeed.
```
h5pcc -showconfig
```
If the returned information contained:
```
                      AM_CFLAGS:
                       CPPFLAGS:
                    H5_CPPFLAGS: -D_GNU_SOURCE -D_POSIX_C_SOURCE=200112L   -DNDEBUG -UH5_DEBUG_API
                    AM_CPPFLAGS:
               Shared C Library: yes
               Static C Library: yes
  Statically Linked Executables: no
                        LDFLAGS:
                     H5_LDFLAGS:
                     AM_LDFLAGS:
 	 	Extra libraries: -lz -ldl -lm
 		       Archiver: ar
 		 	 Ranlib: ranlib
 	      Debugged Packages:
		    API Tracing: no

Languages:
----------
                        Fortran: yes
               Fortran Compiler: /opt/intel_2019/compilers_and_libraries_2019.4.243/linux/mpi/intel64/bin/mpif90 ( built with gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC))
          Fortran 2003 Compiler:
                  Fortran Flags:
               H5 Fortran Flags:
               AM Fortran Flags:
         Shared Fortran Library: yes
         Static Fortran Library: yes

                            C++: no

Features:
---------
                  Parallel HDF5: yes
             High Level library: yes
                   Threadsafety: no
            Default API Mapping: v18
 With Deprecated Public Symbols: yes
         I/O filters (external): deflate(zlib)
                            MPE:
                     Direct VFD: no
                        dmalloc: no
Clear file buffers before write: yes
           Using memory checker: no
         Function Stack Tracing: no
      Strict File Format Checks: no
   Optimization Instrumentation: no
```
that means the installation succeed.


## Add environment variable
```
cd ~
vi .bashrc
```
add the environment variable at the end of the document
```
export PATH=/abcde/usrname/flash/software/hypre/bin:/abcde/usrname/flash/software/hdf5/bin:$PATH
export LD_LIBRARY_PATH=/abcde/usrname/flash/software/hdf5/lib
```
make the variable effect
```
source .bashrc
```
or restart the connection.


## Install and Test FLASH4.6

```
tar -zxvf filename.tar.gz
cd FLASH4.6
```
setup the example 'Sedov'
```
./setup Sedov -auto
```
Modify the path of above parts in ```/Object/Makefile.h/Makefile.h```
```
cd object
vi Makefile.h
```
You have to reset MPI, HDF5, HYPRE path, and delete other path in Makefile.h
```
MPI_PATH=
HDF5_PATH=/abcde/usrname/flash/software/hdf5
HYPRE_PATH=/abcde/usrname/flash/software/hypre
ZLIB_PATH=
```
Then you can run 'Sedov' example
```
make
mpirun -np 4 ./flash4
```
Output normally until reached max SimTime means FLASH4.6 is installed successfully.
Now you can setup your own simulation. (Better to make a new directory to store your simulation by add ```-objdir=New_directory``` when ```setup```)
And don't modify the examples in ```/FLASH4.6/source/Simulation/SimulationMain/```, you can make a new directory and copy the example to it.