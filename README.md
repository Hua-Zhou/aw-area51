# User Guide

## System information

* Chassis: Alienware Area51 R5. Full specs: <https://www.dell.com/support/manuals/us/en/04/alienware-area51-r4/alienwarearea51r5_setupandspecs/specifications?guid=guid-2795f926-e3d3-4d85-9813-11a63248dabb&lang=en-us>

* CPU: Intel i9 9920X (12 core, 12.3MB L2 Cache, 19.7MB L3 Cache, 3.5 GHz up to 4.5GHz with Intel Turbo Boost Max 3.0). Full specs: <https://ark.intel.com/content/www/us/en/ark/products/189127/intel-core-i9-9920x-x-series-processor-19-25m-cache-up-to-4-50-ghz.html>.

* GPU: NVIDIA GeForce RTX 2080 Ti OC with 11GB GDDR6 (14 Gbps), 4352 cores, SP 13.4 TFLOPs, INT4 430TOPs. Full specs: <https://www.nvidia.com/en-us/geforce/graphics-cards/rtx-2080-ti/>.

* Memory: 64GB DDR4 SDRAM 2666 MHz (4 slots of 16GB).

* SSD 1 (Kingston): 512GB PCIe NVMe M.2 SSD, CentOS 7 bootable.

* SSD 2 (Kodak): 920GB SATA III SSD, mounted on `/mnt/AppRun`. This drive serves several purposes.  
	- MATLAB is installed at `/mnt/AppRun/MATLAB`.  
	- MATLAB 2020a installer is available at `/mnt/AppRun/matlab_R2020a_glnxa64`. 
	- Use `/mnt/AppRun` as a scratch space for big data computation (ephemeral). Make sure to delete data files after your computing is done. 
	- The environmental variable `TMPDIR` is set to `/mnt/AppRun/tmp` systemwide, so temporary files from R, Julia and other programs will be dumped into this folder. (Add line `export TMPDIR="/mnt/AppRun/tmp"` to `/etc/bashrc` file.) Also add line `v /mnt/AppRun/tmp 1777 root root 10d` to `/usr/lib/tmpfiles.d/tmp.conf` to clean up the folder periodically.

* Hard drive 1 (WD Black Performance SATA): 6TB, formatted as EXT4, mounted at `/mnt/Data1`. Use this for large, permanent data sets.

* Hard drive 2 (WD Black Performance SATA): 6TB, formatted as EXT4, mounted at `/mnt/Data2`. Use this for large, permanent data sets.

* IPv4: 172.21.99.196 (eth2, 1000 Mbps).

* USB flash drives: will be mounted at `/run/media/[USERNAME]/[DRIVENAME]`, e.g., `/run/media/huazhou/SanDisk64GB`.

* Operating system: CentOS 7.

* Available Editors: vi, vim, emacs, nano, code (VS Code).

## Establish an account

Email Dr. Hua Zhou <huazhou@ucla.edu>. Upon approval, you'll receive an email with username and one-time password.  Log in and change passwood IMMEDIATELY, using Linux command `passwd`.

## Connection to the machine

- Prerequisites for connecting to the machine: 

	- If you are in CHS building and connected via Ethernet cable, then `ssh [USERNAME]@172.21.99.196` should work.

	- If you want to use WIFI to access the machine, then:
		1. You need a UCLA MedNet account. 
		2. On UCLA medical campus, e.g., CHS building, you have to use the `UCLAHealthSecure` wifi, which requires your MedNet credential.  
		3. Outside medical campus, you have to use the GobalProtect VPN Client and OnGuard associated with your MedNet acccount. See
<https://mednet.uclahealth.org/device-security-toolkit/> for instructions. You need to file an IT service request at <https://mednet.uclahealth.org/it-service-catalog-quick-links/> to allow you use VPN over MedNet.

- How to connect to AW Area51?
`ssh <USERNAME>@172.21.99.196`
Using SSH keys is highly recommended. 

## R and RStudio

- RStudio Server can be accessed at <http://172.21.99.196:8787>. Log in using your account credential.

- R version is v3.6.0. Many commonly used R packages (tidyverse, shiny, etc) are already installed systemwide and available to all users. You can find installed packages by R command `installed.packages()`. You can install extra packages, which will be put in your home directory (`~/R` by default). You can also request Dr. Hua Zhou to install packages globally.

