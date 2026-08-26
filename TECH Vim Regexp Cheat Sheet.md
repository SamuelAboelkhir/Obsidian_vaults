---
tags: 
- LI
MOC: Technology
source: "https://cheatography.com/fievel/cheat-sheets/vim-regexp/pdf/?last=1482147025"
---
[[_0000 Home|Home]] | [[_0001 Technology MOC|Back to Technology MOC]] | [[TECH Linux index|Back to index]]
![[fievel_vim-regexp.pdf]]
# Examples
- Starting from this block
```python
class BlockType(Enum):
	paragraph = paragraph
	heading = heading
	code = code
	quote = quote
	unordered_list = unordered_list
	ordered_list = ordered_list
```
- First we use this regex to uppercase all the starting words after selecting the block under the signature in visual mode
```
> '<,'>s/\v(\w+)/\U\1
```
```python
class BlockType(Enum):
    PARAGRAPH = paragraph
    HEADING = heading
    CODE = code
    QUOTE = quote
    UNORDERED_LIST = unordered_list
    ORDERED_LIST = ordered_list
```
- That regex:
	- Went into very magic mode `\v`
	- Created a group with `()`
	- Wthin the group it select 1 word, which defaulted to the first one in each line `\w+`
	- Uppercased the group `\U`
	- Selected the group with `\1` in the replace side
```
> '<,'>s/\v(\w+$)/"\1"
```
```python
class BlockType(Enum):
    PARAGRAPH = "paragraph"
    HEADING = "heading"
    CODE = "code"
    QUOTE = "quote"
    UNORDERED_LIST = "unordered_list"
    ORDERED_LIST = "ordered_list"
```
- The 2nd regex:
	- Went into very magic mode again
	- Created a group again
	- Selected 1 word like before, but we specified it should be at the end of the line `$`
	- Added `""` around 