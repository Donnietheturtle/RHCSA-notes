# Understand and use essential tools
#### Access a shell prompt and issue commands with correct syntax
- Use **who** or **w** commands to view sessions logged in
- **Ctrl-a**: Move cursor to beginning of the line
- **Ctrl-e**: Move cursor to end of the line
- **Ctrl-r**: Reverse search in command history
- **Ctrl-l**: Clear screen
- **Ctrl-u**: Removes current line
- **Alt-b**: Move cursor one word backward
- **Alt-f** moves cursor one word forward
- **!3**: Runs 3rd command from history list

#### Use input-output redirection (>, >>, |, 2>, etc.)
- **STDIN (Same as 0<)**: cat < /etc/redhat-release
- **STDOUT (Same as 1>)**: ls > temp.txt
- **Append text**: echo "test" >> temp.txt
- **Pipe command**: cat /etc/passwd | cut -d ":" -f 1 | sort
- **STDERR**: find / -name "*hosts*" 2>/dev/null
  - ls /usr /cdr > temp.txt 2> error.txt (Redirects STDOUT to temp.txt and STDERR to error.txt)
    - ls vbsfsd > errout 2>&1 (Redirects STDERR to the same destination as STDOUT)

#### Use grep and regular expressions to analyze text
- man -k passwd | grep 8
- grep ^bob /etc/passwd (Shows lines which start with bob)
- grep $ash /etc/passwd (Shows lines which end with ash)
- grep -v '^#' /etc/services -B 5 (Shows lines which do not start with a # sign, but also the five lines directly before each of those lines)
- grep -v -e '^#' -e '^$' /etc/service (Excludes all blank lines and lines which start with #)
- awk -F : '/user/ {print $4}' /etc/passwd (Searches the /etc/passwd for the text *user* and will print the fourth field of any matching line) 
- sed -n 5p /etc/passwd (Print the fifth line from the /etc/passwd file)
- sed -i s/old-text/new-text/g ~/myfile (Replaces all occurrences of old-text with new-text in ~/myfile; -i writes result to file)
- sed -i -e '2d;20,25d' ~/myfile (Deletes lines 2 and 20-25 in ~/myfile)

#### Access remote systems using SSH
- systemctl status sshd (Check sshd process status)
- Public key fingerprints stored in ~/.ssh/known_hosts on server
- Use ssh -Y user@server to start graphical applications (Note: X server must be running on the client and the remote host must be allowed to display screens on the local client)
	- Enable X forwarding be default: Add **ForwardX11 yes** to /etc/ssh/ssh_config
- Key-based authentication: ssh-copy-id server > copies ~/.ssh/id_rsa.pub to ~/.ssh/authorized_keys on the hosts
- SFTP: sftp user@server
- Rsync: rsync [OPTION...] SRC... [DEST]

#### Log in and switch users in multiuser targets
- **su - user** : Login as a different user
- **su -** : Prompt for root password to login 
- **sudo -i** : Login as root for authorized users only and runs the shell specified by the target user's password database entry as a login shell 
- Virtual consoles **Ctrl-Alt-F[1-6]** in a GUI enviroment or **chvt #** to change between them
- F1: Access to the GNOME DIsplay Manager graphical login
- F2: Access to the current graphical console
- F3: Gives access back to the current graphical session
- F4-F6: Gives access to nongraphical consoles 

#### Archive, compress, unpack, and uncompress files using tar, gzip, and bzip2
- Create archive: tar -cf archive.tar /files you want to archive 
- Add file to archive: tar -**r**vf /root/homes.tar /etc/hosts
- View contents of tar file: tar -tvf /root/homes.tar
- Extract archive contents: tar -**x**vf /root/homes.tar
	- Extract single file to /tmp directory: tar -xvf /root/homes/tar -C /tmp etc/hosts
- Archive using gzip and bzip2: 
	- gzip: tar -c**z**f file.tar /home
	- bzip2: tar -c**j**f file.tar /home

#### Create and edit text files
- Vim commands:
    - **i**: Switch to input mode at the cursor
    - **a**: Switch to input mode after the cursor
    - **o**: Opens a new line below the cursor and enters input mode
    - **:wq**: Writes the current file and quits
    - **:q!** Quits the file without applying changes
    - **:w filename**: Writes the current file with a new filename
    - **dd**: Deltese the current line and places the contents into memory
    - **yy**: Copies the current line
    - **p**: Pastes the content
    - **v**: Enters visual mode
    - **u**: Undoes the last command
    - **Ctrl-r**: Redoes the last undo
    - **gg**: Moves cursor to first line in the file
    - **G**: Moves cursor to the last line in the file
    - **/text**: Searches for text from the current cursors position forward
    - **?text**: Searches for text from the current cursors position backward
    - **^**: Goes to the first position in the current line
    - **$**: Goes to the last position in the current line
    - **!ls**: Adds the output of ls (or any other command) in the current file
    - **:%s/old/new/g**: Replaces all occurrences of old with new

#### Create, delete, copy, and move files and directories
- Wildcards:
	- \*: Refers to unlimited numbers of characters (EX: ls user*)
	- **?**: Refers to one character (EX: ls c?t)
	- **[auo]**: Refers to one character inside of the brackets (EX: ls c[auo]t)
- mkdir directory
- rm -r files
- cp [path-to-file] [path-to-destination]
	- By default hidden files are not copied when using cp command
		- cp /somedir/.* /tmp: Copies all files which have a name starting with a dot (Need to use -R if you want to copy hidden directories)
		- cp -a /somedir/ . : Copies the entire directory including its contents to the current directory
		- cp -a /somedir/. . : Copies all files including hidden ones to the current directory
- mv [file] [directory]

#### Create hard and soft links
- **Hard links**: Every file always has one hard link, which is the name of the file 
	- If one hard link is removed the other one is not effected
	- Allows you to keep the contents the same for two files in multiple locations
	- Can not create hard links to directories
	- EX: ln /etc/hosts /tmp
- **Soft links (Symbolic links)**: 
	- Link to the name of the file, but not directly to the inode
	- If the original file is removed, the symbolic link is invalid
	- Can link to directories and files on other devices
	- EX: ln -s /home /tmp

#### List, set, and change standard ugo/rwx permissions 
- Permission: Numeric Representation
    - Read: 4
    - Write: 2
    - Execute: 1
- chmod 744 file
- chmod g+w,o-r file
- Apply subdirectories recursively: Use (uppercase X) chmod -R a+**X** files instead of chmod -R a+x

#### Locate, read, and use system documentation including man, info, and files in /usr/share/doc
- Use **--help** when you want a quick overview of the command
- Use **apropos** or **man -k** to search through the short summaries of the mandb database
- Man pages categorization:
    - 1: Executable programs or shell commands
    - 5: File formats and conventions
    - 8: System administration commands
- Update mandb database with **sudo mandb**
- **Info or pinfo**:
    - Some commands have the main documentation in the info system
    - Pinfo is easier to parse information
- **/usr/share/doc**: Used for services and more complicated systems such as rsyslog, bind, Kerberos, and OpenSSL 
