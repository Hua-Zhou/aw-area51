# User Guide

## System information

* Chassis: Alienware Area51 R5. Full specs: <https://www.dell.com/support/manuals/us/en/04/alienware-area51-r4/alienwarearea51r5_setupandspecs/specifications?guid=guid-2795f926-e3d3-4d85-9813-11a63248dabb&lang=en-us>

* CPU: Intel i9 9920X (12 core, 12.3MB L2 Cache, 19.7MB L3 Cache, 3.5 GHz up to 4.5GHz with Intel Turbo Boost Max 3.0). Full specs: <https://ark.intel.com/content/www/us/en/ark/products/189127/intel-core-i9-9920x-x-series-processor-19-25m-cache-up-to-4-50-ghz.html>.

* GPU: NVIDIA GeForce RTX 2080 Ti OC with 11GB GDDR6 (14 Gbps), 4352 cores, SP 13.4 TFLOPs, INT4 430TOPs. Full specs: <https://www.nvidia.com/en-us/geforce/graphics-cards/rtx-2080-ti/>.

* Memory: 64GB DDR4 SDRAM 2666 MHz (4 slots of 16GB).

* SSD 1 (Kingston): 512GB PCIe NVMe M.2 SSD, CentOS 7 bootable.

* SSD 2 (Kodak): 920GB SATA III SSD, mounted on `/mnt/AppRun`. Use this as scratch space for big data computation (ephemeral). Make sure to delete data files after your computing is done. The environmental variable `TMPDIR` is also set to `/mnt/AppRun/tmp` systemwide, so temporary files from R, Julia and other programs will be dumped into this folder. (Add line `export TMPDIR="/mnt/AppRun/tmp"` to `/etc/bashrc` file.) Also add line `v /mnt/AppRun/tmp 1777 root root 10d` to `/usr/lib/tmpfiles.d/tmp.conf` to clean up the folder periodically.

* Hard drive 1 (WD Black Performance SATA): 6TB, formatted as EXT4, mounted at `/mnt/Data1`. Use this for large, permanent data sets.

* Hard drive 2 (WD Black Performance SATA): 6TB, formatted as EXT4, mounted at `/mnt/Data2`. Use this for large, permanent data sets.

* IPv4: 10.47.201.62 (eth2, 1000 Mbps).

* USB flash drives: will be mounted at `/run/media/[USERNAME]/[DRIVENAME]`, e.g., `/run/media/huazhou/SanDisk64GB`.

* Operating system: CentOS 7.

* Available Editors: vi, vim, emacs, nano, code (VS Code).

## Establish an account

Email Dr. Hua Zhou <huazhou@ucla.edu>. Upon approval, you'll receive an email with username and one-time password.  Log in and change passwood IMMEDIATELY, using Linux command `passwd`.

## Connection to the machine

- Prerequisites for connecting to the machine: 

	- If you are in CHS building and connected via ethernet cable, then `ssh [USERNAME]@10.47.201.62` should work.

	- If you want to use WIFI to access the machine, then:
		1. You need a UCLA MedNet account. 
		2. On UCLA medical campus, e.g., CHS building, you have to use the `UCLAHealthSecure` wifi, which requires your MedNet credential.  
		3. Outside medical campus, you have to use the GobalProtect VPN Client and OnGuard associated with your MedNet acccount. See
<https://mednet.uclahealth.org/device-security-toolkit/> for instructions. You need to file an IT service request at <https://mednet.uclahealth.org/it-service-catalog-quick-links/> to allow you use VPN over MedNet.

- How to connect to AW Area51?
`ssh <YourUserName>@10.47.201.62`
Using SSH keys is highly recommended. 

## R and RStudio

- RStudio Server can be accessed at <http://10.47.201.62:8787>. Log in using your account credential.

- R version is v3.6.0. Many commonly used R packages (tidyverse, shiny, etc) are already installed systemwide and available to all users. You can find installed packages by R command `installed.packages()`. You can install extra packages, which will be put in your home directory (`~/R` by default). You can also request Dr. Hua Zhou to install packages globally.

## Julia

- Julia v1.1.1, v1.3.0, and v1.4.0 is available. In bash, you can access by `julia-1.1`, `julia-1.3`, and `julia-1.4` respectively.  Currently `julia` command is aliased with Julia v1.4.

- Since v1.0, all Julia packages are installed in user home directories.

- To use Jupyter notebook or JupyterLab, install the IJulia package in Julia and `build IJulia`.

## Jupyter

- JupyterHub can be accessed at <http://10.47.201.62:8000>. Log in using your account credential.

- JupyterLab interface: <http://10.47.201.62:8000/user/[USERNAME]/lab?>.

- Jupyter notebook interface: <http://10.47.201.62:8000/user/[USERNAME]/tree>.

- Available Jupyter Notebook kernels: R, Python 2, Python 3, Julia 1.1.1, bash.

## VS Code

You can also use VS Code on your local machine (laptop or desktop) to develop code on the server. 

1. Make sure SSH key connection with the server works.

2. Install the `Remote Development extension pack` in VS Code.

3. In VS Code, Run `Remote-SSH: Connect to Host...` from the Command Palette (`F1`) and enter `[USERNAME]@10.47.201.62`.

Read <https://code.visualstudio.com/docs/remote/ssh> for details.

## KNITRO

To use Knitro (nonlinear programming software) in Julia, issue following command to install `KNITRO.jl` package.  
```
ENV["KNITRODIR"] = "/usr/local/knitro-12.2.2-Linux-64"
using Pkg
Pkg.add("KNITRO")
Pkg.build("KNITRO")
Pkg.test("KNITRO")
```
