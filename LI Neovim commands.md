---
tags: 
- neovim
- LI
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Linux MOC|Back to Linux MOC]] 
# Basics
## File manipulation
- `:file`: Allows you to rename your current file
- `:read`: Reads a file's content in the current file
	- `:read !sed -n '10,20p' file1.txt`: A format for appending specific lines from a file in the current file
- You can use external CLI commands with nvim open by typing `:![command]`
- To do a mass search and replace without `specter`
	- Use telescope's livegrip and type a term
	- press `C-q` to add all occurrences of the term to quickfix
	- type `:cdo s/[oldterm]/[newterm]/gc`
		- `cdo`: Allows you to apply the commands after it too all the entries in the quickfix menu
		- Best to use `c` to receive a confirmation message before you actually apply the change
- You can reload a certain file, or even just certain lines of code by highlighting them and going to command mode then typing `source %`
	- It will look like this `'<,'>source %`
## ROT13
- This is a letter substitution cipher where each letter is replaced by the 13th letter after it in the Latin alphabet
- I'm mentioning it, because vim has a built-in method of performing this cipher [https://en.wikipedia.org/wiki/ROT13](https://en.wikipedia.org/wiki/ROT13)