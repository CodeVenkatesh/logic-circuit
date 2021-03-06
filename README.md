# LogicCircuit
A Python module to simulate logical circuits/graphs.

Inspired by the computational graph behind TensorFlow. Granted, it might be overkill but it *is* very powerful.

The module builds graphs as variables are created. Then, the graphs can be evaluated to obtain an output.

### Basic Usage
Start by cloning or downloading the project, then import it into Python. Only Python 2.7 has been tested at the moment.

```python
import logiccircuit as lc
```

Then, build your circuit. In this example, C = A and B

```python
A = lc.Variable()
B = lc.Variable()
C = lc.And(A, B)
```

Currently the following functional nodes are availiable:

Name | # of Arguments
--- | ---
ConstTrue | 0
ConstFalse | 0
Variable | 0
And | 2 or more
Or | 2 or more
Not | 1
Nand | 2 or more
Nor | 2 or more
Xor | 2 or more
Xnor | 2 or more

To evaluate a circuit for a node with certain inputs to the variables, use `partial_eval`.

```python
lc.partial_eval(C, input_dict={A: True, B: True})

# Output:
# True
```

Pass a list of nodes instead of a single node to evaluate multiple nodes at once.

### Advanced Usage
You can define your own functional blocks from the existing selection of functional nodes. These blocks can then be used like any other functional node. For example, you can define a half adder like this:

```python
def HalfAdder(A, B):
	S = lc.Xor(A, B)
	C = lc.And(A, B)
	return S, C
```

If you want to separate your circuits into completely different graphs, you can explicity create a graph and use it when adding nodes. Then evaluate with respect to a graph. For example:

```python
circuit1 = lc.Graph()
circuit2 = lc.Graph()

with circuit1:
	A = lc.Variable()
	B = lc.Variable()
	C = lc.And(A, B)

circuit1.partial_eval(C, input_dict={A: True, B: False})

# Output:
# False

with circuit2:
	X = lc.Variable()
	Y = lc.Variable()
	Z = lc.Or(X, Y)

circuit2.partial_eval(Z, input_dict={X: True, Y: False})

# Output:
# True
```

### To-Do
* Implement efficient medium-scale integration circuits
* Automate the process of evaluating every possible input
* Add memoization for more efficient computation
* Ensure compatibility with Python 3.x