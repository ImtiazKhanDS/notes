
A process in which a function keeps calling itself is known as recursion and the corresponding function is called a recursive function. There are two important components to write a recursive function -

1. Recurrence relation - Break a larger problem to a smaller problem
2. Termination condition 

Control Flow 

```python
def fact(N:int):
	if N == 0: return 1
	return N * fact(N-1)
```


```mermaid
flowchart TD

A[main] --> B["fact(5)"] --> C["fact(4)"] --> D["fact(3)"] --> E["fact(2)"] --> F["fact(1)"] --> G["fact(0)"]







```

This is top to bottom (depth first) , the return happens in bottom to top fashion. The first function to return is the bottom (LIFO) . This is called stack.

Time complexity : O(n)      ->    n push operations and n pop operations

Space complexity : O(n)   ->    Recursion stack memory

This type of recursion is called single branch recursion.


```python
def func(x : int):
	print(x)
	if x>=3:
		return
	func(x+1)
	func(x+2)

if __name__ == "__main__":
	func(0)
```


The above snippet of code is for multi branch recursion

 **Recursion Tree Diagram**

```mermaid
graph TD

f_0("f(0)") --> f_1_0("0")
f_0("f(0)") --> f_1_1("f(1)")

f_0("f(0)") --> f_1_2("f(2)")

f_1_1 --> f_2_0("1")
f_1_1 --> f_2_1("f(2)")
f_1_1 --> f_2_2("f(3)")

f_2_2 --> f_5_0("3")


f_2_1 --> f_3_0("2")
f_2_1 --> f_3_1("f(3)")
f_2_1 --> f_3_2("f(4)")


f_3_1 --> f_4_0("3")
f_3_2 --> f_4_1("4")


f_1_2 --> f_6_0("2")
f_1_2 --> f_6_1("f(3)")
f_1_2 --> f_6_2("f(4)")


f_6_1 --> f_7_0("3")
f_6_2 --> f_7_1("4")
```

Print numbers from 1 to N

```python
# Given N print 1 -> N using recursion
def f(x:int, n:int):
	if x > n:
		return
	print(x)
	f(x+1, n)

f(1, 10)
  

# with a single variable.
def f(n:int):
	if n == 0:
		return
	f(n-1)
	print(n)

f(10)
```


Given a m * n Grid  

Destination : (m-1, n-1)

Need to reach (0, 0) --> (m-1, n-1)

How many distinct paths ? Given Constraints : 1. 1 Unit right and 1 Unit Bottom


| (0, 0) | -   | -      |
| ------ | --- | ------ |
| -      | -   | -      |
| -      | -   | -      |
| -      | -   | (m, n) |






