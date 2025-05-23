```
date (print or set the system date and time)
cal, ncal (displays a calendar)
df (report file system disk space usage)
free (display amount of free and used memory in the system)

Even if we have no terminal emulator running, several terminal sessions 
continue to run behind the graphical desktop. Called virtual terminals or virtual
consoles, these sessions can be accessed on most Linux distributions by pressing
CTRL-ALT-F1 through CTRL-ALT-F6 on most systems. When a session is accessed, it
presents a login prompt into which we can enter our username and password.
To switch from one virtual console to another, press ALT and F1–F6. To return
to the graphical desktop, press ALT-F7.

pwd (print name of current/working directory)
cd (change directory)
ls (list directory contents)
file (determine file type)

less (view file contents) {
	space : scroll forward one page;
	b     : scoll back one page;
	up arrow : scroll up one line
	down arrow : scroll down one line;
	G : move to the end of the text file;
	1G or g : move to the beginning of the text file;
	/characters : search forward to the next occurrence of characters;
	n : search for the next occurrence of the previous search;
	h : display help screen;
	q : quit less;
}

cp (copy files and directories)
mv (move/rename files)
mkdir (make directories)
rm (remove files or directories)
ln (make links between files)

shell Wildcards
* : any characters
? : any single character
[characters] : any character that is a member of the set characters
[!characters] : any character that is not a member of the set characters
[[:class:]] any character that is a member of the specified class {
	[:alnum:] : any alphanumeric character;
	[:alpha:] : any alphabetic character;
	[:digit:] : any numeral;
	[:lower:] : any lowercase letter;
	[:upper:] : any uppercase letter;
}

type (indicate how a command name is interpreted)
which (locate a command)
man (an interface to the system reference manuals)
apropos (display a list of apropriate commands)

info (display a command's info entry) {
	? : display command help;
	BACKSPACE : display previous page;
	Spacebar : display next page;
	n : display the next node;
	p : display the previous node;
	u : display the parent node of the currently displayed node, usually a menu;
	ENTER : follow the hyperlink at the cursor location;
	q : quit;
}

whatis (display a very brief description of a command)
alias (create an alias for a command)
unalias

help [shell built-in command]

cat (concatenate files and print on the standard output)
sort (sort lines of text files)
uniq (report or omit repeated lines)
wc (print newline, word, and byte counts for each file)
grep, egrep, fgrep, rgrep (print lines that match patterns)
head (output the first part of files)
tail (output the last part of files)
tee (read from standard input and write to standard output and files)

ls -l /bin/usr 2> ls-error.txt (redirect standard error, 2 can be replaced with any file descriptor)
ls -l /bin/usr &> ls-output.txt (redirect both standard output and standard error)
ls -l /bin/usr 2> /dev/null (/dev/null receive input and does nothing with it, used for don't want any messages from a command)

echo (display a line of text)

Expansion
Pathname Expansion : echo *
Tilde Expansion : echo ~[username]
Arithmetic Expansion : echo $((expression)) {support only integers, operator is one of the following : +, -, *, /, %, **}
Brace Expansion : echo Front-{A,B,C}-Back (or can use {A..Z}, {M..A}, {1..8}, {8..1})
Parameter Expansion : echo $USER
Command Substitution : echo $(ls)	echo $(ls /usr/bin | grep zip)

Quotes
Double Quotes : ls -l "this is a file" (treat like one file, but $, \, `still remain the same meaning like in the shell, that means parameter expansion, arithmetic expansion and command substitution are still carried out)
Single Quotes : suppress all expansion

Escaping Characters (\)
\ can be used in shell and double quotes to escape character

clear (clear the terminal screen)
history (display the contents of the history list)

