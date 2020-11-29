---
layout: post
title: "Python 'inspect': Diagnosing a Documentation Discrepancy"
description: Using Python's inspect module to catch a documentation discrepancy
tags: python quantum-computing
---

As part of my research into the layout of Quantum Computers, I end up generating and optimizing a lot of circuits designed to test the topological constraints of such machines.

Most of my work has been centered on using [Pytket](https://github.com/CQCL/pytket) although further research had me taking a look at [PyZX](https://github.com/Quantomatic/pyzx) which also has its own unique way of conducting circuit optimization.

I excitedly `pip install`'d the software (as recommended by their documentation) only to find that I couldn't even get through the first part of the tutorial!

## The Problem

The tutorial begins simply enough by showing you how you can leverage some built-in functions in PyZX to generate random functions.

All you have to do is the following:

```python
import pyzx as zx
circuit = zx.generate.CNOT_HAD_PHASE_circuit(qubits=10, depth=20, clifford=True)
```

My import went fine but the moment I invoked that function I got this odd error:

```python
TypeError: CNOT_HAD_PHASE_circuit() got an unexpected keyword argument 'depth'
```

## Investigation

perhaps there was some discrepancy between the documentation and the code...

![PyZX Documentation](/images/pyzx-cnot-had-phase-documentation.png)

Didn't seem like it!

For safe measure I tried remove the keyword arguments and just putting values in, as well some permutations of the code but I couldn't seem to find a way around it.

To be doubly sure, I cloned and dug through the raw code myself and sure enough, the type declaration seemed to match everything so far:

```python
def CNOT_HAD_PHASE_circuit(
        qubits: int, 
        depth: int, 
        p_had: float = 0.2, 
        p_t: float = 0.2, 
        clifford:bool=False
        ) -> Circuit:
```

Eventually I got the idea of just invoking the function without any arguments. That way I might be able to get Python to spit out what the function wanted (despite the fact literally everything else said otherwise).

```
TypeError: CNOT_HAD_PHASE_circuit() missing 4 required positional arguments: 'qubits', 'gates', 'p_had', and 'p_t'
```

Aha! Nowhere in the documentation did it mention a `gates` argument!

I was happy to know I wasn't going completely insane but I still wanted a clearer confirmation of things than the sloppy method of inference I was using. 

## Python `inspect`

A bit of digging rendered a rather neat module Python has called `inspect`, which comes with a slew of functions that allows you to literally peek inside other python objects.

One of the tools is `getsource()` which does the following according to the Python docs:

> Return the text of the source code for an object. The argument may be a module, class, method, function, traceback, frame, or code object. The source code is returned as a single string.

The moment I saw "function" I was immediately hooked. 

## Applying `inspect`

I did the following:

```python
import inspect
lines = inspect.getsource(zx.generate.CNOT_HAD_PHASE_circuit)
print(lines)
```

and was met with a rather nice output for the function code. That's when I was convinced there was a discrepancy between the docs and tutorial versus what was actually in that PyZX library when I installed it:

```python
def CNOT_HAD_PHASE_circuit(qubits, gates, p_had, p_t, clifford=False):
```

Note the existence of the `gates` argument which isn't mentioned anywhere in the screenshot of the PyZX docs above!

## The Aftermath

I did file an issue on Github and it turns out it was because the documentation was based on the latest version of PyZX (the one in the Github repo) but that it hadn't been published to PyPI.

This means that when I `pip install`'d it I got an older version that was out of date. 

You can see the issue in its entirety [here](https://github.com/Quantomatic/pyzx/issues/42).

Looks like the updated version won't be pushed for a bit so in the meantime, I'll just install through a `git clone` and then `pip`.

__tl;dr__

* invoking a function without its arguments can be a neat way to get more information on how to properly call the function itself
* the Python `inspect` module is really neat for seeing the source code behinds all sorts of Python object
* I'm not insane!

## Why Bother Writing This Post?

I could have just made this post considerably shorter by linking the Github issue.

However, I felt that being able to document my thought process has the following benefits:

* It forces my brain to consolidate the new information I've learned (I have to explain it to an audience)
* I can refer back to this if I ever encounter a similar problem
* It can (potentially) be of assistance to other developers out there
* I just want more green squares on my Github profile