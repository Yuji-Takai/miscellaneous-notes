## Commands
- `cd ~/`: go to home directory
- `pwd` : print current directory 
- `ls -al`: lists all of the files in the directory with read-write access permissions, author, last edit date and file name
- `ls -a` -> lists all of the files including those starting from period
- `sh FILE_NAME.sh`: run sh files 

## Viewer
- `view FILE_NAME`: open the viewer and view the file
- Hit `:q` to escape when no edit 
- `:q!`: force quit when inserted but not want to save 

## ssh config setup
1. `cd ~/`: go to home
2. `cd .ssh`
3. `vi config`: write the config file for ssh 
    - write params like command name and user names and server names to eliminate the need to type them in manually every time
4. `cd ..`: go back to home directory
    ```
    # Example
    ssh login-stg
    ```