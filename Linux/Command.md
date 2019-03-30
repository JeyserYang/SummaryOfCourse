CLI:        Command Line Interface.

**In fact, All Commands can be found in directory(/bin). They are process(进程) from OS perspective and similar to the**

**file .exe on Windows.**


#### Simple Command


```
man:        man is the system's manual pager.  each page argument given to man  is normally  the  name of a program,

            utility or function.

----------------'man' command is the most significant.

ls:         List  information  about the FILEs (the current directory by default).
            
            Sort entries alphabetically if none of -cftuvSUX nor --sort is  specified.

hostname:   Hostname is used to display the system's DNS name, and  to  display  or set its hostname or NIS domain 
name.

--help:     like 'man'.

cd:         Change the shell working directory. Change the current directory to DIR.  The default DIR is the value of

            the HOME shell variable.

dir:        list directory contents.

pwd:        print name of current/working directory.

touch:      change file timestamps. A FILE argument that does not exist is created empty, unless -c  or  -h is supplied.  （touch命令不常用，用来更改文件或目录的日期时间，包括存取时间和更改时间）

cp:         Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

ln:         Create hard links by default, symbolic links  with  --symbolic. hardlink is a same file with previous

             file. symboliclink is a different file with previous file. you can see their inode number by using 'ls -i'.

mv:         Rename SOURCE to DEST, or move SOURCE(s) to DIRECTORY.

rm:         remove files or directories,By default, it does not remove directories.
        
mkdir:      Create the DIRECTORY(ies), if they do not already exist.

rmdir:      Remove the DIRECTORY(ies), if they are empty.

file:       determine file type.（查看文件类型，类似于Windows上通过扩展名来获得文件类型）

cat:        concatenate files and print on the standard output.

more:       file perusal filter for crt viewing. Users should realize that less(1)  provides more(1) emulation plus

            extensive enhancements.

tail:       Print  the  last  10  lines of each FILE to standard output.

head:       Print  the  front 10  lines of each File to standard output.
```

#### A little Complex Command

```
ps:        report a snapshot of the current processes.
           ps displays information about a selection of the active processes.  If
           you want a repetitive update of the selection and the displayed
           information, use top(1) instead.

top:       The  top program provides a dynamic real-time view of a running system.
           It can display system summary information as well as  a  list  of  pro‐
           cesses  or  threads  currently  being managed by the Linux kernel.
           (等同于Windows的任务管理器中的进程显示)

kill:      The  default  signal  for kill is TERM.  Use -l or -L to list available
           signals.  Particularly useful signals include  HUP,  INT,  KILL,  STOP, CONT,  and  0. 

---------------- The command should be care for using.

df:        df displays the amount of disk space available on the file system containing each  filename  argument.

du:        Summarize disk usage of the set of FILEs, recursively for directories.

sort:      sort lines of text files.

grep, egrep, fgrep, rgrep:      print lines matching a pattern for searching a data.

gzip, gunzip, zcat:             compress or expand files.

tar:       an archiving utility.

----------------This command will be usually used.

sleep:     delay for a specified amount of time.

history:   display the history command that you input.It's maxSize is 1000 lines. you can also see them in hide file 

           '.bash_history'.   

printenv:  Print the values of the specified environment VARIABLE(s).

env:       like 'printenv', It has more function than 'printenv'.
```



#### Standard Use

```
1.   hostname  -  i:            Display the network address(es) of the host name.
     Sudo hostname yang         set its hostname as yang.
     Hostname:                  display the host name.

2:   man ls:                    Display the manual page for the item (program) ls.
     man man.7                  Display the manual page for macro package man from section 7.

3:   cd /:                      return to the root directory.
     cd:                        return to the home directory.

4:   ls - - color  || F:        colorize  the output.
     ls -a:                     open all files,include the hidden file.
     ls -l:                     Display the number of block on the first line.
                                Display the specific information of each file or directory on the next
                                every line.   Include:
                                file type  such as  directory(d), file(-), char device(c), block device(b).
                                file authority
                                the number of file’s hardlink.
                                Username.
                                Groupname.
                                File size.
                                the last modification time of file
                                file name or directory name.

5:   rm -r:                     remove directories and their contents recursively.
     rm -i:                     prompt before every removal, 
                                you should always add ‘i’ when you would like to remove file.
     rm -f:                     force to delete the file.

6:   cat -n:                    number all output lines.    This is a useful command.

7:   tail -n:                   output the last NUM lines, instead of the last  10;  or  use  -n
                                +NUM to output starting with line NUM. 
                                This ‘-n’ is’t character n,  it is a specific number.

8:   ps – ef:                   To see every process on the system using standard syntax: 

     ps -- forest:              ASCII art process tree. It can display the relationship between process and parent.

9:   df -h:                     human-readable   print sizes in powers of 1024

10:  du -chs:                   display the total size of the directory.

11:  sort -n:                   compare according to string numerical value.

     sort -M:                   compare (unknown) < 'JAN' < ... < 'DEC'.

     sort -r:                   reverse the result of comparisons.

12:  grep -e:                   If this  option  is  used  multiple times or is combined with the -f (--file) option, 

                                search for all patterns given.

13:  tar -cvf a.tar /test       archive the all files under ‘/test’ to a.tar.

     tar -xvf a.tar:            compress the all files from a.tar.

     tar -zxvf a.tgz:           compress the .tgz type file.

14:  type -a echo:              display the all implement about ‘echo’ command.

15:  PATH=$PATH:/home/yangweijie/               create a PATH environment variable. It will apply for one time.

```