Command Line Editing {
	Cursor Movement:
	CTRL-A Move cursor to the beginning of the line.
	CTRL-E Move cursor to the end of the line.
	CTRL-F Move cursor forward one character; same as the right arrow key.
	CTRL-B Move cursor backward one character; same as the left arrow key.
	ALT-F Move cursor forward one word.
	ALT-B Move cursor backward one word.
	CTRL-L Clear the screen and move the cursor to the top left corner. The clear command does the same thing.

	Text Editing Commands:
	CTRL-D Delete the character at the cursor location.
	CTRL-T Transpose (exchange) the character at the cursor location with the one preceding it.
	ALT-T Transpose the word at the cursor location with the one preceding it.
	ALT-L Convert the characters from the cursor location to the end of the word to lowercase.
	ALT-U Convert the characters from the cursor location to the end of the word to uppercase.

	Cut and Paste Commands:
	CTRL-K Kill text from the cursor location to the end of line.
	CTRL-U Kill text from the cursor location to the beginning of the line.
	ALT-D Kill text from the cursor location to the end of the current word.
	ALT-BACKSPACE Kill text from the cursor location to the beginning of the current word. If the cursor is at the beginning of a word, kill the previous word.
	CTRL-Y Yank text from the kill-ring and insert it at the cursor location.
}

Search History
!number (carry out the number command in the command history, which is in .bash_history)
first enter CTRL-R perform a reverse incremental search in command history, which means as search as you enter characters, press CTRL-j to copy the line from search to current command line, to find the next occurrence of the text, press CTRL-R again. To quit searching, press either CTRL-G or CTRL-C.

