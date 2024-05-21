# AlmaLinux-9 with Siesta-4.1b

## Test Linux AlmaLinux-9 with Siesta-4.1b ##

As the successor of CentOS 7, I downloaded the software AlmaLinux-9 
on the laptop PC, and tested the linux for the Siesta-4.1b code [1].
The installation was all right with AlmaLinux-9.4-x86_64-dvd.iso
of 10 GB memory, and was finished in 15 minutes for reboot.

At the beginning, I opened the gfotran by # gfortran -V which
was necessary on AlmaLinux-9. I also did # pip -V.
The MPI mpich4 was downloaded and installed for the Siesta-4.1 code.


Then, I downloaded the Siesta-4.1b code and did unpacking:  
% tar -zxvf siesta-4.1b.tar.gz.
Before the Siesta code, it was necessary to install OpenBLAS and Scalapack.
For OpenBLAS, it was straight forward after a while.
However, for Scalapack, my installation went wrong as # make -i,
since VirtalBox is used where the AlmaLinux-9 showed unnecessary errors 
without success.
So for the being, the import of ./lib/libscalapack.a was made from 
the CentOS 7.

The arch.make file is the following:  
  .SUFFIXES:  
  .SUFFIXES: .f .F .o .c .a .f90 .F90  
  SIESTA_ARCH = gfortran-MPI  

  CC = mpicc  
  FPP = $(FC) -E -P -x c  
  FC = mpifort  

  MPI_INTERFACE = libmpi_f90.a  
  MPI_INCLUDE = .   

  FFLAGS = -O2 -fPIC -ftree-vectorize -march=native -fallow-argument-mismatch  
 #FFLAGS = -O2 -fexpensive-optimizations -ftree-vectorize -fprefetch-loop-arrays -march=native -fPIC -fopenmp  
  FC_SERIAL = gfortran  

  AR = ar  
  RANLIB = ranlib  
  SYS = nag  

  SP_KIND = 4  
  DP_KIND = 8  
  KINDS   = $(SP_KIND) $(DP_KIND)   
  
  FPPFLAGS = -DMPI   
  LDFLAGS  =  
  INCFLAGS =  
  INSDIR = /opt  
  COMP_LIBS =     # libsiestaLAPACK.a libsiestaBLAS.a  
  LDFLAGS += -L$(INSDIR)/openblas/lib -Wl,-rpath=$(INSDIR)/openblas/lib  
  LIBS = -lgomp -L/opt/openblas/lib -lopenblas  
  LIBS += -L/opt/scalapack/lib -lscalapack  

The Siesta-4.1b was installed by "make -i" because the error stop was avoided.
The test is shown in the Siesta-4.1bTest.pdf file.


## References

1. J. M. Soler et al., J. Phys. Cond. Matt. 14, 2745 (2002).