#### Operation Display

```
cat /etc/passwd
    [......]
    yangweijie:x:1000:1000:Ubuntu-17,,,:/home/yangweijie:/bin/bash

ls -lF /bin/sh
    lrwxrwxrwx 1 root root 4 Dec 31 18:53 /bin/sh -> dash*

--------The user default shell is bash shell. System default shell is dash shell.you    
    can use the command '/bin/dash' to start the dash shell.input 'exit' to stop.


ps -f
    UID         PID   PPID  C STIME TTY          TIME CMD
    yangwei+   2418   1538  0 14:24 pts/0    00:00:00 bash
    yangwei+   2434   2418  0 14:24 pts/0    00:00:00 ps -f
bash
ps -f
    UID         PID   PPID  C STIME TTY          TIME CMD
    yangwei+   2418   1538  0 14:24 pts/0    00:00:00 bash
    yangwei+   2436   2418  0 14:25 pts/0    00:00:00 bash
    yangwei+   2446   2436  0 14:25 pts/0    00:00:00 ps -f

--------Now, There are two bash process. you can see it that the first bash process is 
    the parent to second bash process by the attribute 'PPID'.you can recursively 
    input the 'exit' to stop the CLI.


pwd; ls; cd /etc; pwd; echo $BASH_SUBSHELL
    /
    bin    dev   initrd.img  lost+found  opt   run   srv       tmp  vmlinuz
    boot   etc   lib         media       proc  sbin  swapfile  usr
    cdrom  home  lib64       mnt         root  snap  sys       var
    /etc
    0
(pwd; ls; cd /etc; pwd; echo $BASH_SUBSHELL)
    /
    bin    dev   initrd.img  lost+found  opt   run   srv       tmp  vmlinuz
    boot   etc   lib         media       proc  sbin  swapfile  usr
    cdrom  home  lib64       mnt         root  snap  sys       var
    /etc
    1

--------The last attribute show whether exist child shell.0 : None; >=1 : exist.


sleep 5&
    [1] 1678
ps -f
    UID         PID   PPID  C STIME TTY          TIME CMD
    yangwei+   1617   1609  0 19:49 pts/0    00:00:00 bash
    yangwei+   1678   1617  0 19:57 pts/0    00:00:00 sleep 5
    yangwei+   1679   1617  0 19:57 pts/0    00:00:00 ps -f

    [1]+  Done                    sleep 5

--------'&' represent that the process will be running on background.you can also use
    'coproc sleep 5'. It is same function compared with '&'.


which ps
    /bin/ps
type -a ps
    ps is /bin/ps
ls -li /bin/ps
    1048712 -rwxr-xr-x 1 root root 133432 Jul 12  2016 /bin/ps

--------'ps' command belong to the external command.It has a new PID number.

type -a cd
    cd is a shell builtin
type -a exit
    exit is a shell builtin
type -a echo
    echo is a shell builtin
    echo is /bin/echo

--------'cd', 'exit' command belong to the internal command.They have not PID number.
    'echo' command has two kinds of command.


echo my_variable
my_variable=hello
export my_variable
unset my_variable

--------The first command create a local variable 'my_variable'.The second command assign
    a value(hello) to my_variable.The third command export the variable to a global 
    variable.The last command is used to delete a variable.

Environment Variable permanently

you can create a new file('.sh' type) and then put the global variable which you create into the file.

you can put bash shell command that you set into the $HOME/.bashrc. such as 'alias li="ls -li"'.

```