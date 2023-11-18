# RClone Setup

### Installation

1. Installing Go:
    1. Within **$WORK** directory:
        - Download tar file: [https://go.dev/doc/install](https://go.dev/doc/install) into **$WORK**
        - `tar -C . -xzf go1.21.1.linux-amd64.tar.gz`
            - If there is a previous Go installation:
                - `rm -rf ./go && tar -C . -xzf go1.21.1.linux-amd64.tar.gz`
        - `export PATH=$PATH:./go/bin`
        - Check that Go is installed: `go version`
2. Install RClone using the following 
    1. (NOTE: build using your own node by running: `idev -p vm-small`)
    
    ```bash
    git clone https://github.com/rclone/rclone.git
    cd rclone
    go build
    ```
    
3. Add to **$PATH**:
    1. In terminal and also ~.bashrc:
        
        ```bash
        export PATH=$PATH:$WORK/rclone
        ```
        
    

### RClone Configuration

- This setup should work for any external cloud storage; the example below will be for Google Drive
1. Run rclone config in **DCV remote desktop**
    
    ```bash
    rclone config
    ```
    
2. Proceed with configuration (refer to the link below)
    1. Follow steps 2-5: [https://itsfoss.com/use-onedrive-linux-rclone/](https://itsfoss.com/use-onedrive-linux-rclone/)
3. Creating a fake fusermount3 command
    1. This step is required only if the system does not have fusermount3 installed
    2. In **$WORK** create a fake fusermount3:
        
        ```bash
        mkdir fake_fusermount3
        nano fusermount3
        # Save the empty file and quit
        ```
        
    3. Creating the symlink (Creates a reference redirect to source fusermount, which 1. the Lonestar system has and 2. is compatible with rclone)
        
        ```bash
        ln -s /usr/bin/fusermount $WORK/fake_fusermount/fusermount
        ```
        
    4. Add to **$PATH** (add to terminal and ~/.bashrc):
        
        ```bash
        export PATH=$PATH:$WORK/fake_fusermount3/
        ```
        

### Mounting Cloud FS to Local Folder

*(This is NOT an On-Startup solution; so do this every time)*

- For a DCV Remote Desktop and Jupyter Notebook session
    - Make sure a directory is made in **$SCRATCH** to contain files
    - Open a new terminal
    - `rclone --vfs-cache-mode writes mount "[name of drive set during rclone config]": [directory in $SCRATCH]`
    - Do not close the terminal
- ******TIP******: Once local folder is completed mounted, perform an `rsync` to save into a separate local folder to limit the number of times you would need to mount on-startup