## Use of Linux Debian Operating System

In this directory, we will use the linux operating system Debian, and will test AlmaLinux, 
which replaces CentOS. The Debian-13 runs quite well with some treatments for recent Scalapack 
in Siesta-4.1b compilation below.
AlmaLinux-9.6 is updated for internet connection except for FFTW3, and the pip3 
correction about pairlist, but AlmaLinux goes wrong when "genice CS1 ...." is used.

### Tests of Debian and AlmaLinux OS's

After the CentOS 7 OS was terminated in May 2024, our choice may be AlmaLinux or Debian.
The Debian is the well-known best operating system to test the present AlmaLinux OS.

At the first choice, we download the AlmaLinux-9 OS on the laptop PC, with
AlmaLinux-9.6-x86\_64-dvd.iso of 10 GB memory \[1]. The installation time is
actually 15 minutes, which is rebooted for the windows-linux machine.
The second choice is Debian 13 in November 2025 where similar packages are
installed.

At AlmaLinux 9.6, we install the gcc-gfotran (Fortran) by typing $ gfortran -V,
which is necessary for physicists. We also type $ pip3 -V to install the
python3-pip package. The MPI mpich or openmpi is downloaded from the source site and installed.
For Debian-13 OS, nearly 1500 packages like cpp, make are installed at the initial condition.
The packages such as gcc, gfortran, mpich, and fftw3 are installed where they must be specified 
by one-by-one basis. But, their package names are simply as gcc, etc., instead of the 
very long names of gcc-12-... in the AlmaLinux case.

One big difference is that the internet sites of any choices are opened immediately
for Debian, while the numbers of closed internet sites including fftw3 and similar
are correctef but is not connected for the FFTW3 site in AlmaLinux 9.6.

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

The AlmaLinux-9.3 showed strange errors at the packages of pairlist, which is
corrected in AlmaLinux 9.6 by python-3.13. But, it still shows strange errors 
and stops at "pip3 install genice" (also Nov. 2025). 

We have noted that Debian-13 has no difficulty in compiling "pip3 install genice" 
and also at "genice2". Before installing "genice", one should go some steps:
$ python3 -m venv path/to/venv (in order to have path/to/venv to reach pip),
$ path/to/venv/bin/python (only to confirm),
$ path/to/venv/bin/pip install numpy, and pairlist, etc. 
$ path/to/venv/bin/pip install genice.

It generates the file mh3.exyz, for example, when one goes to "genice2 CS1...".
(Dr. Matsumoto is the author of the Genice package).

### Ab-initio Siesta

Next at the third test, we utilize the Siesta-4.1b code of the Spain group \[4],
by downloading and unpacking by "tar xf siesta-4.1b.tar.gz" (xf is now enough for unpacking).
Before testing on the Siesta-4.1b code, it is necessary to install the OpenBLAS and
Scalapack packages in prior to the Siesta code.

The OpenBLAS package is straight forward after about ten minutes, $ make and 
$ make install PREFIX=/opt/openblas, for example.
The scalapach-2.2.2 file is downloaded and expanded. At the top directory of SLmake.inc,
one should add two things "-fallow-argument-mismatch" for Fortran and 
"-Wno-implicit-function-declaration" for gcc. Then, the directories of TOOLS, SRC, PBLAS, 
BLACS and BLACS/INSTALL are automatically executed as "$ make" in these orders, to 
genarate libscalapack.a. It is 11.2 MB for the latest version of Debian-13 (Nov. 2025).

The arch.make file, mpifort and mpicc for the MPI and OMP cases are the followings
(the upper half of the arch.make, for siesta-4.1-b4gccAM1.tar.gz):  
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

The Siesta-4.1b is installed by "make" with the -fallow-argument-mismatch
switch of FFLAGS shown above. This test is shown in the
Simulatiom_in_Debian-13_AlmaLinuxn-9.pdf  [5].

## References

1. AlmaLinux Homepage https://almalinux.org/.
2. Debian Homepage, https://www.debian.org/.
3. M. Tanaka, and M. Sato, J. Chem. Physics, 126, 034509 (2007);  
   M. Tanaka, Microwave heating of water and ice by TIP5P code,  
   https://github.com/Mtanaka77/ (May 2023).
4. J. M. Soler et al., J. Phys. Cond. Matt. 14, 2745 (2002).
5. M. Matsumoto, https://github.com/vitroid/GenIce.
6. This overall-tested PDF of Simulation_in_Debian_AlmaLinux.pdf.
