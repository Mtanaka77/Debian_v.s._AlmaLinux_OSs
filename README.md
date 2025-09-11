## Debian v.s. AlmaLinux OS's on Water-Ice MD and Ab-initio Siesta ##

In this directory, we have tested the two linux operating systems of AlmaLinux-9 and Debian-12. 
They run quite well in normal operations, but some errors will appear in AlmaLinux OS while 
Debian OS runs successfully, as shown below.

### Tests of Debian v.s. AlmaLinux OS's ###

After the CentOS 7 OS was terminated in May 2024, our choice is the AlmaLinux or Debian OS.
At the first choice, we download the AlmaLinux-9 OS on the laptop PC, with
AlmaLinux-9.4-x86_64-dvd.iso of 10 GB memory [1]. The installation time is 
actually 15 minutes, which is rebooted for the windows-linux machine.
The second choice is Debian 12 in November 2024, and similar softwares are
installed.

At AlmaLinux OS, we open the gcc-gfotran (Fortran) by typing $ gfortran -V, 
which is necessary for physicists. We also type $ pip3 -V to open the 
python3-pip software. The MPI mpich-4 is downloaded from the source site and installed.
For Debian-12 OS, softwares such as gcc, make, mpich, and fftw3 are installed
but must call the softwares by one-by-one basis. But, their label names is simply named 
gcc, etc. instead of the long names of gcc-12-... in the AlmaLinux case.
The 1500 softwares are installed at the initial update.

One big difference is that the internet sites of any choices are opened immediately
for Debian, while the numbers of closed internet sites including fftw3 and similar 
are so many in AlmaLinux 
.

The classic water and ice MD [2] and the ab-initio Siesta-4.1 code [3] are tested
for AlmaLinux and Debian OS.

### Classic MD ###

At the first test, the three-dimensional ice and water molecules 
@p3mtip5p03a.f03 is compiled with a parameter file parm_tip5p_D07a.h, 
and structure files 1cx666a.exyz and 1cx666a.q [2]. 
The mpich-4 and fftw-3 packages must be installed before the compilation. 
The exec run for 6 cpu at least with a start file TIP507_config.start0 
is executed by $ mpiexec -n 6 a.out &, which runs all right for AlmaLinux
and Debian operating systems.

The initial state of the water and ice molecules is constructed in quaternions [4]. 
The pip and pip3 packages are installed as usual linux systems, including for CentOS 7 
and Debian operating systems. 

On the other hand, AlmaLinux requires the Windows 11 system of GitBash and the 
C/C++ packages of Vidual Studio Community, and the related pip3 packages as well. 
It then requires largy memory sizes like 10-20 GB ! 
However, the AlmaLinux-9 shows strange errors at one of the packages of pairlist 
before we should install "pip3 install genice". 
We have noted that Debian-12 has no difficulty in compiling "pip3 install genice",
not help of Windows' GitBash.

### Ab-initio Siesta ###

Next at the third test, we utilize the Siesta-4.1b code of the Spainish group [4], 
by downloading and unpacking by "tar -zxvf siesta-4.1b.tar.gz". 
Before testing on the Siesta-4.1b code, it is necessary to install the OpenBLAS and 
Scalapack packages in prior to the Siesta code.

For OpenBLAS, it is straight forward after a while.
However, the installation of Scalapack needs to go in a different way than CentOS.
The scalapach-2.2.0 file is downloaded and expanded. At the BLACS/SRC directory, 
one type $ make (no option), and mpif90 is done automatically to make 
a part of libscalapack.a. The same is true for PBLACS/SRC, but for SRC, 
one must have given -fallow-argument-mismatch at the Makefile's $(FC) line, and 
type $ make -k. The TOOLS directory is typed just like $ make. It is 10.7 MB 
for the latest libscalapack.a. *) Test by Debian 12 OS

The arch.make file and mpifort for the MPI and OMP case are the followings 
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

The Siesta-4.1b is installed by "make" with -fallow-argument-mismatch 
argument of FFLAGS shown above. This test is shown in the 
Simulatiom_in_AlmaLinux_Debian.pdf [5].

With the aforementioned tests above, we have concluded to use Debian-12 in our
desktop and laptop PCs.

## References

1. AlmaLinux Homepage https://almalinux.org/.

2. Debian Homepage, https://www.debian.org/.

3. M. Tanaka, and M. Sato, J. Chem. Physics, 126, 034509 (2007);  
   M. Tanaka, Microwave heating of water and ice by TIP5P code,  
   https://github.com/Mtanaka77/ (May 2023).

4. J. M. Soler et al., J. Phys. Cond. Matt. 14, 2745 (2002).

5. M. Matsumoto, https://github.com/vitroid/GenIce.

6. This overall-tested PDF of Simulation_in_AlmaLinux_Debian.pdf. 
