---
tags: 
- Algorithms
MOC: Programming
source: "https://www.geeksforgeeks.org/dsa/time-complexities-of-different-data-structures/"
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Alogrithms index|Back to index]]

[Time Complexity](https://www.geeksforgeeks.org/dsa/understanding-time-complexity-simple-examples/) is a concept in computer science that deals with the quantification of the amount of time taken by a set of code or [algorithm](https://www.geeksforgeeks.org/dsa/dsa-tutorial-learn-data-structures-and-algorithms/) to process or run as a function of the amount of input. In other words, the time complexity is how long a program takes to process a given input. The efficiency of an algorithm depends on two parameters:

- Time Complexity
- Space Complexity

****Time Complexity:**** It is defined as the number of times a particular instruction set is executed rather than the total time taken. It is because the total time taken also depends on some external factors like the compiler used, the processor's speed, etc.

****Space Complexity:**** It is the total memory space required by the program for its execution.

### Best case time complexity of different data structures for different operations

| ****Data structure**** | ****Access**** | ****Search**** | ****Insertion**** | ****Deletion**** |
| --- | --- | --- | --- | --- |
| ****Array**** | O(1) | O(1) | O(1) | O(1) |
| ****Stack**** | O(1) | O(1) | O(1) | O(1) |
| ****Queue**** | O(1) | O(1) | O(1) | O(1) |
| ****Singly Linked list**** | O(1) | O(1) | O(1) | O(1) |
| ****Doubly Linked List**** | O(1) | O(1) | O(1) | O(1) |
| ****Hash Table**** | O(1) | O(1) | O(1) | O(1) |
| ****Binary Search Tree**** | O(log n) | O(log n) | O(log n) | O(log n) |
| ****AVL Tree**** | O(log n) | O(log n) | O(log n) | O(log n) |
| ****B Tree**** | O(log n) | O(log n) | O(log n) | O(log n) |
| ****Red Black Tree**** | O(log n) | O(log n) | O(log n) | O(log n) |

### Worst Case time complexity of different data structures for different operations

| ****Data structure**** | ****Access**** | ****Search**** | ****Insertion**** | ****Deletion**** |
| --- | --- | --- | --- | --- |
| ****Array**** | O(1) | O(N) | O(N) | O(N) |
| ****Stack**** | O(N) | O(N) | O(1) | O(1) |
| ****Queue**** | O(N) | O(N) | O(1) | O(1) |
| ****Singly Linked list**** | O(N) | O(N) | O(N) | O(N) |
| ****Doubly Linked List**** | O(N) | O(N) | O(1) | O(1) |
| ****Hash Table**** | O(N) | O(N) | O(N) | O(N) |
| ****Binary Search Tree**** | O(N) | O(N) | O(N) | O(N) |
| ****AVL Tree**** | O(log N) | O(log N) | O(log N) | O(log N) |
| ****Binary Tree**** | O(N) | O(N) | O(N) | O(N) |
| ****Red Black Tree**** | O(log N) | O(log N) | O(log N) | O(log N) |

### The average time complexity of different data structures for different operations

| ****Data structure**** | ****Access**** | ****Search**** | ****Insertion**** | ****Deletion**** |
| --- | --- | --- | --- | --- |
| ****Array**** | O(1) | O(N) | O(N) | O(N) |
| ****Stack**** | O(N) | O(N) | O(1) | O(1) |
| ****Queue**** | O(N) | O(N) | O(1) | O(1) |
| ****Singly Linked list**** | O(N) | O(N) | O(1) | O(1) |
| ****Doubly Linked List**** | O(N) | O(N) | O(1) | O(1) |
| ****Hash Table**** | O(1) | O(1) | O(1) | O(1) |
| ****Binary Search Tree**** | O(log N) | O(log N) | O(log N) | O(log N) |
| ****AVL Tree**** | O(log N) | O(log N) | O(log N) | O(log N) |
| ****B Tree**** | O(log N) | O(log N) | O(log N) | O(log N) |
| ****Red Black Tree**** | O(log N) | O(log N) | O(log N) | O(log N) |

### Related Article on Time and Space Complexity:

- [Time and Space Complexity of Binary Search](https://www.geeksforgeeks.org/dsa/complexity-analysis-of-binary-search/)
- [Time and Space Complexity of Ternary Search](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-ternary-search/)
- [Time and Space Complexity of Breadth First Search (BFS)](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-breadth-first-search-bfs/)
- [Time and Space Complexity of Depth First Search (DFS)](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-depth-first-search-dfs/)
- [Time and Space Complexity of Insertion Sort](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-insertion-sort-algorithm/)
- [Time and Space Complexity of Selection Sort](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-analysis-of-selection-sort/)
- [Time and Space Complexity of Bubble Sort](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-analysis-of-bubble-sort/)
- [Time and Space Complexity of Quick Sort](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-analysis-of-quick-sort/)
- [Time and Space Complexity of Merge Sort](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-analysis-of-merge-sort/)
- [Time and Space complexity of Radix Sort Algorithm](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-radix-sort-algorithm/)
- [Time and Space Complexity of Linked List](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-linked-list/)
- [Time and Space Complexity of Floyd Warshall Algorithm](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-floyd-warshall-algorithm/)
- [Time and Space Complexity of Bellman–Ford Algorithm](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-bellman-ford-algorithm/)
- [Time and Space Complexity of Dijkstra’s Algorithm](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-of-dijkstras-algorithm/)
- [Time and Space Complexity Analysis of Prim's Algorithm](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-analysis-of-prims-algorithm/)
  
# All slides from udemy's JavaScript Algorithms and Data Structures Masterclass
```dataview
table from "AllSlidesPDF-JSAlgorithmsAndDataStructures-ColtSteele" sort file DESC 
```