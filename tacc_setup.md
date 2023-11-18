# TACC Setup

# Accessing TACC and Basic Commands

## Multi-Factor Authentication Pairing

Set up multi-factor authentication using a token app or SMS.

## TACC Account Portal (TAP)

Visit [www.tap.tacc.utexas.edu](https://www.tap.tacc.utexas.edu/) and use the following information:

- System: Lonestar6
- Application: DCV remote desktop
- Project: IBN22012
- Queue: vm-small, development, normal, gpu, gpu-development
- Time limit: 48:0:0 (Small and normal queues have time limits of 48h, development queue has a time limit of 2h)
- Click submit and connect once you’re off the queue
- Use Utilities to link scratch work [here](https://tap.tacc.utexas.edu/jobs/utils/)

## Mac Terminal

- Type: `ssh your_username@ls6.tacc.utexas.edu`
- Confirm with: `yes`
- Enter your TACC password
- Enter your duo mobile key code
- Get idev node before doing anything [here](https://docs.tacc.utexas.edu/software/idev/)
- Type: `idev -p NODETYPE -t TIME(h:m:s)`

## Lonestar6

- Put all working files in `/scratch`, there's no space limit
    - BUT it purges files if you don't access them for 10 days. Always back up files to `/work`.
- `/work` has a limit of 1024 GB. If work is full then transfer to `/range`.
- Type `cd` for `$HOME`, `cdw` for `$WORK`, `cds` for `$SCRATCH`.
    - In shell, type `$HOME/` and press tab, it will show you the absolute path. You need the absolute path instead of `$SCRATCH` to access directories when you transfer files from and to local computer and when you use jupyter notebook.

## Rsync for File Transfer

- `rsync -avz SOURCE DESTINATION`
- `SOURCE` is where you’re sending the file from
- `DESTINATION` is where you’re sending the file to

## Other Instructions

- No sudo, yum, apt-get or anything similar.

# Working with Anaconda in Lonestar6

## Bash File

- The bash file is located in the `home1` folder. Use `ls -a` to see it in the directory, `nano` to edit it.
    - There should be this command in the last line of the file: `conda deactivate`
- Include the Anaconda installation in the bash.
- Add directories to the PATH: `export PATH=.:$WORK/YOUR/ANACONDA/PATH:$PATH`. Do this every time you install/compile a new thing.
- Source the bashrc when you added a new path: `source ./bashrc`.

## Anaconda Installation

- Download through the web browser, making sure it’s installed under `$WORK` not `$HOME`.

### libmamba

- Install libmamba in the base environment and set it as solver. More information can be found [here](https://www.anaconda.com/blog/a-faster-conda-for-a-growing-community).

### conda-forge

- Add conda-forge in the environment channels. More information can be found [here](https://conda-forge.org/docs/user/introduction.html).

## Creating Environments and Installing minian

- Other environments: Sklearn (optional)
- Add minian and other environments as jupyter kernel: `python -m ipykernel install --user --name=EnvName`. More information can be found [here](https://minian.readthedocs.io/en/stable/start_guide/install.html#install-using-conda).

### Possible Issues running minian

- Update GCC in minian environment: `conda install -c conda-forge gcc=12.1.0`
- Downgrade pyviz: `conda install pyviz_comms==2.2.0`
- Use new version of pipeline from minian github (`minian-install --notebooks`), but remember to change the custom codes for slicing and parameter defining.
- Add to PATH`"/work/09117/xz6783/ls6/Anaconda/envs/minian/bin"`.
- Need to install [ffmpeg](https://trac.ffmpeg.org/wiki/CompilationGuide/Centos), which requires nasm and other things
- You might need to get a new load after these steps since jupyter only use the `$PATH` when it started and can’t be updated during running.

### General Downloading and Compiling Instructions

From browser: Change firefox download file to work or scratch first!!

From `wget` or `Curl`(need to compile): cd to target folder first

Unzip:

1. First download the Unzip package to work. Then compile it in the terminal (at the downloaded folder) following the instruction [here](https://www.linuxfromscratch.org/blfs/view/svn/general/unzip.html)
2. After installation, unzip the file in the terminal by running “unzip filename”
3. You can also use 7zip which might be better

# Other Resources

[https://wikis.utexas.edu/display/CoreNGSTools/Linux+fundamentals](https://wikis.utexas.edu/display/CoreNGSTools/Linux+fundamentals)

[https://issm.ess.uci.edu/trac/issm/wiki/lonestar](https://issm.ess.uci.edu/trac/issm/wiki/lonestar)

[https://filezilla-project.org/download.php?type=client](https://filezilla-project.org/download.php?type=client)

[https://www.linux.com/training-tutorials/get-know-rsync/](https://www.linux.com/training-tutorials/get-know-rsync/)

[https://wikis.utexas.edu/display/bioiteam/GVA2022+-+Class+Review#GVA2022ClassReview-Anaconda/Miniconda](https://wikis.utexas.edu/display/bioiteam/GVA2022+-+Class+Review#GVA2022ClassReview-Anaconda/Miniconda)

[https://wikis.utexas.edu/display/CoreNGSTools/Day+1%3A+Intro+to+NGS%2C+Linux+and+TACC](https://wikis.utexas.edu/display/CoreNGSTools/Day+1%3A+Intro+to+NGS%2C+Linux+and+TACC)

[https://github.com/denisecailab/minian/issues/195](https://github.com/denisecailab/minian/issues/195)

[https://tacc.github.io/ctls2017/docs/intro_to_linux/intro_to_linux_05.html](https://tacc.github.io/ctls2017/docs/intro_to_linux/intro_to_linux_05.html)

[https://tacc.github.io/ctls2017/docs/intro_to_linux/intro_to_linux_01.html](https://tacc.github.io/ctls2017/docs/intro_to_linux/intro_to_linux_01.html)

[https://sites.google.com/site/tacchadoop/home/upload-your-data-to-tacc-computer](https://sites.google.com/site/tacchadoop/home/upload-your-data-to-tacc-computer)