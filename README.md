## Debian v.s. AlmaLinux OS

In this directory, we have tested the two linux operating systems of AlmaLinux-9 and Debian-12.
They run quite well in normal operations. The Debian OS runs quite successfully while some errors
appeared in the tested AlmaLinux-9.3 OS. The updated AlmaLinux-9.6 has been updated for
usual internet connections except for FFTW3 by MIT, and the tested softwares of pairlist.
However, it goes errors at the Genice package after "pip3 install genice" !

### Tests of Debian and AlmaLinux OS's

After the CentOS 7 OS was terminated in May 2024, our choice is the AlmaLinux or Debian OS.
The Debian OS is the well-known best OS to test the present AlmaLinux OS.

At the first choice, we download the AlmaLinux-9 OS on the laptop PC, with
AlmaLinux-9.4-x86\_64-dvd.iso of 10 GB memory \[1]. The installation time is
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
are so many in AlmaLinux. This point has been updated but is not open for FFTW3
in AlmaLinux 9.6.

The classic water and ice MD \[2] and the ab-initio Siesta-4.1 code \[3] are tested
for AlmaLinux and Debian OS.

### Classic MD

At the first test, the three-dimensional ice and water molecules
@p3mtip5p03a.f03 is compiled with a parameter file parm\_tip5p\_D07a.h,
and structure files 1cx666a.exyz and 1cx666a.q \[2].
The mpich-4 and fftw-3 packages must be installed before the compilation.
The exec run for 6 cpu at least with a start file TIP507\_config.start0
is executed by $ mpiexec -n 6 a.out \&, which runs all right for AlmaLinux
and Debian operating systems.

The initial state of the water and ice molecules is constructed in quaternions \[4].
The pip and pip3 packages are installed as usual linux systems, including for CentOS 7
and Debian operating systems.

On the other hand, AlmaLinux requires the Windows 11 system of GitBash and the
C/C++ packages of Vidual Studio Community, and the related pip3 packages as well.
It then requires largy memory sizes like 10-20 GB !
The AlmaLinux-9.3 showed strange errors at the packages of pairlist, which is
corrected in AlmaLinux 9.6 by python-3.13.
But, it still shows strange errors and stops at "pip3 install genice" (also in Nov. 2025). 

We have noted that Debian-12 has no difficulty in compiling "pip3 install genice" 
and also at "genice2". It generates the file mh3.exyz when one goes to "genice2 CS1...".
(Dr. Matsumoto is the author of the Genice package).

### Ab-initio Siesta

Next at the third test, we utilize the Siesta-4.1b code of the Spainish group \[4],
by downloading and unpacking by "tar xf siesta-4.1b.tar.gz" (xf is enough for unpacking).
Before testing on the Siesta-4.1b code, it is necessary to install the OpenBLAS and
Scalapack packages in prior to the Siesta code.

For OpenBLAS, it is straight forward after a while.
However, the installation of Scalapack needs to go in a different way than CentOS.
The scalapach-2.2.0 file is downloaded and expanded. At the BLACS/SRC directory,
one type $ make (no option), and mpif90 is done automatically to make
a part of libscalapack.a. The same is true for PBLACS/SRC and SRC for which 
one must have -fallow-argument-mismatch at the Makefile's $(FC) line, and
type $ make -i. The TOOLS directory is typed just like $ make. It is 10.7 MB
for the latest libscalapack.a. \*) Test by Debian 12 OS

The arch.make file and mpifort for the MPI and OMP case are the followings
(the upper half of the arch.make):  
.SUFFIXES:  
.SUFFIXES: .f .F .o .c .a .f90 .F90  
SIESTA\_ARCH = gfortran-MPI-OMP

CC = mpicc  
FPP = $(FC) -E -P -x c  
FC = mpifort

MPI\_INTERFACE = libmpi\_f90.a  
MPI\_INCLUDE = .

FFLAGS = -O2 -fPIE -ftree-vectorize -fprefetch-loop-arrays -march=native   
-fallow-argument-mismatch -fopenmp  
FC\_SERIAL = gfortran

AR = ar  
RANLIB = ranlib  
SYS = nag

SP\_KIND = 4  
DP\_KIND = 8  
KINDS   = $(SP\_KIND) $(DP\_KIND)

FPPFLAGS = -DMPI  
LDFLAGS  =  
INCFLAGS =  
INSDIR = /opt  
COMP\_LIBS =     # libsiestaLAPACK.a libsiestaBLAS.a  
LDFLAGS += -L$(INSDIR)/openblas/lib -Wl,-rpath=$(INSDIR)/openblas/lib  
LIBS = -lgomp -L/opt/openblas/lib -lopenblasomp  
LIBS += -L/opt/scalapack/lib -lscalapack

The Siesta-4.1b is installed by "make" with -fallow-argument-mismatch
argument of FFLAGS shown above. This test is shown in the
Simulatiom\_in\_AlmaLinux\_Debian.pdf \[5].

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
6. This overall-tested PDF of Simulation\_in\_AlmaLinux\_Debian.pdf.
