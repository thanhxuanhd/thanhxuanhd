__There are four types of the shell, we have in Linux__
* Broune Shell -Sh shell
* C Shell - csh or tcsh
* Z shell -zsh
* My Favorite - Bourne again shell -bash

1.  To check shell type
> echo #SHELL

2.  To print on the terminal
> echo Hi from terminal !!

3.  To view the current directory, display absolute path, means path from the root
> pwd

4.  To view all the files in a directory, -a will display hidden files as well
> ls

> ls -a

5.  To change directory, it is case-sensitive, and you have to type in the name of the folder exactly as it is.
> cd .. - To go back from a folder to the folder before that.

> cd my_directory - to goto my_directory folder

6.  When folder name has space e.g. my bird
> cd my\ bird /

7.  Create a new directory
> mkdir foldername

7. 1  When folder name has a space e.g. folder name

> mkdir folder\ name

7.2 Make directory hierarchy from one command 
> mkdir -p /dir1/dir2/dir3/dir4 

8.  Run multiple commands in a single line

> cd abhishek\ sharma; mkdir devops_tools; ls

9. Create a file
> touch file_1.txt

10.  Write content in the file
> cat > file_1.txt

10.1 write anything, write- "this is my first file in Linux" and type Ctrl+d to save

10.2 another way to do it
> echo "this is my first file in linux" > file_1.txt

10.3 View content
> cat file_1.txt

11. Remove directory and file commands
>  mkdir removable_dir <br>
rmdir removable_dir<br>
mkdir removable_dir<br>
cd removable_dir; touch removable_file.txt<br>
rmdir removable_dir<br>
---it gives you an error

_Only blank directories can be deleted with rmdir; to delete a directory, which has content in it, we will use `rm -r`_

> rm -r removable_dir

_To delete a file_
> rm file_1.txt

12. Copy commands

_Copy a file to another file_

>cp {options} source_file target_file<br>
cp new_file.txt copy_file.txt

_Copy File(s) to another directory or folder_

> cp {options} source_file target_directory <br>
> touch /dir_1/dir_2/file_4.txt<br>
> cp -v /dir_1/dir_2/file_4.txt /dir_1/dir_2/dir_3/dir_4

_Copy File(s) to another directory or folder_

> cp {options} source_file target_directory <br>
>touch /dir_1/dir_2/file_4.txt <br>
> cp -v /dir_1/dir_2/file_4.txt /dir_1/dir_2/dir_3/dir_4

_Copy directory to directory_
> cp {options} source_directory target_directory

_To copy a directory from one place to another use -r or -R option in cp command_
>cp -r /dir_1/dir_2/dir_3/dir_4 /dir_1/dir_2/

_If the destination directory already has the same file, still you want to copy that file, to overwrite it use *-i*_
>cat > dir_1/dir_2/file_4.txt - this is me. press ctrl+d<br>
cp -i /dir_1/dir_2/file_4.txt /dir_1/dir_2/dir_3/dir_4

13.  Move command- mv command to move the files one place to another
> touch dir_1/dir_2/file_3.txt <br>
mv -v dir_1/dir_2/file_3.txt dir_1/dir_2/dir_3

14. Rename filename
> touch latest.txt

_want to change this file name latest to new_
> mv latest.txt new.txt <br>
rm latest.txt

15. Search something 

Need to install `sudo apt install mlocate`
> locate new.txt

_if you want to ignore the case_
> locate -i New.txt

_if you want a file that has the word "this"_
> locate -i this

_If you want the file(s) that has words "this" and "me"_
> locate -i *this*me

16. To view available disk space
> df <br>
_above command will show disk space in KB, to view in MB use the following command_<br>
df -m

17. To view disk usage by files
> du

_to view disk space used by a directory_
> du dir_1

18. To view Linux distro
> uname <br>
uname -a

19. Package manager apt-get
> sudo apt-get update <br>
sudo apt-get install nginx -y

20. To view your IP and name in host or network
> hostname <br>
hostname -I

21. To know your user account
> whoami

22. To view user id, group id, group etc.
> id

23. Download a file from a link
> curl www.randomlink.com/downloadable-file.txt -O <br>
OR <br>
wget www.randomlink.com/downloadable-file.txt -O downloadable-file

24.  Service start/stop/enable/disable/restart/status
> systemctl status nginx <br>
systemctl restart nginx  <br>
systemctl stop nginx <br>
systemctl status nginx <br>
systemctl start nginx  <br>
systemctl disable nginx  <br>
systemctl enable nginx

25. To check connection to server
> ping {ip address} <br>
ping www.google.com

26.  Bonus to read

- Write clear to clean the terminal.
- to autofill use TAB. Suppose you have a folder- my_folder and want to go to that folder, You just need to type cd my and then TAB and the terminal fills the rest up and makes it cd my_folder.
- To stop a command write Ctrl+C
- to force stop a command write Ctrl+Z
- Write exit to exit from terminal

Base on  https://dev.to/imabtiwari/linux-i-am-love-with-terminal-2n7p