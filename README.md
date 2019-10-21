# FLASH-code-installation
Installation procedure of FLASH code for MHD simulation

All of the software is open source except FLASH4.X, you should go to http://flash.uchicago.edu/site/flashcode/ to request an account.

This installation guide is espescially for GCC version > 4.4.7 Red Hat server, other versions may lead to unsuccessful installation.

My PATH-TO-INSTALL is /to/your/path/software, and my tar-files store in /to/your/path/, you can change your path by yourself. You'd better split the two path in case of you need to reinstall some part due to some error.

## Check GCC version
```
gcc -v
```
check C, C++, Fortran compiler, if return
```
gcc version 4.8.5 20150623 (Red Hat 4.8.5-28) (GCC) 
```
your gcc is done.

## Install Open MPI

Suggest to use mpi have been installed with system, because it will be conveinent to submit your missions.

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
./configure --prefix=/to/your/path/software/openmpi
```
if no error, then
```
make
make install
```
#### Add environment variable
```
cd ~
gedit .bashrc
```
add the environment variable at the end of the document
```
export USHOME=/to/your/path/software
export PATH=$USHOME/hypre/bin:$USHOME/hdf5/bin:$USHOME/openmpi/bin:$PATH
export LD_LIBRARY_PATH=/to/your/path/software/hdf5/lib:/to/your/path/software/openmpi/lib
```
make the variable effect
```
source .bashrc
```
or restart the connection.

Test mpi and run examples.
```
cd to/your/path/openmpi-x-y-z/examples
mpicc -o hello_c hello_c.c
mpiexec -np 4 ./hello_c
mpirun -np 4 ./hello_c
```
Output normally means openmpi is installed successfully.

Or you can add the environment variable by this way:
```
cd ~
gedit .bash_profile
```
add the environment variable at the end of the document
```
export PATH=/to/your/path/software/openmpi/bin:$PATH
export INCLUDE=/to/your/path/software/openmpi/include:$INCLUDE
export LD_LIBRARY_PATH=/to/your/path/software/openmpi/lib:$LD_LIBRARY_PATH
```
make the variable effect
```
source .bashrc
```

## Install zlib ( No need to install this unit unless you need '-lz' )

You can download zlib at : http://www.zlib.net (maybe need VPN) or other mirror sites
```
tar -zxvf filename.tar.gz
```
```
cd zlib-x-y-z
```
```
./configure --prefix=/to/your/path/software/zlib
```
if no error, then
```
make
make install
```
Most of the time you don't need to add environment variables, but if 'make' reports errors like 'No such file or directory' about zlib later, you have to do that.
```
cd ~
gedit .bash_profile
```
add the environment variable at the end of the document
```
export PATH=/to/your/path/software/zlib/bin:$PATH
export INCLUDE=/to/your/path/software/zlib/include:$INCLUDE
export LD_LIBRARY_PATH=/to/your/path/software/zlib/lib:$LD_LIBRARY_PATH
```
make the variable effect
```
source .bashrc
```

## Install Hypre

You can download hypre at : https://computation.llnl.gov/casc/hypre/software.html 
```
tar -zxvf filename.tar.gz
cd hypre-x-y-z
```
```
./configure --prefix=/to/your/path/software/hypre CC=mpicc FC=mpif90
```
if report error like:
```
cannot run C compiled programs
```
then add '--host=x86_64' when configure, just like 
```
./configure --prefix=/to/your/path/software/hypre CC=mpicc FC=mpif90 --host=x86_64
```
if no error, then
```
make
make install
```
Most of the time you don't need to add environment variables, but if 'make' reports errors like 'No such file or directory' about hypre, you have to do that.
```
cd ~
gedit .bash_profile
```
add the environment variable at the end of the document
```
export PATH=/to/your/path/software/hypre/bin:$PATH
export INCLUDE=/to/your/path/software/hypre/include:$INCLUDE
export LD_LIBRARY_PATH=/to/your/path/software/hypre/lib:$LD_LIBRARY_PATH
```
make the variable effect
```
source .bashrc
```

Test hypre diffuse unit and mpirun.
```
cd src/example
```
modify the installation path in Makefile ```HYPRE_DIR=/to/your/path/software/hypre```
```
make
mpirun -np 2 filename
```
Output normally means hypre and mpi are installed successfully.

If report warnings like ```libifport.so```, you have to add environment variable in ~/.bashrc below
```
export LD_LIBRARY_PATH=/opt/intel/compilers_and libraries_2017.2.174/linux/compiler/lib/intel64_lin:$LD_LIBRARY_PATH
```
Attention: the path to libifport.so is different, you have to locate the files yourself.

## Install HDF5 and links

You can download hypre at : www.hdfgroup.org/downloads/hdf5/ , hdf5-1.8.x is prefered.
```
tar -zxvf filename.tar.gz
cd hdf5-x-y-z
```
```
CC=/path/to/mpicc FC=/path/to/mpif90 ./configure --enable-parallel --enable-fortran --enable-shared --prefix=/to/your/path/software/hdf5
```
if report error like:
```
cannot run C compiled programs
```
then add '--host=x86_64' when configure, just do like before.

Or you have to set the links by yourself, like
```
export AM_LDFLAGS:  -L/to/your/path/software/zlib/lib
export AM_CPPFLAGS:  -I/to/your/path/software/zlib/include
CC=/to/your/path/software/openmpi/bin/mpicc FC=/to/your/path/software/openmpi/bin/mpif90 ./configure --enable-fortran --prefix=/to/your/path/software/hdf5
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
Installation point: /to/your/path/software/hdf5
AM_LDFLAGS:  
C Compiler: /to/your/path/software/openmpi/bin/mpicc ( MPICH version 3.2.1 built with gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC))
AM_CPPFLAGS:  
Fortran Compiler: /to/your/path/software/openmpi/bin/mpif90 ( MPICH version 3.2.1 built with gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC))
                  Fortran Flags: 
               H5 Fortran Flags:  -pedantic -Wall -Wextra -Wunderflow -Wimplicit-interface -Wsurprising -Wno-c-binding-type  -s -O2
