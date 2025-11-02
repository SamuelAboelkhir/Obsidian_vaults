---
tags:
- Other
- PG
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]
# LSP terms explained with examples

## Core Navigation Actions
#### gd - Go to Definition
What it does: Jumps to where something is originally defined/created.
Example:
```python
pythonCopy# You're here, cursor on 'calculate_tax'
result = calculate_tax(income)  # Press 'gd' here
# Jumps to:
def calculate_tax(amount):      # ← Takes you here (the definition)
    return amount * 0.1
```

#### gr - Go to References
What it does: Shows everywhere that thing is used/mentioned.
Example:
```python
pythonCopydef calculate_tax(amount):      # Press 'gr' here
    return amount * 0.1
	
# Shows you a list of everywhere it's used:
# Line 15: result = calculate_tax(income)
# Line 23: tax = calculate_tax(salary) 
# Line 31: calculate_tax(bonus)
```

#### gD - Go to Declaration
What it does: Goes to where something is declared (usually the same as definition in most languages).
Difference:
- Declaration: "This thing exists"
- Definition: "This is what the thing actually does"
```c
// Declaration (in header file)
int calculate_tax(int amount);    # ← gD takes you here

// Definition (in source file) 
int calculate_tax(int amount) {   # ← gd takes you here
    return amount * 0.1;
}
```
## Advanced Navigation
#### gI - Go to Implementation
What it does: Shows concrete implementations of abstract things.
Example:
```python
class Animal:                    # Abstract base
    def make_sound(self):        # Press 'gI' here
        pass

class Dog(Animal):
    def make_sound(self):        # ← Shows this implementation
        return "Woof!"

class Cat(Animal):
    def make_sound(self):        # ← And this implementation
        return "Meow!"
```
#### K - Hover Documentation
What it does: Shows documentation/info about what's under your cursor.
Example:
```python
import requests

requests.get(url)  # Press 'K' on 'get'
# Shows: "Sends a GET request. Returns Response object..."
```
## Symbol Actions
#### `<leader>ts` - Document Symbols
What it does: Lists all important things in current file (functions, classes, variables).
Shows:
```
Copy📄 current_file.py
├── 🏛️ class UserManager
├── 🔧 def create_user()
├── 🔧 def delete_user() 
├── 📦 def validate_email()
└── 📊 CONSTANTS
```
#### grW - Workspace Symbols
What it does: Search for symbols across your entire project.
Example:
```
Type "user" → Shows all functions/classes with "user" from whole project
Type "calculate" → Shows all calculate functions across all files
```
#### grt - Type Definition
What it does: Shows what type something is.
Example:
```python
typescriptCopylet user = getUser();  # Press <leader>D on 'user'
# Shows: interface User { name: string; email: string; }
```
## Code Actions
#### gra - Code Action
What it does: Shows available fixes/improvements for current line.
Examples:
```python
pythonCopyimport os
import sys
import requests  # Unused import - gra shows "Remove unused import"

def calculate_tax(amount):
    # Missing return type - gra shows "Add return type annotation"
```
#### grA - Global Code Action
What it does: Fixes/improves entire file.
Examples:
```
"Organize imports" (sort and remove unused)
"Add missing type annotations"
"Fix all auto-fixable issues"
```
#### grn - Rename
What it does: Renames something everywhere it's used.
Example:
```python
pythonCopydef calculate_tax(amount):      # Rename this to 'compute_tax'
    return amount * 0.1

result = calculate_tax(100)     # Automatically becomes compute_tax(100)
tax = calculate_tax(salary)     # Automatically becomes compute_tax(salary)
```
## Real-World Usage Examples
#### Python Development:
```python
pythonCopy# 1. Working with imports
import pandas as pd

df = pd.DataFrame()  # Press 'K' on DataFrame → shows docs
                     # Press 'gd' → jumps to pandas source
                     # Press 'gra' → might show "Add type hint"## 2. Function navigation  
def process_data(df):
    result = clean_data(df)  # Press 'gd' → jumps to clean_data function
    return result            # Press 'gr' on process_data → shows all calls

# 3. Class hierarchy
class DataProcessor:
    def process(self):       # Press 'gI' → shows all child implementations
        pass
```
#### JavaScript/TypeScript:
```ts
typescriptCopy// 1. Import management
import { User } from './types';  # gra → "Add to import" for unused types
                                # grA → "Organize imports" for whole file

// 2. Type checking
const user: User = getUser();   # K on User → shows interface definition
                               # <leader>D → shows User type structure

// 3. Refactoring
function calculateTotal() {     # grn → rename everywhere
    return items.reduce...      # gra → "Extract to arrow function"
}
```
## Which Ones Are Most Useful?
#### Essential (use daily):
```
gd - Jump to definition
K - Show documentation
gra - Quick fixes
grn - Rename variables/functions
```
#### Very Helpful:
```
gr - Find all usages
<leader>ts - Navigate file structure
grA - Fix whole file issues
```
#### Occasional:
```
gi - See implementations (useful with inheritance)
grW - Search across project
grt - Check types
```
Think of LSP as your coding assistant that knows about your entire codebase and can help you navigate, understand, and fix code much faster than doing it manually!