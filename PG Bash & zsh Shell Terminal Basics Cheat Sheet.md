---
tags: 
- Programming-Language/Bash
- PG
MOC: Technology
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Bash index|Back to index]]
- Acquired from [https://www.datacamp.com/cheat-sheet/bash-and-zsh-shell-terminal-basics-cheat-sheet](https://www.datacamp.com/cheat-sheet/bash-and-zsh-shell-terminal-basics-cheat-sheet)
![](https://media.datacamp.com/legacy/image/upload/v1700048361/Bash_Cheat_Sheet_4503e68287.png)

Have this cheat sheet at your fingertips

[Download PDF](https://media.datacamp.com/legacy/image/upload/v1700047731/Marketing/Blog/Bash_Cheat_Sheet.pdf)

## What are Bash & zsh Terminals?

Shell terminals, such as Bash and zsh, are text-based user interfaces for interacting with an operating system. They allow you to input commands through a command line, offering direct communication with the system for tasks like file manipulation, program execution, and system control. Bash is common on Linux systems and zsh is the default on MacOS systems.

## Definitions

The `working directory` is the directory that commands are executed from. By default, commands will read and write files to this directory.

The `root directory` is the top of the file system. All other directories are contained within the hierarchy of this directory.

An `absolute path` starts from the root directory. Think of it like latitude and longitude - the values to a location don't change wherever you are.

A `relative path` starts from the working directory. Think of it like directions from where you are, like "20 kilometers West from here".

A `glob pattern` is a way of specifying multiple files at once.

A `regular expression` is a more complex way of specifying text fragments. Learn more in DataCamp's [Regular Expressions Cheat Sheet](https://www.datacamp.com/cheat-sheet/regular-expresso).

## Getting Help

Display the manual for a command with man

```bash
man head
```

## File System Navigation

Print the current working directory with pwd

```bash
pwd
```

Change the current working directory with cd

```bash
cd data/raw # Go to raw dir inside data dir inside current dir
```

Absolute paths start with the root directory, /

```bash
cd /home
```

Relative paths can start with the current working directory,.

```bash
cd ./images
```

Move up to the parent directory with.. (can be used repeatedly)

```bash
cd ../.. # Go to grandparent directory
```

List files and folders in the current working directory with ls

```bash
ls
```

List all files and folders, including hidden ones (names starting.) with ls -a

```bash
ls -a
```

List files and folders in a human-readable format with ls -lh

```bash
ls -lh
```

List files and folders matching a glob pattern with ls *pattern*

```bash
ls *.csv # Returns all CSV files
```

Recursively list all files below the current working directory with ls -R

```bash
ls -R
```

List estimated disk usage of files and folders in a human-readable format with du -ah

```bash
du -ah
```

Find files by name in the current directory & its subdirectories with find. -type f -name pattern

```bash
find . -type f -name *.ipynb # Find Juypter notebooks
```

## Displaying Files

Display the whole file with cat

```bash
cat README.txt
```

Display a whole file, a page at a time with less

```bash
less README.txt
```

Display the first few lines of a file with head

```bash
head -n 10 filename sales.csv # Get first 10 lines of sales.csv
```

Display the last few lines of a file with tail

```bash
tail -n 10 filename sales.csv # Get last 10 lines of sales.csv
```

Display columns of a CSV file with cut

```bash
cut -d , -f 2-5, 8 sales.csv # Using comma delimiter, select fields 2 to 5 & 8
```

Display the lines of a file containing text matching a regular expression with grep

```bash
grep [0-9]+ sales.csv # Matches lines containing numbers
```

Display the names of files with filenames containing text matching a regular expression with grep -r

```bash
grep -r sales[0-9]+\.csv # Matches filesnames with "sales", numbers, dot "csv"
```

Get the line word & character count of a file with wc

```bash
wc README.txt
```

## Copying, Moving and Removing Files

Copy (and paste) a file to a new directory with cp

```bash
cp sales.csv data/sales-2023.csv # Copy to data dir and rename
```

Copy files matching a glob pattern with cp pattern newdir

```bash
cp *.csv data/ # Copy all CSV files to data dir
```

Move (cut and paste) a file to a new directory with mv

```bash
mv sales.csv data/sales-2023.csv # Move to data dir and rename
```

Rename a file by moving it into the current directory

```bash
mv sales.csv sales-2023.csv
```

Move files matching a glob pattern with mv pattern newdir

```bash
mv *.csv data/ # Move all CSV files to data dir
```

Prevent overwriting existing files with mv -n

```bash
mv -n sales.csv data/sales-2023.csv # Rename unless new filename exists
```

Remove (delete) a file with rm

```bash
rm bad_data.json
```

Remove a directory with rmdir

```bash
rmdir temp_results
```

## Combining commands

Redirect the output from a command to a file with >

```bash
head -n 5 sales.csv > top_sales.csv
```

Pipe the output from a command to another command with |

```bash
head -n 5 sales.csv | tail -n 1
```

Redirect the input to a command with <

```bash
head -n 5 < sales.csv
```

## Glob Patterns

Match one or more character with \*

```bash
*.txt # Match all txt files
```

Match a single character with?

```bash
sales202?.csv # Match this decade's sales data files
```

Match any character in the square brackets with \[...\]

```bash
sales202[0123].csv # Match sales data files for 2020 to 2023
```

Match any patterns listed in the curly braces with {...}

```bash
{*.csv *.tsv} # Matches all CSV and TSV files
```

## Manipulating File Contents

Sort lines of a file with sort

```bash
sort random_order.txt
```

Sort in descending order using sort -r

```bash
sort -r random_order.txt
```

Combine cut and sort using a pipe to sort a column of a CSV file

```bash
cut -d , -f 2 | sort
```

Remove adjacent duplicate lines with uniq

```bash
uniq sales.csv
```

Get counts of (adjacent) duplicate lines with uniq -c

```bash
uniq -c sales.csv
```

Combine sort and uniq using a pipe to remove duplicate lines

```bash
sort random_order.txt | uniq
```

## Variables

List all environment variables with set

```bash
set
```

Create a shell variable with name=value (no spaces around =)

```bash
mydata=sales.csv
```

Create a shell variable from the output of a command with name=$(command)

```bash
pyfiles=$(ls *.py)
```

Print an environment variable or shell variable name with echo $value

```bash
echo $HOME
```

Convert a shell variable into an environment variable with export

```bash
export mydata
```

## Loops and Flow Control

Execute a command for multiple values with for variable in values; do command; done

```bash
datafiles=*.csv
for file in datafiles; do echo $file; done
```

Spread loops over multiple lines for increased readability

```bash
datafiles=*.csv
for file in datafiles
do 
    echo $file
done
```

Conditionally execute code with if \[ condn \]; then command; else alt\_command; fi

```bash
x=99;
if [ $x > 50 ] ; then
    echo "too high";
elif [ $x < 50 ] ; then
    echo "too low";
else
    echo "spot on";
fi
```

## Reusing Commands

See your previous commands with history

```bash
history
```

Save commands in a shell file (extension.sh) and run them with bash or zsh

```bash
bash mycommands.sh 
zsh mycommands.sh
```

---

![Richie Cotton's photo](https://media.datacamp.com/cms/richie-sq.jpeg?w=128)

Author

[Richie Cotton](https://www.datacamp.com/portfolio/richie)

Richie helps individuals and organizations get better at using data and AI. He's been a data scientist since before it was called data science, and has written two books and created many DataCamp courses on the subject. He is a host of the DataFramed podcast, and runs DataCamp's webinar program.

Topics

Related

![](https://media.datacamp.com/legacy/v1700047996/Excel_Keyboard_Shortcuts_Cheat_Sheet_8be3b4534b.png?w=750)Excel Shortcuts Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/excel-shortcuts-cheat-sheet)

Improve on your Excel skills with the handy shortcuts featured in this convenient cheat sheet!

Richie Cotton

![](https://media.datacamp.com/legacy/v1715077530/La_Tex_Cheat_Sheet_c38cf64295.png?w=750)LaTeX Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/latex-cheat-sheet)

Learn everything you need to know about LaTeX in this convenient cheat sheet!

Richie Cotton

![](https://media.datacamp.com/legacy/v1697798108/Markdown_Cheat_Sheet_9657d9746f.png?w=750)Markdown Cheat Sheet

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/markdown-cheat-sheet-23)

Learn everything you need to know about Markdown in this convenient cheat sheet!

Richie Cotton

Python for Data Science - A Cheat Sheet for Beginners

cheat-sheet

[View original](https://www.datacamp.com/cheat-sheet/python-for-data-science-a-cheat-sheet-for-beginners)

This handy one-page reference presents the Python basics that you need to do data science

Karlijn Willems

8 Useful Shell Commands for Data Science

Tutorial

[View original](https://www.datacamp.com/tutorial/shell-commands-data-scientist)

Which shell commands do data scientists use nearly every day? Discover and learn how to use them in this tutorial!

Alexis Perrier

Python For Data Science - A Cheat Sheet For Beginners

Tutorial

[View original](https://www.datacamp.com/tutorial/python-data-science-cheat-sheet-basics)

This handy one-page reference presents the Python basics that you need to do data science

Karlijn Willems