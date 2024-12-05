## AlmaLinux vs Debian on Water-Ice MD and Ab-initio Siesta Codes

We have tested the linux operating system, AlmaLinux-9 and Debian-12. 
They run well in normal operations, but some errors in AlmaLinux while
Debian is success as shown below.

For the successor of CentOS 7 that is terminated in May 2024, we download the
AlmaLinux on the laptop PC. The OS is AlmaLinux-9.4-x86_64-dvd.iso of 10 GB memory [1]. 
The actual installation time is about 15 minutes which is rebooted for new Linux windows.

Debian 12 OS is installed, with choice softwares added such as gcc, make, mpich
and fftw3. The 1500 softwares are installed at initial update.  

At the first time, we open the gcc-gfotran (Fortran) by typing # gfortran -V, 
which is necessary on AlmaLinux-9. We also type # pip3 -V to open the python3-pip software.
The MPI of mpich-4 is downloaded from the source site and installed.

Softwares for Debian OS are added one by one if they are necessary; they are simply
named of labels (gcc instead of gcc-12-... in AlmaLinux).
The classic water and ice MD [2] and the ab-initio Siesta-4.1 code [3] are tested
for AlmaLinux.

### Classic MD ###

At the first test, the three-dimensional ice and water molecules 
@p3mtip5p03a.f03 is compiled with a parameter file parm_tip5p_D07a.h, 
and structure files 1cx666a.exyz and 1cx666a.q [2]. 
The mpich-4 and fftw-3 packages must be installed before the compilation. 
The exec run for 6 cpu at least with a start file TIP507_config.start0 
is executed by # mpiexec -n 6 a.out &, which runs all right.

The initial state of the water and ice molecules is constructed in quaternions [4]. 
The pip and pip3 packages are installed as usual linux systems, including for CentOS 7 
and Debian operating systems. 

On the other hand, the Windows 11 system needs GitBash using the C/C++ packages of 
Vidual Studio Community, and then the related pip3 packages. 
However, the AlmaLinux-9 shows strange errors at one of the packages of pairlist 
before we should install # pip3 install genice. 　

### Ab-initio Siesta ###

Next at the second test, we download the Siesta-4.1b code [3] and unpack by:  
% tar -zxvf siesta-4.1b.tar.gz. Before working on the Siesta code, 
it is necessary to install the OpenBLAS and Scalapack packages.
For OpenBLAS, it is straight forward after a while.

However, the installation of Scalapack must go in a different way than CentOS.
The scalapach-2.2.0 file is downloaded and expanded. At the BLACS/SRC directory, 
one must type $ make (no option), and mpif90 is done automatically.
The same for PBLACS/SRC, but for SRC, one must give -fallow-argument-mismatch
at Makefile's $(FC) line, and type $ make -k. The TOOLS directory is typed 
just like $ make. It will be 10.7 MB for the latest libscalapack.a.
*) Tested by Debian 12 OS

The arch.make file for the MPI case mpifort is the following (the upper half
of the arch.make):  
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

The Siesta-4.1b is installed by "make -i" because the error stop is avoided.
The test is shown in the Siesta-4.1bTest.pdf [5].


## References

1. AlmaLinux OS, https://almalinux.org/.

2. M. Tanaka, and M. Sato, J. Chem. Physics, 126, 034509 (2007);  
   M. Tanaka, Microwave heating of water and ice by TIP5P code,  
   https://github.com/Mtanaka77/ (May 2023).

3. J. M. Soler et al., J. Phys. Cond. Matt. 14, 2745 (2002).

4. M. Matsumoto, https://github.com/vitroid/GenIce.

5. This homepage PDF file of MD_Siesta-4_Tests.pdf. 