## Julia

- Julia v1.1.1, v1.3.0, v1.4.0, v1.5.0, and v1.6.0 are available. In bash, you can access by `julia-1.1`, `julia-1.3`, `julia-1.4`, `julia-1.5`, and `julia-1.6` respectively.  Currently `julia` command is aliased with Julia v1.6.0.

- Since v1.0, all Julia packages are installed in user home directories.

- To use Jupyter notebook or JupyterLab, install the IJulia package in Julia and `build IJulia`.

## Jupyter

- JupyterHub can be accessed at <http://172.21.99.196:8000>. Log in using your account credential.

- JupyterLab interface: <http://172.21.99.196:8000/user/[USERNAME]/lab?>.

- Jupyter notebook interface: <http://172.21.99.196:8000/user/[USERNAME]/tree>.

- Available Jupyter Notebook kernels: R, Python 2, Python 3, Julia 1.1.1, bash.

## VS Code

You can also use VS Code on your local machine (laptop or desktop) to develop code on the server. 

1. Make sure SSH key connection with the server works.

2. Install the `Remote Development extension pack` in VS Code.

3. In VS Code, Run `Remote-SSH: Connect to Host...` from the Command Palette (`F1`) and enter `[USERNAME]@172.21.99.196`.

Read <https://code.visualstudio.com/docs/remote/ssh> for details.

## Matlab

- Matlab is installed only for user `huazhou` under UCLA individual license. The install location is `/mnt/AppRun/MATLAB` with a symbolic link created `/usr/local/bin/matlab`.

- If you want to use Matlab on this machine, follow these steps.  
	- Follow instructions at <https://softwarecentral.ucla.edu/matlab-getmatlab> to sign up an account at UCLA MATLAB portal using your `ucla.edu` email. 
	- Run the installer `/mnt/AppRun/matlab_R2020a_glnxa64/install`. This installation program requires GUI. So you need to enable X11 forwarding for SSH. During installation, make sure to use your own UCLA Matlab account credential. 

## KNITRO

To use Knitro (nonlinear programming software) in Julia, first copy the license into your home directory. In terminal (bash),
```
cp /usr/local/knitro-12.3.0-Linux-64/artelys_lic_3871_UCLABioStat_2021-04-09_knitro_64-7a-f6-75-54.txt ~
```
Then install the `KNITRO.jl` package in Julia
```
ENV["KNITRODIR"] = "/usr/local/knitro-12.3.0-Linux-64"
using Pkg
Pkg.add("KNITRO")
Pkg.build("KNITRO")
Pkg.test("KNITRO")
```

## UK Biobank

UK biobank data is avalable at `/mnt/ukbiobank_wdeasystore_14tb` with content  
```
74G	/mnt/ukbiobank_wdeasystore_14tb/accelerometer/accelerometer_data
2.5M	/mnt/ukbiobank_wdeasystore_14tb/accelerometer/idlists
74G	/mnt/ukbiobank_wdeasystore_14tb/accelerometer
2.3T	/mnt/ukbiobank_wdeasystore_14tb/bulkexomevcf
4.5T	/mnt/ukbiobank_wdeasystore_14tb/cnv
8.5K	/mnt/ukbiobank_wdeasystore_14tb/dropoutfilter
105G	/mnt/ukbiobank_wdeasystore_14tb/exome
93G	/mnt/ukbiobank_wdeasystore_14tb/genotype
51G	/mnt/ukbiobank_wdeasystore_14tb/haplotype
2.4T	/mnt/ukbiobank_wdeasystore_14tb/imputed
7.4G	/mnt/ukbiobank_wdeasystore_14tb/phenotype/UKBiobankRefresh
61G	/mnt/ukbiobank_wdeasystore_14tb/phenotype
9.4T	/mnt/ukbiobank_wdeasystore_14tb/
```
NEVER write to this external hard drive. For fast computing, copy relevant data to `/mnt/AppRun` (SSD) or `/mnt/Data2` (internal hard drive). 

For researchers in Dr. Hua Zhou's group, UK Biobank data is also available on [Hoffman2 cluster](https://github.com/chris-german/Hoffman2Tutorials) at `/u/project/huas/kose/ukbdata/`. You need to apply for permission by emailing to Dr. Hua Zhou first.

## Databases

- MySQL v8 is installed.
