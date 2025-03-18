# Understand and use essential tools
#### Access a shell prompt and issue commands with correct syntax
   - Virtual terminals are accessible using **Ctrl-Alt-Fn[2-6]** in a GUI enviroment or **chvt #** to change between them
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
