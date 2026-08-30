---
tags: 
- CLI
- LI
MOC: Technology
---

[[_0000 Home|Home]] | [[_0001 Linux MOC|Back to Linux MOC]] 

# Basics
## Terminal
- The terminal was historically a physical computer interface used to type commands for the computer
- It was made of just a screen and a keyboard
- Nowadays, a terminal is a terminal emulator, such as alacritty or ghostty, that emulates a physical terminal
- The main responsibility of the terminal is to be an interface for communication between the user, and the shell, by letting you pass text-based commands, and rendering the output
## Shell
- The shell is the program that runs the commands passed by the terminal
- They are basically a REPL
- Shells also come in multiple flavors, such as Bash and Zsh, and each shell has built-in commands and the ability to be extended with user made commands, as shells are in fact full on programming languages
## Variables
- On the shell we can have variables, functions, loops, and more
- However, shells are still not meant to be used for writing big programs
- The main intention behind the shell's programming capabilities is to write small scripts, and running other programs
- A variable on the shell would look like this
```bash
variable="value"
```
- Not leaving any spaces here is intentional as it's part of the syntax
- The variable can then be used later like this
```bash
echo $variable
```
- String interpolation is quite easy with shells
```bash
echo This is my variable $variable
```
## Filesystem
- The filesystem is basically a tree structure comprised of directories and files, where directories can hold other directories and more files
- The files themselves are just a collection of raw binary data, and a file's metadata is usually identified by an `inode` (index node)
- A filesystem starts at the root directory, and wherever you are on the filesystem is called the current working directory
- In a filepath, the first `/` represents the root directory, so `/home/blackdovah` means root -> home -> blackdovah
- `.` is an alias for the current directory, and `..` represents the parent directory
- We can also create symbolic links (symlink) files essentially making one file a pointer to the other
```bash
ln -s target_path link_path
```
- More information on `ln` can be found here [[LI CLI Tools and Commands]]
## Compiled and interpreted
- These are files with the suffix `.sh` which are normally located in a `/bin` folder, such as `/user/bin`
- These scripts are interpreted by the shell, line by line when they're executed
- Linux also uses a bunch of compiled programs, such as the shell itself `sh`
## Environment variables
- These are similar to variables in the sense that they too are variables, but, they're more powerful
- They're available to all programs that are running on the shell, not just the shell itself
- To set one
```bash
export VARIABLE="value"
```
- The convention here is to define them with uppercase names
- Usage is the same as normal variables
```bash
echo $VARIABLE
```
- And as stated, they will be available for use in programs, such as scripts
```bash
# example.sh
#!/bin/sh
echo "This is an env variable $VARIABLE"
```
- A script must be made executable for the user, since this isn't a default permission
```bash
chmod +x ./example.sh
./example.sh
```
## PATH
- This is a built-in environment variable, and is core to the whole functionality of the shell
- The PATH stores the location of every single executable on the system, making them instantly usable
- So instead of
```bash
/bin/ls
```
- We can say
```bash
ls
```
- The PATH doesn't store the individual executable's paths, but rather their container directories, which is why it will be filled with paths that end in `/bin`
- The paths in PATH are `:` separated
```bash
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```
- This means that the shell now knows to look for executable in
	- `/usr/local/bin`
	- `/usr/bin`
	- `/bin`
	- `/usr/sbin`
	- `/sbin`
- Like any other variable or environment variable, the PATH can be re-assigned, so we can for example do this
```bash
export PATH=""
```
- Which will basically clear out the path, making all commands unusable without their full path
- Those changes don't persist though, and remain within the current shell session only
- To append the PATH, we can say
```bash
export PATH="$PATH:/path/to/new"
```
- It's important to remember to use `"` instead of `'` as the double quote expands variables, similar to backticks in javascript
- But this addition still won't persist past the current shell session, so how can we change this?
- We need to build the PATH on every shell session, using the `rc` file, whether that's `.bashrc` or `.zshrc`, or whatever other `rc` for the current shell
- This is done by writing the above `export` command in the `rc` file, which gets automatically sourced in the shell on session startup
- This is actually what all the executables do, as they all
## Output
- We have two types of outputs from any command, which are `stout` and `stderr`
- Those outputs go straight to the console, but can also be redirected with `>` and `2>` respectivaly
## Signals
- `CTRL + C` issues a `SIGINT` or signal interrupt to any running process
- Sometimes that's not enough, and a more drastic measure needs to be taken via the `kill` command to stop a process, which issues a `SIGKILL`
## The unix philosophy
- This philosophy can be summed in those three lines:
	1. Write programs that do one thing and do it well.
		1. Commands like `ls` and `grep` only have one purpose, even if they can take flags that alters the way they perform that purpose
	2. Write programs to work together.
		1. Since programs only do one thing, they can be chained together seemlessly
	3. Write programs to handle text streams, because that is a universal interface.
		1. That's why point number 2 is even possible, because programs that use the same interface, work well together
		2. The fact that all programs use the same text interface, which is a text stream, which means a sequential assortment of characters, or in other words, just text
- The shell is a command-line (text) interface. 
- Text-based interfaces are much more powerful and extensible than graphical interfaces. 
- That's why developers have been using them for decades, and why what we can do with them looks like magic to the uninitiated.
## Package managers
- A package manager is a software tool that helps you install other software. Its primary functions include:
	- Downloading software from official sources
	- Installing software
	- Updating software
	- Removing software
	- Managing dependencies
### How Does a Package Manager Work?
- When you type a command like `apt install neovim`, the package manager will:
	1. Check to see if the package is already installed.
	2. If it's not installed, it will download the package from a repository.
	3. It will install the package on your computer.
	4. It will install any dependencies that the package needs to run.
	5. It will (hopefully) add the package to your PATH if it should be there.
- Good package managers keep track of what packages you have installed, and what versions of those packages you have installed. They keep your filesystem nice and tidy, making sure you haven't installed 10 different instances of the same package or application.
### Webi
- Webi lets you install command line tools directly from the web, with no need for a traditional package manager like `apt` or `brew`. 
- You don't need to install Webi itself at all; instead, you just run a shell command that downloads and runs a given tool's official installer script.
- As a general rule, it's not smart to pipe a shell script from the internet directly into your terminal, because that script could do malicious things. 
- That said, as long as you're doing it via HTTPS, and you trust the source (in this case `webinstall.dev`), it's not as big of a concern.