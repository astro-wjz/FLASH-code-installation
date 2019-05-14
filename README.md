# FLASH-code-installation
Installation procedure of FLASH code for MHD simulation

All of the software is open source except FLASH4.X, you should go to http://flash.uchicago.edu/site/flashcode/ to request an account.

This installation guide is espescially for GCC version > 4.8 Redhat centos7, other versions may lead to unsuccessful installation.

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
You can install openmpi or mpich, or both
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

## Install zlib

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

# Install Hypre

You can download hypre at : https://computation.llnl.gov/casc/hypre/software.html 
```
tar -zxvf filename.tar.gz
```
```
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






