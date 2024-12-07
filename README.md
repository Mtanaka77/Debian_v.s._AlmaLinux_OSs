## AlmaLinux vs Debian on Water-Ice MD and Ab-initio Siesta ##

We have tested the linux operating systems of AlmaLinux-9 and Debian-12. 
They run quite well in normal operations, but some errors will appear 
in AlmaLinux while Debian turns with success, as shown below.

### Tests of AlmaLinux vs Debian OS ###

After CentOS 7 was terminated in May 2024, our choice was AlmaLinux or Debian OS.
At the first choice, we downloaded the AlmaLinux-9 OS on the laptop PC, with
AlmaLinux-9.4-x86_64-dvd.iso of 10 GB memory [1]. The installation time was 
actually 15 minutes, which was rebooted for the windows-linux machine.
The second choice was Debian 12 in November 2024, and similar softwares were
installed.

At AlmaLinux OS, we opened the gcc-gfotran (Fortran) by typing $ gfortran -V, 
which was necessary for physicists. We also typed $ pip3 -V to open the 
python3-pip software. The MPI mpich-4 was downloaded from the source site and installed.
For Debian 12 OS, softwares such as gcc, make, mpich, and fftw3 were installed
but must call the softwares by one-by-one basis. But, their label names was simply called
gcc, etc. instead of the names of gcc-12-... in the AlmaLinux case.
The 1500 softwares were installed at the initial update.

One big difference was that the internet sites of any choices were opened immediately
for Debian, while the number of internet sites were very limited by AlmaLinux 
as closed sites including fftw3 and else.

The classic water and ice MD [2] and the ab-initio Siesta-4.1 code [3] were tested
for AlmaLinux and Debian OS.

### Classic MD ###

At the first test, the three-dimensional ice and water molecules 
@p3mtip5p03a.f03 was compiled with a parameter file parm_tip5p_D07a.h, 
and structure files 1cx666a.exyz and 1cx666a.q [2]. 
The mpich-4 and fftw-3 packages must be installed before the compilation. 
The exec run for 6 cpu at least with a start file TIP507_config.start0 
was executed by $ mpiexec -n 6 a.out &, which run all right for AlmaLinux
and Debian operating systems.

The initial state of the water and ice molecules was constructed in quaternions [4]. 
The pip and pip3 packages were installed as usual linux systems, including for CentOS 7 
and Debian operating systems. 

On the other hand, the Windows 11 system of AlmaLinux needed GitBash using the 
C/C++ packages of Vidual Studio Community, and then the related pip3 packages. 
However, the AlmaLinux-9 showed strange errors at one of the packages of pairlist 
before we should install "pip3 install genice". 
On the other hand, Debian-12 had no difficulty in compiling "pip3 install genice".

### Ab-initio Siesta ###

Next at the third test, we downloaded the Siesta-4.1b code [3] and unpacked by  
"tar -zxvf siesta-4.1b.tar.gz". Before working on the Siesta code, 
it was necessary to install the OpenBLAS and Scalapack packages.

For OpenBLAS, it was straight forward after a while.
However, the installation of Scalapack went in a different way than CentOS.
The scalapach-2.2.0 file was downloaded and expanded. At the BLACS/SRC directory, 
one typed $ make (no option), and mpif90 was done automatically to make 
a part of libscalapack.a. The same was done for PBLACS/SRC, but for SRC, 
one must have given -fallow-argument-mismatch at the Makefile's $(FC) line, and 
typed $ make -k. The TOOLS directory was typed just like $ make. It was 10.7 MB 
for the latest libscalapack.a. *) Tested by Debian 12 OS

The arch.make file and mpifort for the MPI and OMP case was the following 
(the upper half of the arch.make):  
  .SUFFIXES:  
  .SUFFIXES: .f .F .o .c .a .f90 .F90  
  SIESTA_ARCH = gfortran-MPI-OMP  

  CC = mpicc  
  FPP = $(FC) -E -P -x c  
  FC = mpifort  

  MPI_INTERFACE = libmpi_f90.a  
  MPI_INCLUDE = .   

  FFLAGS = -O2 -fPIE -ftree-vectorize -fprefetch-loop-arrays -march=native \
  -fallow-argument-mismatch -fopenmp  
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
  LIBS += /opt/scalapack/lib/libscalapack.a  

The Siesta-4.1b was installed by "make" because an error was to be avoided
by giving -fallow-argument-mismatch. The test was shown in the 
Simulatiom_in_AlmaLinux_Debian.pdf [5].


## References

1. AlmaLinux Homepage https://almalinux.org/.

2. Debian Homepage, https://www.debian.org/.

3. M. Tanaka, and M. Sato, J. Chem. Physics, 126, 034509 (2007);  
   M. Tanaka, Microwave heating of water and ice by TIP5P code,  
   https://github.com/Mtanaka77/ (May 2023).

4. J. M. Soler et al., J. Phys. Cond. Matt. 14, 2745 (2002).

5. M. Matsumoto, https://github.com/vitroid/GenIce.

6. This overall-points PDF of Simulation_in_AlmaLinux_Debian.pdf. 
