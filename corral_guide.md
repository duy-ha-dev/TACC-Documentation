# Corral Guide

# Contents

- File Transfer between Corral and TACC Server
    - Preliminaries
    - How to Use rsync for File Transfer
    - Alternatives to rsync (not recommended)
    - Cyberduck for Graphical File Transfer
- Setting up Corral for Projects
    - E.g. Minian analysis
    - Handling Conflicts

# File Transfer between Corral and TACC Server

## Preliminaries

1. **Allocation Path**: Determine the path to your project directory on Corral which was provided to you with your allocation.
    1. In this case, it would be Corral project directory: `/corral/uth/IBN22012`
2. **Local Path**: Know the local path to the files or directories you want to transfer.
    1. In TACC, the preferred location is generally `$SCRATCH`
3. **Setting Read Write Permissions**: 
First, set umask to 002: umask 022. Check the umask value by simply typing umask`
DO THIS BEFORE EACH FILE TRANSFER: `chmod -R g+rw filepath`
- e.g. setting all files under scratch `cd $SCRATCH` -> `chmod -R g+rw *` 

## How to Use rsync for File Transfer

rsync is the preferred method for file transfer as it generally has better performance.

**Single File Transfer to TACC:**

- `rsync -u /local/path/to/filename /path/to/project/directory`

**Directory Transfer to TACC:**

To transfer a whole directory and its contents, add the `-r` (recursive) flag:

- `rsync -ru /local/path/to/directory /path/to/project/directory`

**(Preferred)** **Transfer with Progress and Compression:**

You can also use the `-avz` flags to show detailed file names during transfer where the `-z` flag to enable compression:

- `rsync -auvz /local/path/to/filename /path/to/project/directory`

If you are transferring a large amount of files and you want to see the percentage of files transferred rather than the whole list of files, you can also use the `--info=progress2 --info=name0` flags to show progress:
- `rsync -auW --info=progress2 --info=name0 /local/path/to/filename /path/to/project/directory` for initial trasnfer
- `rsync -auz --info=progress2 --info=name0 /local/path/to/filename /path/to/project/directory` for subsequent update
When doing so make sure your local folder shares the same structure as the target folder.

### Alternatives to rsync (not recommended)

**Transfer Using SCP (Secure Copy Protocol):**

- **Single File Transfer to TACC**:
    - `scp /local/path/to/filename username@data.tacc.utexas.edu:/path/to/project/directory`
- **Directory Transfer to TACC**:
    - `scp -r /local/path/to/directory username@data.tacc.utexas.edu:/path/to/project/directory`
- **Single File Transfer from TACC**:
    - `scp username@data.tacc.utexas.edu:/path/to/project/directory/filename /local/path/to/directory`

**Staging to and from Lonestar 6 using cp (copy)**

- *Potentially helpful for performing I/O intensive computational and analysis tasks on Lonestar6 (such as the case for our Minian video analysis)*
- **Staging Files to Lonestar6**:
    - `cp /corral-repl/uth/IBN22012/myfile $SCRATCH/job_directory/`
- **Staging Directories to Lonestar6**:
    - `cp -r /corral-repl/uth/IBN22012/job_directory $SCRATCH/`
- **Copying Output Back to Corral**:
    - After job completion, copy the results back to Corral.
    - `cp -r $SCRATCH/job_directory/output_files /corral-repl/uth/IBN22012/job_directory`

---

## Cyberduck for Graphical File Transfer

Cyberduck is also used for transferring files between your local system and remote servers such as TACC's Lonestar server. Download [here](https://cyberduck.io/download/) and follow the procedures below:

1. Open Cyberduck and click the "Open Connection" button in the top-right corner of the window to bring up the connection configuration window.
2. From the dropdown menu at the top of the window, select the SFTP (SSH File Transfer Protocol) option.
3. In the "Server" field, enter `data.tacc.utexas.edu` to connect to TACC.
4. Enter your TACC username and password in the corresponding fields.
5. If the "More Options" area is not visible, click the small triangle or button to expand the window.
6. In the "Path" field, you can directly enter the path to your project area (e.g., `/corral/uth/IBN22012`) so that when Cyberduck connects, it will take you directly to your data.
7. Click the "Connect" button to initiate the connection.

---

# Setting up Corral for Projects

## E.g. Minian analysis

1. Transfer Minian data to Corral project directory
2. When performing analysis, stage desired directory from Corral to Lonestar6’s $SCRATCH or $WORK
    1. `rsync -avz -r /corral-repl/utexas/myproject/job_directory $SCRATCH/`
3. Once analysis is done, copy the outback back to Corral
    1. `rsync -avz -r $SCRATCH/job_directory/output_files /corral-repl/utexas/myproject/job_directory`

### Handling Conflicts

- As Corral is just a database server, simultaneously working on the same files may be tricky, but here are some simple tips:
    - It is possible to use version control systems like Git to manage changes and merge conflicts on **small** projects.
        - **Initialize a New Repository**:
            - Open the CLI and navigate to your project directory.
            - Run `git init` to initialize a new Git repository.
        - **Set Up Remote Repository**:
            - Create a repository on GitHub.
            - Link your local repository to the remote with `git remote add origin <REMOTE_URL>`.
    - Regularly communicate with team members to coordinate who works on what.
        - If it is unavoidable for multiple people to work on the same file, always output back to corral the filename plus an extension e.g. `projectTitle_myName.py`
