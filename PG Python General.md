---
tags: 
- Python
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Python index|Back to index]]
# Creating a venv
- `python -m venv [projectName]`
- `source [projectName]/bin/activate`
- When you're done with the venv `deactivate`
- To delete the venv simply `rm -rf [projectName]`
# `__name__ == "__main__"`
- The if __name__ == "__main__" idiom is a Python construct that helps control code execution in scripts. It’s a conditional statement that allows you to define code that runs only when the file is executed as a script, not when it’s imported as a module.
- When you run a Python script, the interpreter assigns the value "__main__" to the __name__ variable. If Python imports the code as a module, then it sets __name__ to the module’s name instead. By encapsulating code within if __name__ == "__main__", you can ensure that it only runs in the intended context.