# run_in_hpc
Examples and tutorials for running your Python based code in High Power Computing (HPC) clusters

# Workflow for running a Python script
![](https://img.shields.io/badge/Python-3.10.8-181717?style=for-the-badge&logo=python) 
![](https://img.shields.io/badge/Conda-4.12.0-red?style=for-the-badge&logo=anaconda) 

## Logging into DelftBlue
Enter the following command in terminal (Mac OS) or in the command prompt (Windows).
```bash
ssh <netid>@login.delftblue.tudelft.nl
```
Press Enter, you will be asked to enter your NetID password:
```bash
<netid>@login.delftblue.tudelft.nl's password:
```
Please note, that you will not see any characters appearing on the screen as you type your password. This is normal, and is designed to increase security, so that people watching over your shoulder don't even know how many characters your password contains. Once you typed in your password, press Enter again. You should see the following:
```bash
    ____       ________  ____  __
   / __ \___  / / __/ /_/ __ )/ /_  _____
  / / / / _ \/ / /_/ __/ __  / / / / / _ \
 / /_/ /  __/ / __/ /_/ /_/ / / /_/ /  __/
/_____/\___/_/_/  \__/_____/_/\__,_/\___/

As DelftBlue is a new system and was newly installed and configured, some things might not be fully working yet, and are still in the process of being set up.

For information about using DelftBlue, see the documentation: https://www.tudelft.nl/dhpc/documentation (login using your TU Delft account)
When you have questions, you can ask them in the DHPC chat service: https://mattermost.tudelft.nl/dhpc/ (login using your TU Delft account)

Last login: Thu Jul 21 16:56:39 2022 from 145.90.36.181


Quota information for storage pool scratch (ID: 1):

      user/group     ||           size          ||    chunk files
     name     |  id  ||    used    |    hard    ||  used   |  hard
--------------|------||------------|------------||---------|---------
      <netid>|588559||   20.63 GiB|    5.00 TiB||   278772|  1000000

Quota information for storage pool home (ID: 2):

      user/group     ||           size          ||    chunk files
     name     |  id  ||    used    |    hard    ||  used   |  hard
--------------|------||------------|------------||---------|---------
      <netid>|588559||    3.44 GiB|    8.00 GiB||    48433|  1000000

 11:54:16 up 48 days, 19:22,  6 users,  load average: 0.09, 0.20, 0.15

 [<netid>@login04 ~]$
```
If you want to avoid entering password everytime you log in into DelftBlue, you can set up **SSH-key** generation by following this [website](https://kb.n0c.com/en/knowledge-base/how-to-create-an-ssh-key-and-connect-to-an-account/), and choose the correct operating system.
> **Note** This step is important and needs to be done only in ur private system, so that you can always connect to your remote server without authentication

# Solving Optimization using Pyomo and GUROBI
![](https://img.shields.io/badge/Python-3.10.8-181717?style=for-the-badge&logo=python) 
![](https://img.shields.io/badge/Conda-4.12.0-red?style=for-the-badge&logo=anaconda) 
![](https://img.shields.io/badge/Pyomo-6.4.3-yellow?style=for-the-badge&logo=)
![](https://img.shields.io/badge/Gurobi-9.5.2-EE3524?style=for-the-badge&logo=Gurobi) 


## Installing Gurobi (Using executable)
Download the gurobi software by logging in from: https://www.gurobi.com/downloads/gurobi-software/ in your **local computer**. Lets say you have it downloaded in the directory: /Users/matrix/Desktop/Downloads. The downloaded file will have a name: *tar xvfz gurobix.x.x_linux64.tar.gz*

> **Note** Guribi version 10.0.0 has a bug for Linux system where it cannot derive the host ids during installations. The following tutorial is tested with gurobi=9.5.2
#
**Copying the downloaded file to DelftBlue**
> **Note** Your next step is to choose a destination directory. I recommend /opt for a shared installation, but other directories will work as well.
Create a directory named opt in the HOME of Delftblue:
```
mkdir opt
```
Follow the following steps in the terminal/ bash of your local computer:
```
cd  /Users/matrix/Desktop/Downloads
rsync -v gurobix.x.x_linux64.tar.gz netid@login.delftblue.tudelft.nl:/home/netid/opt
```

Copy the Gurobi distribution to the destination directory and extract the contents. Extraction is done with the following command

```
tar xvfz gurobix.x.x_linux64.tar.gz
```

Make sure you replace x.x.x by the actual version name visible in the downloaded file.
This command will create a sub-directory /opt/gurobixxx/linux64 that contains the complete Gurobi distribution (assuming you chose /opt). Your *installdir* (which we'll refer to throughout this document) will be /opt/gurobixxx/linux64.

**Setting up Path variables**

The Gurobi Optimizer makes use of several executable files. In order to allow these files to be found when needed, you will have to modify a few environment variables:

* GUROBI_HOME should point to your *installdir*.
* PATH should be extended to include *installdir*/bin.
* LD_LIBRARY_PATH should be extended to include *installdir*/lib.

Users of the bash shell should add the following lines to their .bashrc files located in their HOME. This can be done in the Delftblue using ![](https://img.shields.io/badge/Vim-blue?style=for-the-badge&logo=Vim) editorby follwing commands:

```
cd
vim .bashrc
```
  
You will enter the Vim editor. Type i and you will go into **insert** mode. Now after the first line type the following:

```
export GUROBI_HOME="/opt/gurobi901/linux64"
export PATH="${PATH}:${GUROBI_HOME}/bin"
export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:${GUROBI_HOME}/lib"
```
  
Press **esc** key and then exit the editor by typing **:wq:** enter.


> **Note** More information abailable at: https://www.gurobi.com/documentation/9.0/quickstart_linux/software_installation_guid.html

**Connecting Gurobi installations to Python**
You need to run a script **setup.py** available with the gurobi installation files. For it follow the commands:
```
cd /home/netid/opt/gurobi952/linux64
python setup.py
```

### Installng Gurobi (Using conda) (Recomended)
  
```bash
module load miniconda3
conda env create "new_env" python=3.10.8
conda activate "new_env"
conda install -c gurobi=9.5.2 gurobi
```
  
 ## Installing Pyomo
 Install Pyomo using Conda in the Delftblue terminal.
 ```
 conda activate "new_env"
 conda install -c conda-forge pyomo
 ```
  
 ## Installing License
 On the terminal of Delftblue:
  ```
  touch gurobi.lic
  vim gurobi.lic
 ```
  In the gurobi.lic file, type the following:
  ```
  TOKENSERVER=flexserv-x1.tudelft.nl
  PORT=27099
  ```
Now your Gurobi installation setup is ready. Time for testing.
  ## Testing the installations
  
Create a test.py script in your HOME directory using:
```
vim test.py
```
Now type the code available in [opt_test.py](opt_test.py)
Save and quit Vim editor by typing- ":wq", hit enter. Run the file using:
```
module load miniconda3
conda activate "new_env"
python test.py
```

Remember to replace "new_env" by the actual name of the evironment. If the installations and license is correct you will get the following output in your Delftblue terminal:
```
>> Optimisation problem solved sucessfully

>> *** Solution *** :
>> x: 100.0
>> y: -29.0
```
> **Note** The code will take around 5 mins to run, so be patient
If it does not show any output or shows some error, you need to check if all the previous steps are correctly followed. For any questions feel free to contact [me](https://github.com/nkpanda97)
