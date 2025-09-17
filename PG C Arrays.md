---
tags:
- C
- PG
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[PG C index|Back to index]]
# Arrays
- In C an array is merely a pointer to the memory address of the first element in the array
- When you pass an array to a function, it decays into a pointer to the first element in the array and not the array itself, therefore, if you need to do a calculation relying on the array size, you'll need to pass that as well to the function
	- ```
	  #define <stdio.h>
	  
	  void test(*arr, size){
		  printf("First element in the str array is %c, and the arr size is %d \n", arr[0], size)
	  }
	  void main(){
		  int x[5] = {0,1,2,3,4};
		  int size = sizeof(x)/sizeof(x[0])
		  test(x, size)
	  }
	  ```
	- Note that to calculate the size of the array, you use the sizeof function which calculates the total size of the array as the total bytes count of its elements, which is why you then need to divide the total size by the size of any one element (element 0 by convention)