---
tags: 
- LI
- CLI
- TECH
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Technology MOC|Back to Technology MOC]] | [[TECH Linux index|Back to index]]
- `find . -name "EH*" | xargs sed -i 's/MOC: Technology/MOC: Cybersecurity/g'`
	- `sed` receives file contents instead of filenames as input text
```zsh
		# Find files and delete them
find . -name "*.tmp" | xargs rm

# Count lines in multiple files
find . -name "*.txt" | xargs wc -l

# ✅ Safe for filenames with spaces
find . -name "*.txt" -print0 | xargs -0 rm

# Alternative with -exec (also safe)
find . -name "*.txt" -exec rm {} +

# Run up to 4 processes simultaneously  
find . -name "*.jpg" | xargs -P 4 -I {} convert {} {}.optimized.jpg

# Use comma as delimiter instead of space/newline
echo "file1.txt,file2.txt,file3.txt" | xargs -d ',' ls -l
```