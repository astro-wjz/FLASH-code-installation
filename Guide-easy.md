# FLASH-code-installation

- 1. Install MPI (not nessesary on server)
- 2. Install HYPRE
- 3. Install HDF5
- 4. Install FLASH

Installation guide of FLASH code for MHD simulation

All of the softwares are open source except FLASH4.X, users should go to http://flash.uchicago.edu/site/flashcode/ to request an account.

This installation guide is espescially for GCC version > 4.4.7 Red Hat server, other versions may lead to unsuccessful installation.

My PATH-TO-INSTALL is /flash/software/hypre and /flash/software/hdf5, and my tar-files is stored in /flash/, users can change your path by yourself. You'd better split the two path in case of you need to reinstall some part due to some error.

- ## Install Open MPI

**Recommand strongly** to use mpi which have been installed with system, load existed module is a good choise.

If users want to install another version of mpi, firstly check the path of system mpi and remember this path.
```
which mpiicc
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

If users want to install another one, users can install openmpi or mpich, or both

Users can download mpich at ：www.mpich.org/downloads/

Users can download openmpi at : https://www.open-mpi.org/software/ompi/v4.0/
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

Most of time, users don't have to add any environment variables.
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
or restart the terminal.


- ## Install Hypre

Users can download hypre at : https://computation.llnl.gov/casc/hypre/software.html 
```
tar -zxvf filename.tar.gz
cd hypre-x-y-z/src
```
For intel compiler, you can use `CC=mpiicc` and `FC=mpiifort`, for other compiler, you have to use `CC=mpicc` and `FC=mpif90`.
```
./configure --prefix=/abcde/usrname/flash/software/hypre CC=mpiicc FC=mpiifort CXX=mpiicpc
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


- ## Install HDF5 and links

Users can download hypre at : www.hdfgroup.org/downloads/hdf5/ , hdf5-1.8.x is prefered.
```
tar -zxvf filename.tar.gz
cd hdf5-x-y-z
```
```
CC=mpiicc FC=mpiifort CXX=mpiicpc ./configure --enable-parallel --enable-fortran --prefix=/abcde/usrname/flash/software/hdf5
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


- ## Install and Test FLASH4.6

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
Users have to modify MPI, HDF5, HYPRE path, and delete other path in Makefile.h
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

There are some 2D and 3D examples in https://github.com/astro-wjz/FLASH-EXAMPLES .