```
that means the installation succeed.(I installed mpich here)

Most of the time you don't need to add environment variables, but if 'make' reports errors like 'No such file or directory' about hdf5, you have to do that.
```
cd ~
gedit .bash_profile
```
add the environment variable at the end of the document
```
export PATH=/to/your/path/software/hdf5/bin:$PATH
export INCLUDE=/to/your/path/software/hdf5/include:$INCLUDE
export LD_LIBRARY_PATH=/to/your/path/software/hdf5/lib:$LD_LIBRARY_PATH
```
make the variable effect
```
source .bashrc
```

Test hdf5 and links.
```
cd example
./run-all-ex.sh
```
If report errors like ```./run-all-ex.sh: line53: ../../../bin/h5cc: No such file or directory messed up compiling h5_crtdat.c```
you have to modify line 34 and line 44 in ```run-c-ex.sh```, add ```/to/your/path/software/hdf5/bin```to line 44 and delete ```../../..``` in line 34.

Output normally means hdf5 and links are installed successfully.

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

You have to reset MPI, HDF5, HYPRE, ZLIB path, and delete other path in Makefile.h
```
MPI_PATH=/to/your/path/software/openmpi
HDF5_PATH=/to/your/path/software/hdf5
HYPRE_PATH=/to/your/path/software/hypre
ZLIB_PATH=
```
Then you can run 'Sedov' example
```
cd object
make
mpirun -np 4 ./flash4 -par_file flash.par
```
#### Error 1

If you meet an error like ```cannot find -lz``` when ```make```

you have to copy ```/to/your/path/software/zlib/lib/libz.a``` to ```/to/your/path/software/hdf5/lib```

#### Error 2

If you meet an error like```error while loading shared libraries: libpython2.7.so: cannot open shared object file: No such file or directory```

you have to add environment viriable
```
export LD_LIBRARY_PATH=/usr/local/aegis/SecureCheck/lib:$LD_LIBRARY_PATH
```

#### Error 3

If you meet an error like```error while loading shared libraries: libhdf5.so.101:cannot open shared object file: No such file or directory```

you have to add environment viriable
```
export LD_LIBRARY_PATH=/to/your/path/software/hdf5/lib:$LD_LIBRARY_PATH
```

Output normally until reached max SimTime means FLASH4.6 is installed successfully.
Now you can setup your own simulation. (Better to make a new directory to store your simulation by add ```-objdir=New_directory``` when ```setup```)

## Visualizing output files

Make sure your connection is X server connection or you cannot visualize.

You can download VisIt at : https://wci.llnl.gov/simulation/computer-codes/visit/executables

Find any directory to install Visit, you just have to extract tar file directly, and
```
cd visitx.y.z/bin
./visit
```
then you can read output files just like on Windows platform.

On Windows platform, you can visualizing VisIt through "MobaXterm", you can download it everywhere.

On Mac or Linux platform, you should install XQuartz first, then modify ```/private/etc/ssh/ssh_config``` 

```
ForwardX11 yes
```

then modify ```/private/etc/ssh/sshd_config```

```
X11Forwarding yes
X11DisplayOffset 10
```

Attention: when you connect to romote servers, you should input

```
ssh -X user@hostname
```



