---
tags: 
- LI
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Technology MOC|Back to Technology MOC]] | [[TECH Linux index|Back to index]]
# File manipulation
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