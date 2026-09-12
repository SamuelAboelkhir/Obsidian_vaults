---
tags: 
- emacs
- LI
MOC: Technology
source: "https://orgmode.org/worg/orgcard.html"
---
[[_0000 Home|Home]] | [[_0001 Linux MOC|Back to Linux MOC]] 
## Table of Contents

- [1. Getting Started](#org024e18c)
- [2. Visibility Cycling](#orga0ec809)
- [3. Motion](#org9aa4d4d)
- [4. Structure Editing](#org8362428)
- [5. Capture - Refile - Archiving](#org160492e)
- [6. Filtering and Sparse Trees](#org8ea5abc)
- [7. Tables](#orgcbcd298)
    - - [7.0.1. Creating a table](#org960930a)
    - [7.1. Commands available inside tables](#org44b10f4)
        - [7.1.1. Re-aligning and field motion](#org316eb16)
        - [7.1.2. Row and column editing](#org1f24735)
        - [7.1.3. Regions](#org7551b4a)
        - [7.1.4. Miscellaneous](#orgc0c9f29)
    - [7.2. Tables created with the table.el package](#orga05e2dc)
    - [7.3. Spreadsheet](#org24c40d6)
    - [7.4. Formula Editor](#org80fc120)
- [8. Links](#org6e3b162)
- [9. Working with Code (Babel)](#org7256042)
- [10. Completion](#orgc37433e)
- [11. TODO Items and Checkboxes](#orgf56a56a)
- [12. Tags](#orgbbeb97c)
- [13. Properties and Column View](#org1a8044c)
- [14. Timestamps](#orgfe86986)
    - [14.1. Clocking time](#orgf39b40d)
- [15. Agenda Views](#org48faeed)
    - [15.1. Commands available in an agenda buffer](#orgcd11259)
        - [15.1.1. View Org file](#orgcd47ff1)
        - [15.1.2. Change display](#orga67e783)
        - [15.1.3. Remote editing](#orgb3fe741)
        - [15.1.4. Misc](#org88c2a85)
        - [15.1.5. Calendar commands](#org4d49ca0)
        - [15.1.6. Quit and Exit](#org3d57400)
- [16. LaTeX and cdlatex-mode](#org5abc37b)
- [17. Exporting and Publishing](#orge145e11)
- [18. Dynamic Blocks](#orgc7d0c31)

## 1. Getting Started

 
|Key|Function|
|---|---|
|`M-x org-info`|To read the on-line documentation try|

## 2. Visibility Cycling

 
|Key|Function|
|---|---|
|`TAB`|rotate current subtree between states|
|`S-TAB`|rotate entire buffer between states|
|`C-u C-u TAB`|restore property-dependent startup visibility|
|`C-u C-u C-u TAB`|show the whole file, including drawers|
|`C-c C-r`|reveal context around point|

## 3. Motion

 
|Key|Function|
|---|---|
|`C-c C-n/p`|next/previous heading|
|`C-c C-f/b`|next/previous heading, same level|
|`C-c C-u`|backward to higher level heading|
|`C-c C-j`|jump to another place in document|
|`S-UP/DOWN`|previous/next plain list item|

## 4. Structure Editing

 
|Key|Function|
|---|---|
|`M-RET`|insert new heading/item at current level|
|`C-RET`|insert new heading after subtree|
|`M-S-RET`|insert new TODO entry/checkbox item|
|`C-S-RET`|insert TODO entry/ckbx after subtree|
|`C-c -`|turn (head)line into item, cycle item type|
|`C-c *`|turn item/line into headline|
|`M-LEFT/RIGHT`|promote/demote heading|
|`M-S-LEFT/RIGHT`|promote/demote current subtree|
|`M-S-UP/DOWN`|move subtree/list item up/down|
|`C-c ^`|sort subtree/region/plain-list|
|`C-c C-x c`|clone a subtree|
|`C-c C-x v`|copy visible text|
|`C-c C-x C-w/M-w`|kill/copy subtree|
|`C-c C-x C-y or C-y`|yank subtree|
|`C-x n s/w`|narrow buffer to subtree / widen|

## 5. Capture - Refile - Archiving

 
|Key|Function|
|---|---|
|`C-c c`|capture a new item (C-u C-u = goto last)|
|`C-c C-w`|refile subtree (C-u C-u = goto last)|
|`C-c C-x C-a`|archive subtree using the default command|
|`C-c C-x C-s`|move subtree to archive file|
|`C-c C-x a/A`|toggle ARCHIVE tag / to ARCHIVE sibling|
|`C-TAB`|force cycling of an ARCHIVEd tree|

## 6. Filtering and Sparse Trees

 
|Key|Function|
|---|---|
|`C-c /`|construct a sparse tree by various criteria|
|`C-c / t/T`|view TODO's in sparse tree|
|`C-c a t`|global TODO list in agenda mode|
|`C-c a L`|time sorted view of current org file|

## 7. Tables

#### 7.0.1. Creating a table

- `C-c |`: convert region to table
- `C-3 C-c |` : separator at least 3 spaces

### 7.1. Commands available inside tables

The following commands work when the cursor is inside a table. Outside of tables, the same keys may have other functionality.

#### 7.1.1. Re-aligning and field motion

 
|Key|Function|
|---|---|
|`C-c C-c`|re-align the table without moving the cursor|
|`TAB`|re-align the table, move to next field|
|`S-TAB`|move to previous field|
|`RET`|re-align the table, move to next row|
|`M-a/e`|move to beginning/end of field|

#### 7.1.2. Row and column editing

 
|Key|Function|
|---|---|
|`M-LEFT/RIGHT`|move the current column left|
|`M-S-LEFT`|kill the current column|
|`M-S-RIGHT`|insert new column to left of cursor position|
|`M-UP/DOWN`|move the current row up/down|
|`M-S-UP`|kill the current row or horizontal line|
|`M-S-DOWN`|insert new row above the current row|
|`C-c -`|insert hline below (C-u : above) current row|
|`C-c RET`|insert hline and move to line below it|
|`C-c ^`|sort lines in region|

#### 7.1.3. Regions

 
|Key|Function|
|---|---|
|`C-c C-x C-w/M-w/C-y`|cut/copy/paste rectangular region|
|`C-c C-q`|fill paragraph across selected cells|

#### 7.1.4. Miscellaneous

 
|Key|Function|
|---|---|
|`` C-c ` ``|edit the current field in a separate window|
|`C-u TAB`|make current field fully visible|
|`M-x org-table-export`|export as tab-separated file|
|`M-x org-table-import`|import tab-separated file|
|`C-c +`|sum numbers in current column/rectangle|

To limit column width to N characters, use `... | <N> | ...`.

### 7.2. Tables created with the table.el package

 
|Key|Function|
|---|---|
|`C-c ~`|insert a new table.el table|
|`C-c C-c`|recognize existing table.el table|
|`C-c ~`|convert table (Org-mode <-> table.el)|

### 7.3. Spreadsheet

- Formulas typed in field are executed by `TAB`, `RET` and `C-c C-c`.
- `=` introduces a column formula, `:=` a field formula.
- Example: Add Col1 and Col2: `=1+2`
- … with printf format specification: `=1+2;%.2f`
- … with constants from constants.el: `=1/c/cm`
- sum from 2nd to 3rd hline: `:=vsum(@II..@III)`

 
|Key|Function|
|---|---|
|`=`|apply current column formula|
|`C-c =`|set and eval column formula|
|`C-u C-c #`|set and eval field formula|
|`C-c *`|re-apply all stored equations to current line|
|`C-u C-c *`|re-apply all stored equations to entire table|
|`C-u C-u C-c *`|iterate table to stability|
|`C-#`|rotate calculation mark through # * ! ^ _|
|`C-c ?`|show line, column, formula reference|
|`C-c }/{`|toggle grid / debugger|

### 7.4. Formula Editor

 
|Key|Function|
|---|---|
|`C-c '`|edit formulas in separate buffer|
|`C-c C-c`|exit and install new formulas|
|`C-u C-c C-c`|exit, install, and apply new formulas|
|`C-c C-q`|abort|
|`C-c C-r`|toggle reference style|
|`TAB`|pretty-print Lisp formula|
|`M-TAB`|complete Lisp symbol|
|`S-cursor`|shift reference point|
|`M-up/down`|shift test line for column references|
|`M-S-up/down`|scroll the window showing the table|
|`C-c }`|toggle table coordinate grid|

## 8. Links

 
|Key|Function|
|---|---|
|`C-c l`|globally store link to the current location|
|`C-c C-l`|insert a link (TAB completes stored links)|
|`C-u C-c C-l`|insert file link with file name completion|
|`C-c C-l`|edit (also hidden part of) link at point|
|`C-c C-o`|open file links in emacs|
|`C-u C-c C-o`|…force open in emacs/other window|
|`mouse-1/2`|open link at point|
|`mouse-3`|…force open in emacs/other window|
|`C-c %`|record a position in mark ring|
|`C-c &`|jump back to last followed link(s)|
|`C-c C-x C-n`|find next link|
|`C-c C-x C-p`|find previous link|
|`C-c '`|edit code snippet of file at point|
|`C-c C-x C-v`|toggle inline display of linked images|

## 9. Working with Code (Babel)

 
|Key|Function|
|---|---|
|`C-c C-c`|execute code block at point|
|`C-c C-o`|open results of code block at point|
|`C-c C-v c`|check code block at point for errors|
|`C-c C-v j`|insert a header argument with completion|
|`C-c C-v v`|view expanded body of code block at point|
|`C-c C-v I`|view information about code block at point|
|`C-c C-v g`|go to named code block|
|`C-c C-v r`|go to named result|
|`C-c C-v u`|go to the head of the current code block|
|`C-c C-v n`|go to the next code block|
|`C-c C-v p`|go to the previous code block|
|`C-c C-v d`|demarcate a code block|
|`C-c C-v x`|execute the next key sequence in the code edit bu|
|`C-c C-v b`|execute all code blocks in current buffer|
|`C-c C-v s`|execute all code blocks in current subtree|
|`C-c C-v t`|tangle code blocks in current file|
|`C-c C-v f`|tangle code blocks in supplied file|
|`C-c C-v i`|ingest all code blocks in supplied file into the|
|`C-c C-v z`|switch to the session of the current code block|
|`C-c C-v l`|load the current code block into a session|
|`C-c C-v a`|view sha1 hash of the current code block|

## 10. Completion

In-buffer completion completes TODO keywords at headline start, TeX macros after `\', option keywords after `#-', TAGS after `:', and dictionary words elsewhere.

 
|Key|Function|
|---|---|
|`M-TAB`|complete word at point|

## 11. TODO Items and Checkboxes

 
|Key|Function|
|---|---|
|`C-c C-t`|rotate the state of the current item|
|`S-LEFT/RIGHT`|select next/previous state|
|`C-S-LEFT/RIGHT`|select next/previous set|
|`C-c C-x o`|toggle ORDERED property|
|`C-c C-v`|view TODO items in a sparse tree|
|`C-3 C-c C-v`|view 3rd TODO keyword's sparse tree|
|`C-c , [ABC]`|set the priority of the current item|
|`C-c , SPC`|remove priority cookie from current item|
|`S-UP/DOWN`|raise/lower priority of current item|
|`M-S-RET`|insert new checkbox item in plain list|
|`C-c C-x C-b`|toggle checkbox(es) in region/entry/at point|
|`C-c C-c`|toggle checkbox at point|
|`C-c #`|update checkbox statistics (C-u : whole file)|

## 12. Tags

 
|Key|Function|
|---|---|
|`C-c C-q`|set tags for current heading|
|`C-u C-c C-q`|realign tags in all headings|
|`C-c \\`|create sparse tree with matching tags|
|`C-c C-o`|globally (agenda) match tags at cursor|

## 13. Properties and Column View

 
|Key|Function|
|---|---|
|`C-c C-x p/e`|set property/effort|
|`C-c C-c`|special commands in property lines|
|`S-left/right`|next/previous allowed value|
|`C-c C-x C-c`|turn on column view|
|`C-c C-x i`|capture columns view in dynamic block|
|`q`|quit column view|
|`v`|show full value|
|`e`|edit value|
|`n/p` or `S-left/right`|next/previous allowed value|
|`a`|edit allowed values list|
|`> / <`|make column wider/narrower|
|`M-left/right`|move column left/right|
|`M-S-right`|add new column|
|`M-S-left`|Delete current column|

## 14. Timestamps

 
|Key|Function|
|---|---|
|`C-c .`|prompt for date and insert timestamp|
|`C-u C-c .`|like C-c . but insert date and time format|
|`C-c !`|like C-c . but make stamp inactive|
|`C-c C-d`|insert DEADLINE timestamp|
|`C-c C-s`|insert SCHEDULED timestamp|
|`C-c / d`|create sparse tree with all deadlines due|
|`C-c C-y`|the time between 2 dates in a time range|
|`S-RIGHT/LEFT`|change timestamp at cursor ±1 day|
|`S-UP/DOWN`|change year/month/day at cursor by ±1|
|`C-c >`|access the calendar for the current date|
|`C-c <`|insert timestamp matching date in calendar|
|`C-c C-o`|access agenda for current date|
|`mouse-1/RET`|select date while prompted|
|`C-c C-x C-t`|toggle custom format display for dates/times|

### 14.1. Clocking time

 
|Key|Function|
|---|---|
|`C-c C-x C-i`|start clock on current item|
|`C-c C-x C-o/x`|stop/cancel clock on current item|
|`C-c C-x C-d`|display total subtree times|
|`C-c C-c`|remove displayed times|
|`C-c C-x C-r`|insert/update table with clock report|

## 15. Agenda Views

 
|Key|Function|
|---|---|
|`C-c [`|add/move current file to front of agenda|
|`C-c ]`|remove current file from your agenda|
|`C-'`|cycle through agenda file list|
|`C-c C-x </>`|set/remove restriction lock|
|`C-c a a`|compile agenda for the current week|
|`C-c a t`|compile global TODO list|
|`C-c a T`|compile TODO list for specific keyword|
|`C-c a m`|match tags, TODO kwds, properties|
|`C-c a M`|match only in TODO entries|
|`C-c a #`|find stuck projects|
|`C-c a L`|show timeline of current org file|
|`C-c a C`|configure custom commands|
|`C-c C-o`|agenda for date at cursor|

### 15.1. Commands available in an agenda buffer

#### 15.1.1. View Org file

 
|Key|Function|
|---|---|
|`SPC/mouse-3`|show original location of item|
|`L`|show and recenter window|
|`TAB/mouse-2`|goto original location in other window|
|`RET`|goto original location, delete other windows|
|`C-c C-x b`|show subtree in indirect buffer, ded.\ frame|
|`F`|toggle follow-mode|

#### 15.1.2. Change display

 
|Key|Function|
|---|---|
|`o`|delete other windows|
|`v`|view mode dispatcher|
|`d w vm vy vSP`|switch to day/week/month/year/def view|
|`D / G / K`|toggle diary entries / time grid / habits|
|`E / R`|toggle entry text / clock report|
|`l / v l/L/c`|toggle display of logbook entries|
|`v a/A`|toggle inclusion of archived trees/files|
|`r / g`|refresh agenda buffer with any changes|
|`/`|filter with respect to a tag|
|`s`|save all org-mode buffers|
|`f / b`|display next/previous day,week,…|
|`. / j`|goto today / some date (prompt)|

#### 15.1.3. Remote editing

 
|Key|Function|
|---|---|
|`0-9`|digit argument|
|`t`|change state of current TODO item|
|`C-k`|kill item and source|
|`/ a`|archive default|
|`C-c C-w`|refile the subtree|
|`: / T`|set/show tags of current headline|
|`e`|set effort property (prefix=nth)|
|`, / P`|set / compute priority of current item|
|`S-UP/DOWN`|raise/lower priority of current item|
|`C-c C-a`|run an attachment command|
|`C-c C-s/d`|schedule/set deadline for this item|
|`S-LEFT/RIGHT`|change timestamp one day earlier/later|
|`>`|change timestamp to today|
|`i`|insert new entry into diary|
|`I / O / X`|start/stop/cancel the clock on current item|
|`J`|jump to running clock entry|
|`m / u / B`|mark / unmark / execute bulk action|

#### 15.1.4. Misc

 
|Key|Function|
|---|---|
|`C-c C-o`|follow one or offer all links in current entry|

#### 15.1.5. Calendar commands

 
|Key|Function|
|---|---|
|`c`|find agenda cursor date in calendar|
|`c`|compute agenda for calendar cursor date|
|`M`|show phases of the moon|
|`S`|show sunrise/sunset times|
|`H`|show holidays|
|`C`|convert date to other calendars|

#### 15.1.6. Quit and Exit

 
|Key|Function|
|---|---|
|`q`|quit agenda, remove agenda buffer|
|`x`|exit agenda, remove all agenda buffers|

## 16. LaTeX and cdlatex-mode

 
|Key|Function|
|---|---|
|`C-c C-x C-l`|preview LaTeX fragment|
|`TAB`|expand abbreviation (cdlatex-mode)|
|`` ` / ' ``|insert/modify math symbol (cdlatex-mode)|
|`C-c C-x [`|insert citation using RefTeX|

## 17. Exporting and Publishing

Exporting creates files with extensions .txt and .html in the current directory. Publishing puts the resulting file into some other place.

 
|Key|Function|
|---|---|
|`C-c C-e`|export/publish dispatcher|
|`C-c C-e C-v`|export visible part only|
|`C-c C-e #`|insert template of export options|
|`C-c :`|toggle fixed width for entry or region|
|``C-c C-x {\tt\char`\}``|toggle pretty display of scripts, entities|

1. Comments: Text not being exported  
    
    Lines starting with # and subtrees starting with `COMMENT` are never exported.
    
     
    |Key|Function|
    |---|---|
    |`C-c ;`|toggle COMMENT keyword on entry|
    

## 18. Dynamic Blocks

 
|Key|Function|
|---|---|
|`C-c C-x C-u`|update dynamic block at point|
|`C-u C-c C-x C-u`|update all dynamic blocks|