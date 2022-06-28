in this directory i take a dive into the world of shell scripting i will look into shell redirections.

A. write a script that prints "Hello, World" followed by a new line in the starndard output
B. write a script that displays a confused smiley "(Ôo)'.
C. disply the content of the /etc/passwd file.
D. display the content of /etc/passwd and /etc/hosts
E. display the last 10 lines of /etc/passwd
F. display the first 10 lines of /etc/passwd
G. write a script that displays the third line of the file iacta.
	the file iacta will be in the working directory
	the use of sed id not allowed.
H. Write a shell script that creates a file named exactly \*\\'"Best School"\'\\*$\?\*\*\*\*\*:) containing the text Best School ending by a new line.
I. write a script that writes into the file ls_cwd_content the result of the command ls -la. if the file ls_cwd_content already exists, it should be overwritten. if the file ls_cwd_content does  not exist, create it.
J. write a script that duplicates the last line of the file iacta. the file iacta will be in the working directory.
K. write a script that deletes all the regular files not the directories with a .js extension that are present in the current directory and all its subfolders.
L. write a script that counts the number of directories and subdirectories in the current directory. the current and parent directories should not be taken into account. hidden directories should be counted.
M. create a script that displays the 10 newest files in the current directory the requirements are one file per line and they should be sorted from the newest to the oldest.
N. create a script that takes a list of words as input and prints only words that appear exactly once. the input format is one line one word, the output format is one line one word and the words should be sorted.
O. display the lines containing the pattern root from the file /etc/passwd
P. display the number of lines that contain the pattern bin in the file /etc/passwd
Q. display the lines containing the pattern root and three lines after them in the file /etc/passwd
R. display all the lines in the file /etc/passwd that do not containn the pattern bin
S. display all the lines of the file /etc/ssh/sshd_config starting with a letter, capital letters are included.
T. replace all charecters A and c from input to Z and e respectively
U. create a script that removes all letters c and C from input
V. write a script that reverses its input
W. write a script that displays all users and their home directories, sorted by users. based on the /etc/passwd file.
X. write a command that finds all empty files and directories in the current directory and all sub directories.
only the names of the files and directories should be displayed not the entire path, hidden files should be listed, one file per line, the listing should end with a new line, you are not allowed to use basename, grep, egrep, fgrep or rgrep
Y. Write a script that lists all the files with a .gif extension in the current directory and all its sub-directories.

Hidden files should be listed
Only regular files (not directories) should be listed
The names of the files should be displayed without their extensions
The files should be sorted by byte values, but case-insensitive (file aaa should be listed before file bbb, file .b should be listed before file a, and file Rona should be listed after file jay)
One file name per line
The listing should end with a new line
You are not allowed to use basename, grep, egrep, fgrep or rgrep
Z. An acrostic is a poem (or other form of writing) in which the first letter (or syllable, or word) of each line (or paragraph, or other recurring feature in the text) spells out a word, message or the alphabet. The word comes from the French acrostiche from post-classical Latin acrostichis). As a form of constrained writing, an acrostic can be used as a mnemonic device to aid memory retrieval. Read more.

Create a script that decodes acrostics that use the first letter of each line.

The ‘decoded’ message has to end with a new line
You are not allowed to use grep, egrep, fgrep or rgrep
AA. Write a script that parses web servers logs in TSV format as input and displays the 11 hosts or IP addresses which did the most requests.

Order by number of requests, most active host or IP at the top
You are not allowed to use grep, egrep, fgrep or rgrep

