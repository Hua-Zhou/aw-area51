- Download CentOS 7 DVD iso image from a mirror on 
http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1810.iso

- Make bootable USB from iso image based instructions on
<https://wiki.centos.org/HowTos/InstallFromUSBkey>
On Mac terminal:
`sudo dd if=CentOS-7-x86_64-DVD-1810.iso of=/dev/disk2`
Read this post if the `dd` command incurs `dd: /dev/disk2: Resource busy` error:
https://unix.stackexchange.com/questions/271471/running-dd-why-resource-is-busy
May need to unmount the partition on USB flash drive first:
`umount /dev/disk2s1`
Took almost one hour for a USB 3.0 flash drive.

- The original plan is to keep the original SSD for Windows 10, and add another SSD (Kodak 960GB) for CentOS 7. 
    - Plug in SSD using SATA cabels (one for power one for data). 
    - In BIOS, Advanced -> Integrated Devices -> SATA Mode -> choose `AHCI` instead of `Intel Smart Response Technology` (RAID). Then the SSD will be recognized by CentOS installer.
    - To boot into Windows 10, need to change to `Intel Smart Response Technology` (RAID) again and then F12 to boot menu.
This didn't work out. Salvadore Munguia at DGIT installed the CentOS on the machine and wiped out original Windows 10.

- CentOS:
    - Installation with Gnome GUI
    - `sudo yum update` after first log-in
    - Follow the steps at <https://github.com/felipenoris/math-server-docker> to install the common tools, including R/RStudio, JupyterHub, JupyterLab, Julia
    - Add PATH for all users. Create a bash script file with a name ending in `envs.sh` in the directory `/etc/profile.d`. The file must be readable by all users and make it owned by user `root`, group `root`. Put your export statements in that file.
"""
export PATH=/usr/local/sbin:/usr/local/bin:$PATH
export PATH=/usr/local/texlive/distribution/bin/x86_64-linux:$PATH
export PATH=/usr/local/conda/anaconda3/bin:$PATH
export PATH=/usr/local/cuda-10.1/bin:$PATH
export CMAKE_ROOT=/usr/local/share/cmake-3.14
export PYTHON=/usr/local/conda/anaconda3/envs/py3/bin/python
export JULIA_PKGDIR=/usr/local/julia/share/julia/site
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib64/R/lib:/usr/local/lib:/lib:/usr/lib/jvm/jre/lib/amd64/server:/usr/lib/jvm/jre/lib/amd64:/usr/lib/jvm/java/lib/amd64:/usr/java/packages/lib/amd64:/lib:/usr/lib:/usr/local/lib:/usr/local/cuda-10.1/lib64
"""    

- To set up the JupyterHub: follow instructions at <https://github.com/jupyterhub/jupyterhub/wiki/Run-jupyterhub-as-a-system-service>. The line 
"""
c.JupyterHub.extra_log_file = '/var/log/jupyterhub.log'
"""
in the configuration file `/etc/jupyterhub/jupyterhub_config.py` is causing trouble spawning the user processes because regular user doesn't have permission on the folder. Has to be commented out.

- Install NVIDIA CUDA 10.1 <https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=CentOS&target_version=7&target_type=runfilelocal>
Need to stop X server `init 3` to install successfully, also need to disable secure boot in BIOS.

- Install CUDNN.

- Test GPU installation in Julia: `test CuArrays`.

- Install Gurobi 8.1: 
    1. Download the .tar.gz file, from Gurobi website
    2. Copy to `/opt`
    3. `sudo tar xvfz gurobi8.1.1_linux64.tar.gz`
    4. Add `export PATH=$PATH:/opt/gurobi811/linux64/bin` and `export GUROBI_HOME=/opt/gurobi811/linux64` to `/etc/profile.d/envs.sh`

- Install Mosek:
    1. Need to copy existing mosek.lic file to `~/mosek/` folder.

- Reboot hard drive paraphrase: 01011964

- To install R packages globally:  
```
R -e 'install.packages("PkgName")'
```
To set default CRAN mirror:   
```
echo 'options(repos = c(CRAN="http://cran.rstudio.com"))' >> /usr/lib64/R/library/base/R/Rprofile
```
To start RStudio server:
```
 sudo /usr/lib/rstudio-server/bin/rserver
 ```