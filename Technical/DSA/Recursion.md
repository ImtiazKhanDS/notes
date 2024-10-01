
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
