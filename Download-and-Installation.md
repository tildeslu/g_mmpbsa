---
layout: page
---

#### Contents

* [**Pre-compiled binary**](#binary)
    
    * [**Download**](#download-binary)
    
    * [**Installation**](#install-binary)

* [**Installation from source code**](#source)
    
    * [**Download**](#download-source)
    
    * [**With APBS-1.3.x**](#install-with-apbs13)
    
    * [**With APBS-1.4.x**](#install-with-apbs14)
    
    * [**Without APBS (To use with external APBS)**](#install-without-apbs)

***

###  <a name="binary"></a> Pre-compiled executable program 

Pre-compiled program **does not require** any external library or GROMACS and APBS package. These programs are standalone and without any dependency. Download, extract and use it.

##### <a name="download-binary"></a> Download


|                         |    GROMACS-4.5.x     |     GROMACS-4.6.x    |
|:------------------------|:--------------------:|:--------------------:|
|  Linux x86_64 (64 bit)  |   [Download][1]      |    [Download][2]     |
|  Linux x86 (32 bit)     |   [Download][3]      |    [Download][4]     |
|                      **To use with external APBS program**            |
|  Linux x86_64 (64 bit)  |   [Download][5]      |    [Download][6]     |
|  Linux x86 (32 bit)     |   [Download][7]      |    [Download][8]     |


[**Download Python Scripts**][9]

##### <a name="install-binary"></a> Installation

Extract the package. Copy `g_mmpbsa` and `energy2bfac` from `bin` directory to `/usr/local/bin` or `$HOME/bin`.

    tar -zxvf g_mmpbsa.tar.gz
    cd bin
    sudo cp g_mmpbsa /usr/local/bin/.
    sudo cp energy2bfac /usr/local/bin/.
    
or
    
    tar -zxvf g_mmpbsa.tar.gz
    cd bin
    cp g_mmpbsa $HOME/bin/.
    cp energy2bfac $HOME/bin/.
    
or 

add `g_mmpbsa/bin` to `PATH` environment variable by adding following line in `.bashrc` file:

    export PATH=${PATH}:/path/to/g_mmpbsa/bin

---


### <a name="source"> </a> Installation from Source-Code

#### <a name="download-source"> </a> Download

**1. ZIP/TAR**

The [ZIP](https://github.com/RashmiKumari/g_mmpbsa/zipball/master) file or [TAR](https://github.com/RashmiKumari/g_mmpbsa/tarball/master) for the tool can be downloaded from [github](https://github.com/RashmiKumari/g_mmpbsa).

Also, these files can be downloaded from the left side of current webpage. After downloading the file, extract ZIP/TAR file. 

**2. Using git**

    git clone --depth 1 https://github.com/RashmiKumari/g_mmpbsa
    

#### <a name="install-with-apbs13"> </a> Install with APBS-1.2.x or 1.3.x

**Warning:**{: style="color: red"} Compiler for GROMACS, APBS and g_mmpbsa should be same.

**Required Library from GROMACS:** `libgmx`

**Required Libraries from APBS:** `libapbsmainroutines`, `libapbs`, `libapbsblas`, `libmaloc`, `libapbsgen` and `libz`.


##### <a name="install-gromacs"></a> A. Installation of GROMACS 4.5.x/4.6.x

[GROMACS](http://www.gromacs.org/) should be compiled and installed from the source-code. 

`g_mmpbsa` cannot use GPU and cannot be compiled with CUDA. If CUDA is in PATH variable, GROMACS-4.6.x compiled by default with CUDA for GPU acceleration. To avoid error in g\_mmpbsa compilation, use `-DGMX_GPU=off` with `cmake` during installation of GROMACS-4.6.x. 

**Warning:**{: style="color: red"} Currently, double precision GROMACS is not supported.

**1. Installation of GROMACS-4.5.x**

    tar -zxvf gromacs-4.5.7.tar.gz
    cd gromacs-4.5.7
    ./configure --prefix=/opt/gromacs
    make
    sudo make install
    
`--prefix=/opt/gromacs` specifies the path where gromacs will be installed.

**2. Installation of GROMACS-4.5.7 or GROMACS-4.6.x**

    tar -zxvf gromacs-4.6.7.tar.gz
    cd gromacs-4.6.7
    mkdir build
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/opt/gromacs -DGMX_GPU=off
    make
    sudo make install

`-DCMAKE_INSTALL_PREFIX=/opt/gromacs` specifies the path where gromacs will be installed.

##### Installation of APBS 1.2.x or 1.3.x

[APBS](http://www.poissonboltzmann.org/apbs) should be compiled and installed from the source-code. 

**Warning:**{: style="color: red"} Downloaded binaries and libraries would not work as some required libraries are missing.

    tar -zxvf apbs-1.3-source.tar.gz
    cd apbs-1.3-source
    ./configure --prefix=/opt/apbs
    make
    make install

`--prefix=/opt/apbs` specifies the path where APBS will be installed.

##### C. Installation of g_mmpbsa

Follow these steps:

    cd g_mmpbsa
    mkdir build
    cd build
    cmake -DGMX_PATH=/opt/gromacs \
          -DAPBS_INSTALL=/opt/apbs \    
          -DAPBS_SRC=/path/to/apbs-1.3-source \
          -DCMAKE_INSTALL_PREFIX=/opt/g_mmpbsa \
          ..
    make
    make install

* `-DGMX_PATH` : path to GROMACS installation directory.
* `-DAPBS_INSTALL`: path to APBS installation directory.
* `-DAPBS_SRC`: path to APBS source code directroy from where APBS is configured and installed.
* `-DCMAKE_INSTALL_PREFIX`: path to a directory where g\_mmpbsa will be installed. If this option is not provided, g\_mmpbsa will install either in `/usr/local/bin` or in `$HOME/bin` directory.

* In case of **ERROR:**{: style="color: red"} `FFTW library file libfftw3f.so or libfftw3f.a not found...`; use `-DFFTW_LIB=/path/to/fftw/lib/` to give a path for the directory containing either of the two files.

#### <a name="install-with-apbs14"> </a> Install with APBS-1.4.x

**Warning:**{: style="color: red"} Compiler for GROMACS, APBS and g_mmpbsa should be same.

**Required Library from GROMACS:** `libgmx`

**Required Libraries from APBS:** `libmaloc`, `libapbs_geoflow`, `libapbs_mg`, `libapbs_pmgc` and `libapbs_generic`.

##### A. Install GROMACS 4.5.x/4.6.x

Install as described [above](#install-gromacs).

##### B. Download and Install APBS-1.4.x

Follow these steps to download and install APBS-1.4.1 version:

    # Download using git from GitHub repository
    git clone -b 1.4.1-binary-release --single-branch https://github.com/Electrostatics/apbs-pdb2pqr
    cd apbs-pdb2pqr
    cd apbs
    cd build
    sh cleanup.sh
    # Installation
    cmake -DCMAKE_INSTALL_PREFIX=/opt/apbs -DENABLE_BEM=off -DENABLE_iAPBS=on ..
    make
    sudo make install
    
##### C. Installation of g_mmpbsa

    cd g_mmpbsa
    mkdir build
    cd build
    cmake -DGMX_PATH=/opt/gromacs \
          -DAPBS14=on \
          -DAPBS_INSTALL=/opt/apbs \    
          -DCMAKE_INSTALL_PREFIX=/opt/g_mmpbsa \
          ..
    make
    sudo make install

* `-DGMX_PATH` : path to GROMACS installation directory.
* `-DAPBS14=on`: to enable installation using APBS-1.4.x libraries
* `-DAPBS_INSTALL`: path to APBS installation directory.
* `-DCMAKE_INSTALL_PREFIX`: path to a directory where g\_mmpbsa will be installed. If this option is not provided, g\_mmpbsa will install either in `/usr/local/bin` or in `$HOME/bin` directory.

* In case of **ERROR:**{: style="color: red"} `FFTW library file libfftw3f.so or libfftw3f.a not found...`; use `-DFFTW_LIB=/path/to/fftw/lib/` to give a path for the directory containing either of the two files.

#### <a name="install-without-apbs"> </a>Installation without APBS (to use with external APBS)

**Warning:**{: style="color: red"} Compiler for GROMACS, APBS and g_mmpbsa should be same.

**Required Library from GROMACS:** `libgmx`

##### A. Install GROMACS 4.5.x/4.6.x

Install as described [above](#install-gromacs).

##### B. Installation of g_mmpbsa

    cd g_mmpbsa
    mkdir build
    cd build
    cmake -DGMX_PATH=/opt/gromacs \
          -DEXT_APBS=on \
          -DCMAKE_INSTALL_PREFIX=/opt/g_mmpbsa \
          ..
    make
    sudo make install

* `-DGMX_PATH` : path to GROMACS installation directory.
* `-DEXT_APBS=on`: to enable installation without APBS libraries
* `-DCMAKE_INSTALL_PREFIX`: path to a directory where g\_mmpbsa will be installed. If this option is not provided, g\_mmpbsa will install either in `/usr/local/bin` or in `$HOME/bin` directory.

* In case of **ERROR:**{: style="color: red"} `FFTW library file libfftw3f.so or libfftw3f.a not found...`; use `-DFFTW_LIB=/path/to/fftw/lib/` to give a path for the directory containing either of the two files.


---
[1]: package/GMX45x/g_mmpbsa.tar.gz
[2]: package/GMX46x/g_mmpbsa.tar.gz
[3]: package/GMX45x_32bit/g_mmpbsa.tar.gz
[4]: package/GMX46x_32bit/g_mmpbsa.tar.gz
[5]: package/GMX45x_extrn_APBS/g_mmpbsa.tar.gz
[6]: package/GMX46x_extrn_APBS/g_mmpbsa.tar.gz
[7]: package/GMX45x_extrn_APBS_32bit/g_mmpbsa.tar.gz
[8]: package/GMX46x_extrn_APBS_32bit/g_mmpbsa.tar.gz
[9]: package/scripts.tar.gz