id (print real and effective user and group IDs)
chmod (change a file's mode)
umask (display or set file mode mask)
su (run a shell as another user)
sudo (execute a command as another user)
chown (change a file's owner)
chgrp (change a file's group ownership)
passwd (change a user's password)

File Types (-rw-rw-rw-) {
	- 		A regular file.
	d 		A directory.
	l 		A symbolic link. Notice that with symbolic links, the remaining file attributes are always rwxrwxrwx and are dummy values. The real file attributes are those of the file the symbolic link points to.
	c 		A character special file. This file type refers to a device that handles data as a stream of bytes, such as a terminal or modem.
	b 		A block special file. This file type refers to a device that handles data in blocks, such as a hard drive or CD-ROM drive.
}

Permission Attributes {
	                Files 										Directories
	r               Allows a file to be opened and read.		Allows a directory’s
																contents to be listed if
																the execute attribute is
																also set.

	w 				Allows a file to be written to or 			Allows files within a
					truncated; however, this attribute does 	directory to be created,
					not allow files to be renamed or 			deleted, and renamed if
					deleted.The ability to delete or rename 	the execute attribute is
					files is determined by directory 			also set.
					attributes.

	x 				Allows a file to be treated as a program 	Allows a directory to
					and executed. Program files written in 		be entered; e.g., 
					scripting languages must alsobe set as 		cd directory.
					readable to be executed.
}

chmod 700 filename/directory 		 chmod [a,u,g,o] [+,-,=] [r,w,x] (only file/directory owner and superuser can change file mode)
umask 0002 (if umask is 0000, the new file created with attributes -rw-rw-rw-, which is default file attributes, through umask we can remove some of those attributes)
su - [user] (change to user)		su -c 'command' (pass the command to new shell)
chown [owner][:[group]] file... (need add sudo at front)

ps (report a snapshot of the current processes)
top (display Linux processes)
jobs (list active jobs)
bg (place a job in the background)
fg (place a job in the foreground)
kill (send a signal to process)
killall (kill processes by name)
shutdown (shut down or reboot the system)

Process States {
	R 	Running. The process is running or ready to run.
	S 	Sleeping. The process is not running; rather, it is waiting for an event, such as a keystroke or network packet.
	D 	Uninterruptible sleep. Process is waiting for I/O such as a disk drive.
	T 	Stopped. Process has been instructed to stop (more on this later).
	Z 	A defunct or “zombie” process. This is a child process that has terminated but has not been cleaned up by its parent.
	< 	A high-priority process. It’s possible to grant more importance to a process, giving it more time on the CPU. This property of a process is called niceness. A process with high priority is said to be less nice because it’s taking more of the CPU’s time, which leaves less for everybody else.
	N 	A low-priority process. A process with low priority (a nice process) will get processor time only after other processes with higher priority have been serviced.
}

BSD-Style ps Column Headers (ps aux) {
	USER 	User ID. This is the owner of the process.
	%CPU 	CPU usage as a percent.
	%MEM 	Memory usage as a percent.
	VSZ 	Virtual memory size.
	RSS 	Resident Set Size. The amount of physical memory (RAM) the process is using in kilobytes.
	START 	Time when the process started. For values over 24 hours, a date is used.
}

xlogo & (make xlogo running in the background, use jobs to see active jobs, use fg %number to make the job back to foreground)

pstree (display a tree of processes)
vmstat (report virtual memory statistics)

printenv (print all or part of environment)
set (set shell options)
export (export environment to subsequently executed programs)
alias (create an alias for a command)

Startup Files for Login Shell Sessions (/etc/profile, ~/.profile)
Startup Files for Non-Login Shell Sessions (/etc/bash.bashrc, ~/.bashrc)

Vim
Moving the Cursor Around {
	L or right arrow 			Right one character
	H or left arrow 			Left one character
	J or down arrow 			Down one line
	K or up arrow 				Up one line
	0 (zero) 					To the beginning of the current line
	SHIFT-6 (^) 				To the first non-whitespace character on the current line
	SHIFT-4 ($) 				To the end of the current line
	W 							To the beginning of the next word or punctuation character
	SHIFT-W (W) 				To the beginning of the next word, ignoring punctuation characters
	B 							To the beginning of the previous word or punctuation character
	SHIFT-B (B) 				To the beginning of the previous word, ignoring punctuation characters
	CTRL-F or PAGE DOWN 		Down one page
	CTRL-B or PAGE UP 			Up one page
	number-SHIFT-G 				To line number (for example, 1G moves to the first line of the file)
	SHIFT-G (G) 				To the last line of the file
} (By prefixing a command with a number, we may specify the number of times a command is to be carried out. For example, the command 5j causes vi to move the cursor down five lines.)

Text Deletion Commands {
	x 					The current character
	3x 					The current character and the next two characters
	dd 					The current line
	5dd 				The current line and the next four lines
	dW 					From the current cursor location to the beginning of the next word
	d$ 					From the current cursor location to the end of the current line
	d0 					From the current cursor location to the beginning of the line
	d^ 					From the current cursor location to the first non-whitespace character in the line
	dG 					From the current line to the end of the file
	d20G 				From the current line to the 20th line of the file
}

Yanking(Coping) Commands {
	yy 					The current line
	5yy 				The current line and the next four lines
	yW 					From the current cursor location to the beginning of the next word
	y$ 					From the current cursor location to the end of the current line
	y0 					From the current cursor location to the beginning of the line
	y^ 					From the current cursor location to the first non-whitespace character in the line
	yG 					From the current line to the end of the file
	y20G 				From the current line to the 20th line of the file
}

J (join current line and the next line

Searching Within a Line (The f command searches a line and moves the cursor to the next instanceof a specified character. For example, the command fa would move the cursor to the next occurrence of the character a within the current line. After performing a character search within a line, the search may be repeated by typing a semicolon.)

Global Search and Replace
:%s/Line/line/g (to change the word Line to line for the entire file) {
	: 					The colon character starts an ex command.
	% 					Specifies the range of lines for the operation. % is a shortcut meaning from the first line to the last line. Alternatively, the range could have been specified 1,5 (because our file is five lines long), or 1,$, which means “from line 1 to the last line in the file.” If the range of lines is omitted, the operation is performed only on the current line.
	s 					Specifies the operation—in this case, substitution (search and replace).
	/Line/line/ 		The search pattern and the replacement text.
	g 					This means global, in the sense that the substitution is performed on every instance of the search string in each line. If g is omitted, only the first instance of the search string on each line is replaced.
}

:%s/line/Line/gc (substitution need user confirmation, vim prompt when substitution : replace with Line (y/n/a/q/l/^E/^Y)?          here is what they mean {
	y 					Perform the substitution.
	n 					Skip this instance of the pattern.
	a 					Perform the substitution on this and all subsequent instances of the pattern.
	q or ESC 			Quit substituting.
	l 					Perform this substitution and then quit. Short for last.
	CTRL-E, CTRL-Y 		Scroll down and scroll up, respectively. Useful for viewing the context of the proposed substitution.
})

vim file file... (open multiple files, :n switch to next file, :N switch to previous file, :buffers show files, :buffer number switch to number file)


Escape Codes Used in Shell Prompts {
	\a 					ASCII bell. This makes the computer beep when it is encountered.
	\d 					Current date in day, month, date format; for example, “Mon May 26”
	\h 					Hostname of the local machine minus the trailing domain name
	\H 					Full hostname
	\j 					Number of jobs running in the current shell session
	\l 					Name of the current terminal device
	\n 					A newline character
	\r 					A carriage return
	\s 					Name of the shell program
	\t 					Current time in 24-hour, hours:minutes:seconds format
	\T 					Current time in 12-hour format
	\@ 					Current time in 12-hour, AM/PM format
	\A 					Current time in 24-hour, hours:minutes format
	\u 					Username of the current user
	\v 					Version number of the shell
	\V 					Version and release numbers of the shell
	\w 					Name of the current working directory
	\W 					Last part of the current working directory name
	\! 					History number of the current command
	\# 					Number of commands entered during this shell session
	\$ 					This displays a “$” character unless you have superuser privileges. In that case, it displays a “#” instead.
	\[ 					This signals the start of a series of one or more non-printing characters. It is used to embed non-printing control characters that manipulate the terminal emulator in some way, such as moving the cursor or changing text colors.
	\] 					This signals the end of a non-printing character sequence.
}

Escape Sequences Used to Set Text Colors {
	\033[0;30m 			Black
	\033[0;31m 			Red
	\033[0;32m 			Green
	\033[0;33m 			Brown
	\033[0;34m 			Blue
	\033[0;35m 			Purple
	\033[0;36m 			Cyan
	\033[0;37m 			Light Gray
	\033[1;30m 			Dark Gray
	\033[1;31m 			Light Red
	\033[1;32m 			Light Green
	\033[1;33m 			Yellow
	\033[1;34m 			Light Blue
	\033[1;35m 			Light Purple
	\033[1;36m 			Light Cyan
	\033[1;37m 			White
}

Escape Sequences Used to Set Background Color {
	\033[0;40m 			Black
	\033[0;41m 			Red
	\033[0;42m 			Green
	\033[0;43m 			Brown
	\033[0;44m 			Blue
	\033[0;45m 			Purple
	\033[0;46m 			Cyan
	\033[0;47m 			Light Gray
}

Cursor Movement Escape Sequences {
	\033[l;cH 			Move the cursor to line l and column c.
	\033[nA 			Move the cursor up n lines.
	\033[nB 			Move the cursor down n lines.
	\033[nC 			Move the cursor forward n characters.
	\033[nD 			Move the cursor backward n characters.
	\033[2J 			Clear the screen and move the cursor to the upper-left corner (line 0, column 0).
	\033[K 				Clear from the cursor position to the end of the current line.
	\033[s 				Store the current cursor position.
	\033[u 				Recall the stored cursor position.
}


ping (send ICMP ECHO_REQUEST to network hosts)
traceroute (print the route packets trace to network host)
netstat (print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships)
ftp (internet file transfer program)
lftp (an improved internet file transfer program)
wget (the non-interactive network downloader)
scp (OpenSSH secure file copy)
sftp (OpenSSH secure file transfer)
locate (find files by name)
find (search for files in a directory hierarchy)
xargs (build and execute command lines from standard input)
touch (change file timestamps)
stat (display file or file system status)

locate searchstring (The locate database is created by another program named updatedb. Most systems equipped with locate run updatedb once a day. Since the database is not updated continuously, you will notice that very recent files do not show up when using locate. To overcome this, it’s possible to run the updatedb program manually by becoming the superuser and running updatedb at the prompt. )

find [directory] [test] {
	-cmin n 			Match files or directories whose content or attributes were last modified exactly n minutes ago. To specify fewer than n minutes ago, use -n; to specify more than n minutes ago, use +n.
	-cnewer file 		Match files or directories whose contents or attributes were last modified more recently than those of file.
	-ctime n 			Match files or directories whose contents or attributes (i.e., permissions) were last modified n*24 hours ago.
	-empty 				Match empty files and directories.
	-group name 		Match file or directories belonging to group name. name may be expressed as either a group name or as a numeric group ID.
	-iname pattern 		Like the -name test but case insensitive.
	-inum n 			Match files with inode number n. This is helpful for finding all the hard links to a particular inode.
	-mmin n 			Match files or directories whose contents were modified n minutes ago.
	-mtime n 			Match files or directories whose contents only were last modified n*24 hours ago.
	-name pattern 		Match files and directories with the specified wildcard pattern.
	-newer file 		Match files and directories whose contents were modified more recently than the specified file. This is very useful when writing shell scripts that perform file backups. Each time you make a backup, update a file (such as a log) and then use find to determine which files have changed since the last update.
	-nouser 			Match file and directories that do not belong to a valid user. This can be used to find files belonging to deleted accounts or to detect activity by attackers.
	-nogroup 			Match files and directories that do not belong to a valid group.
	-perm mode 			Match files or directories that have permissions set to the specified mode. mode may be expressed by either octal or symbolic notation.
	-samefile name 		Similar to the -inum test. Matches files that share the same inode number as file name.
	-size n 			Match files of size n.
	-type c 			Match files of type c.
	-user name 			Match files or directories belonging to name. name may be expressed by a username or by a numeric user ID.
}

gzip (compress or expand files)
bzip2 (a block-sorting file compressor)
tar (an archiving utility)
zip (package and compress (archive) files)
rsync (remote file and directory synchronization)

tar mode[options] pathname... {
	tar Modes
	c 					Create an archive from a list of files and/or directories.
	x 					Extract an archive.
	r 					Append specified pathnames to the end of an archive.
	t 					List the contents of an archive.
} (Unless you are operating as the superuser, files and directories extracted from archives take on the ownership of the user performing the restoration, rather than the original owner.)


cut (remove sections from each line of files)
paste (merge lines of files)
join (join lines of two files on a common field)
comm (compare two sorted files line by line)
diff (compare files line by line)
patch (apply a diff file to an oringinal)
tr (translate or delete characters)
sed (stream editor for filtering and transforming text)
aspell (interactive spell checker)

nl (number lines of files)
fold (wrap each input line to fit in specified witch)
fmt (simple optimal text formatter)
pr (coonvert text files for printing)
printf (format and print data)
groff (a document formatting system)
```
## 设置bash终端提示符颜色
```
vim ~/.bashrc
在文件末尾添加：
PS1='\[\e[1;31m\]\u@\h \[\e[0;32m\]\w \[\e[0m\]\$ '
其中：
\e[1;31m：红色高亮（1表示高亮，31为红色）
\u@\h：用户名@主机名
\e[0;32m：绿色普通亮度（0重置样式，32为绿色）
\w：当前工作目录
\e[0m：重置所有样式
\[\]：包裹ANSI转义码，避免Bash计算错误提示符长度
```