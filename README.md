# FLASH-code-installation
Installation procedure of FLASH code for MHD simulation

All of the software is open source except FLASH4.X, you should go to http://flash.uchicago.edu/site/flashcode/ to request an account.

This installation guide is espescially for GCC version > 4.8 Redhat centos7, other versions may lead to unsuccessful installation.

My PATH-TO-INSTALL is /to/your/path/software, and my tar-files store in /to/your/path/, you can change your path by yourself. You'd better split the two path in case of you need to reinstall some part due to some error.

## Check GCC version
```
gcc -v
```
check C, C++, Fortran compiler, if returen
```
gcc version 4.8.5 20150623 (Red Hat 4.8.5-28) (GCC) 
```
your gcc is done.

## Open MPI
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
### Change 


