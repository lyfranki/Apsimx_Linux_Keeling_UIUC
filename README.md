# Apsimx_Linux_Keeling_UIUC

# Keeling Crash Course
*Getting Started with the UIUC Atmospheric Science High-Performance Computing Cluster*


## Setup your Virtual Private Network (VPN) to Access the On-Campus Network
Follow the instructions outlined on the [UIUC Tech Services Page](https://techservices.illinois.edu/services/virtual-private-networking-vpn/download-and-set-up-the-vpn-client)

Download the required software, follow the instructions for logging in, then continue on! You are doing great.

---

## Logging into Keeling
You should have received an email from the School of Earth Society and Environment (SESE)
IT department with login instructions - you should be able to use your netID and password
to login.

You should secure shell (ssh) onto Keeling through a terminal (ex. ssh netid@keeling.earth.illinois.edu)
after this command, you should be prompted to enter your password. Go ahead and do that, then
continue! If you using a Windows machine, you can use [Putty](https://www.putty.org/)

---

## Installing
For installation of python on keeling we suggest using miniconda (https://docs.conda.io/en/latest/miniconda.html). This is a light version of anaconda and is less likely to give you problems later on.

1) Log onto keeling as described above with your netID
2) Use wget to download the installer
<pre><code> wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh </code></pre>
3) change the permissions to the .sh file
<pre><code> chmod u+x Miniconda3-latest-Linux-x86_64.sh </code></pre>
4) run the .sh file
<pre><code> ./Miniconda3-latest-Linux-x86_64.sh </code></pre>

The installer should spit out terms and conditions. Hit enter until it asks for 'yes' or 'no'. Type yes. Things will install from here. At the end it will ask another question. You should type 'y' to this one as well.

5) Now after it is done, we have to refresh our bash session. Run the following
<pre><code> source .bashrc </code></pre>
6) Start a bash session:
<pre><code> bash </code></pre>
7) Now lets check to see if conda is there. Run the following:
<pre><code> conda update conda </pre></code>

If it complained that there is nothing named conda, something didnt work and we will have to debug. But if it is updating something type 'y' and update conda.



### Connect to your notebook using your local machine

7) Using terminal or PowerShell, open a second ssh session to keeling, except now we will open an ssh tunnel to the compute node using the port that we used in the previous step.
```
ssh -L XXXX:127.0.0.1:XXXX netID@keeling.earth.illinois.edu ssh -L XXXX:127.0.0.1:XXXX -N netID@remotemachine
```

You’ll have to replace
  * The port number from above in 4 places
  * Your netID in two places
  * The name of the remote machine (last argument)

Note that you may have to enter your password if you don’t have the ssh key for keeling stored on your local machine.  Warnings for RSA host keys are ok.

This will set up an ssh tunnel from port xxxx (which is accessible from your web browser) to the Jupyter notebook server on keeling.  You will need to kill this ssh session (with Control-C) when you are finished, as you can’t connect again to this port without killing it explicitly.

8) Now, point a web browser to the URL that you obtained in Step 1.  You should see a Jupyter notebook that displays locally, but executes code on keeling, and has access to keeling file systems.  Note that it will not have access to your local file systems (e.g., on your Mac or PC).



---

## Official Keeling Documentation
You should check out the [documentation provided by the UIUC Atmospheric Science staff](https://wiki.illinois.edu/wiki/pages/viewpage.action?spaceKey=manabecluster&title=keeling+Home) which includes the following:
- Explanation of file structure
- Tutorial on parallel computing using Keeling
- Weather Research and Forecasting (WRF) model installation turorial


---
## Create personal environment
You need to create your personal environment. It is better to also install the package you need when creating the environment.
```
conda create -n ly-envi r-essentials r-base gdal python=3.8 gcc_linux-64 proj
```

Then acticate your personal environment

```
conda activate ly-envi
```

---
## Install ApsimX related packages in R

When installing packages in R, we may get the following error since R cannot find proj_api.h. Why R cannot ind proj_api.h is because R does not know where pkg-config is, although we have already install **proj** when creating personal envrionment. (pkg-config is a helper tool used when compiling applications and libraries.)
```
checking GDAL: checking whether PROJ is available for linking:... yes
checking GDAL: checking whether PROJ is available fur running:... yes
checking proj_api.h usability... no
checking proj_api.h presence... no
```
Doing the following in R lead you to build 'sf','rgdal'... successfully (**ly-envi** is our personal envi):
```


print(Sys.setenv(PKG_CONFIG_PATH = "/data/keeling/a/yinl3/miniconda3/envs/ly-envi/lib/pkgconfig/"))



install.packages("rgdal", repos = "http://R-Forge.R-project.org")
install.packages("sp", repos = "http://R-Forge.R-project.org")
install.packages("sf", repos = "http://R-Forge.R-project.org")
install.packages("soilDB", repos = "http://R-Forge.R-project.org")
install.packages('soilDB', dependencies = TRUE) why why dependencies???

```
When you install 'sf', you may get the following error due to without 'units' package. You should more use **conda** to install R packages rather than **install.packages()**. It is likely to cause compiling issues if using install.packages(). If you cannot find the packages you want in conda R reporitory, then try with install.pacages().

conda install -c conda-forge proj4
conda install -c conda-forge r-units    !!!!libudunits2-dev
conda install -c conda-forge libgdal
conda install -c conda-forge r-soildb
conda install -c conda-forge r-spdata

conda install -c conda-forge r-lubridate

install.packages("proj4", repos = "http://R-Forge.R-project.org")

```
ERROR: dependency 'units' is not available for package 'sf'
* removing '/data/keeling/a/yinl3/miniconda3/envs/ly-envi/lib/R/library/sf'

```

```
## Back to personal envi to install udunits2
$ conda install -c conda-forge udunits2
$ conda install r-sf
$ R
print(Sys.setenv(PKG_CONFIG_PATH = "/data/keeling/a/yinl3/miniconda3/envs/ly-envi/lib/pkgconfig/"))

install.packages('units', configure.args = c('--with-udunits2-include=/data/keeling/a/yinl3/miniconda3/envs/ly-envi/include/udunits2'))
install.packages("units", repos = "http://R-Forge.R-project.org")
install.packages("sf", repos = "http://R-Forge.R-project.org")
```

When you install 'soilDB', you may get the following error because the version of 'data.table' is too old.

```
Error: object 'merge.data.table' is not exported by 'namespace:data.table'
Execution halted
ERROR: lazy loading failed for package 'soilDB'

```
```
remove.packages("data.table")
install.packages("data.table")



install.packages("apsimx")
conda install -c conda-forge r-rsqlite
```

---
## Install Apsim Next Generation (ApsimX) GUI on Linux

Then quit R. 
The apsimx library is used to call the function in ApsimX software. So, if we want to use apsimx functions in R, we have to install it.

We need to install dotnet (. NET) firstly (. NET is a free, cross-platform, open source developer platform for building many different types of applications).
In addition, it has to been the version of 3.1 because running ApsimX will require that you have the .net runtime (3.1) installed on your system.

```
conda install -c conda-forge dotnet=3.1
conda install -c anaconda git
git clone https://github.com/APSIMInitiative/ApsimX
```

