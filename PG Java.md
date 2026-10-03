---
tags:
- Java
- Programming-Language
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Java index|Back to index]]
![[PG JavaCheatSheet.pdf]]
# Starting a project
## Manuel approach
- First, create the project directory structure `project/src/main/java/Main.java`
```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```
- Run `javac src/main/java/Main.java` to compile it into a class
- Run `java src/main/Java/ Main` to run the `Main` class