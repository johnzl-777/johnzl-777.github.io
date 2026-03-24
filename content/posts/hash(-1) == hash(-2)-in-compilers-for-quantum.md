+++
title = "hash(-1) == hash(-2) in Compilers for Quantum"
description = "A fun Python bug encountered in compiler development for quantum computing"
date = 2026-03-22
draft = false

[taxonomies]
tags = ["dev", "python", "quantum", "compiler"]

[extra]
lang = "en"
toc = true
comment = false
copy = true
math = false
mermaid = false
featured = false
reaction = false
+++
This post is about how I (quite painfully) suffered the consequences of the CPython `hash(-1) == hash(-2)` behavior.

If you know about this behavior already, I sincerely hope you’ve never encountered this in the wild and that this serves as a fun read on a consequence of it.

If you do NOT know about this behavior then let this post be a warning (and still be a fun read)!

## Context

{% note(title="Note") %}
While I bring up a couple quantum computing tools in this post this is primarily geared towards the compiler engineering side of things. Therefore it's safe to think of things like "Stim" as just a tool with its own DSL that I try to correctly produce from the DSL that lives inside the SDK I work on. 

If you _are_ well-versed in the quantum computing AND compiler space (quite a niche, but a growing one at that!) then I hope you will find even more entertainment from this post (:
{% end %}


One of the things I’ve been working on at [QuEra](https://github.com/QuEraComputing) is transpilation from our SQUIN (Structural Quantum Information) eDSL (embedded Domain Specific Language) to our own [Stim](https://www.github.com/quantumlib/Stim) eDSL, all of which lives inside the [Bloqade SDK](https://github.com/QuEraComputing/bloqade-circuit)[^1]. From there we can easily generate the Stim text format that Stim itself can consume. 

To be more precise, the workflow I target is the following:

1. The user writes a program in `squin` 
2. They apply a compiler pass (`SquinToStimPass`) to convert the program to the `stim` eDSL
3. They can now emit the text form of the `stim` eDSL program


You can see a small example of this workflow below, starting with the `squin` program:

```python
from bloqade import squin as sq

@sq.kernel
def example():
    # allocate 10 qubits
    qs = sq.qalloc(10)
    # apply H gate on all qubits
    sq.broadcast.h(qs)
    # apply some 1 and 2Q gates
    sq.z(qs[0])
    sq.y(qs[5])
    sq.broadcast.cx(controls=[qs[0], qs[1]], targets=[qs[3], qs[5]])
    # measure everything out
    sq.broadcast.measure(qs)
```

The eDSLs I mentioned as well as the ability to spit out files of other forms are all built on top of [Kirin](https://github.com/QuEraComputing/kirin), which handles lowering from Python to [SSA IR](https://en.wikipedia.org/wiki/Static_single-assignment_form) form as well as common compiler optimization passes. We can see what the SSA form looks like immediately by just invoking the `.print()` method all kernels have:

```
func.func @example() -> !py.NoneType {
  ^0(%example_self):
  │  %0 = py.constant.constant 10 : !py.int
  │ %qs = func.invoke qalloc(%0) : !py.IList[!py.Qubit, !Any] maybe_pure=False
  │  %1 = func.invoke h(%qs) : !py.NoneType maybe_pure=False
  │  %2 = py.constant.constant 0 : !py.int
  │  %3 = py.indexing.getitem(%qs, %2) : !py.Qubit
  │  %4 = func.invoke z(%3) : !py.NoneType maybe_pure=False
  │  %5 = py.constant.constant 5 : !py.int
  │  %6 = py.indexing.getitem(%qs, %5) : !py.Qubit
  │  %7 = func.invoke y(%6) : !py.NoneType maybe_pure=False
  │  %8 = py.constant.constant 0 : !py.int
  │  %9 = py.indexing.getitem(%qs, %8) : !py.Qubit
  │ %10 = py.constant.constant 1 : !py.int
  │ %11 = py.indexing.getitem(%qs, %10) : !py.Qubit
  │ %12 = py.ilist.new(values=(%9, %11)){elem_type=!Any} : !py.IList[!py.Qubit, Literal(2,int)]
  │ %13 = py.constant.constant 3 : !py.int
  │ %14 = py.indexing.getitem(%qs, %13) : !py.Qubit
  │ %15 = py.constant.constant 5 : !py.int
  │ %16 = py.indexing.getitem(%qs, %15) : !py.Qubit
  │ %17 = py.ilist.new(values=(%14, %16)){elem_type=!Any} : !py.IList[!py.Qubit, Literal(2,int)]
  │ %18 = func.invoke cx(%12, %17) : !py.NoneType maybe_pure=False
  │ %19 = func.invoke measure(%qs) : !py.IList[!py.MeasurementResult, ~N : !py.int] maybe_pure=False
  │ %20 = func.const.none() : !py.NoneType
  │       func.return %20
} // func.func example
```

If I now apply the `SquinToStimPass` we get the program in our `stim` eDSL form:

```python
from bloqade.stim.passes import SquinToStimPass

SquinToStimPass(dialects=example.dialects)(example)

example.print()
```

```
func.func @example() -> !py.NoneType {
  ^0(%example_self):
  │  %0 = stim.aux.constant.int 0 : !py.int
  │  %1 = stim.aux.constant.int 1 : !py.int
  │  %2 = stim.aux.constant.int 2 : !py.int
  │  %3 = stim.aux.constant.int 3 : !py.int
  │  %4 = stim.aux.constant.int 4 : !py.int
  │  %5 = stim.aux.constant.int 5 : !py.int
  │  %6 = stim.aux.constant.int 6 : !py.int
  │  %7 = stim.aux.constant.int 7 : !py.int
  │  %8 = stim.aux.constant.int 8 : !py.int
  │  %9 = stim.aux.constant.int 9 : !py.int
  │       stim.gate.H(targets=(%0, %1, %2, %3, %4, %5, %6, %7, %8, %9)){dagger=False : !py.bool}
  │       stim.gate.Z(targets=(%0)){dagger=False : !py.bool}
  │       stim.gate.Y(targets=(%5)){dagger=False : !py.bool}
  │       stim.gate.CX(targets=(%3, %5), controls=(%0, %1)){dagger=False : !py.bool}
  │ %10 = stim.aux.constant.float 0.0 : !py.float
  │       stim.collapse.MZ(p=%10, targets=(%0, %1, %2, %3, %4, %5, %6, %7, %8, %9))
  │ %11 = func.const.none() : !py.NoneType
  │       func.return %11
} // func.func example
```

Which you can then emit to text form:

```python
import io
from bloqade.stim import emit as emit_stim, main as main_dialect

def codegen(mt):
    buf = io.StringIO()
    emit = emit_stim.EmitStimMain(dialects=main_dialect, io=buf)
    emit.initialize()
    emit.run(mt)
    return buf.getvalue().strip()
    
print(codegen(example))
```

```
H 0 1 2 3 4 5 6 7 8 9
Z 0
Y 5
CX 0 3 1 5
MZ(0.00000000) 0 1 2 3 4 5 6 7 8 9
```
## The Problem

At some point as I was developing the `SquinToStimPass` I ran into a rather peculiar problem. 

Given a program that had `-1` and `-2` as Stim eDSL constant statements, when I ran the [CSE (Common Subexpression Elimination)](https://en.wikipedia.org/wiki/Common_subexpression_elimination) rewrite rule, `-1` would disappear only to be replaced by `-2` ! 

I’d have something in the original SQUIN language[^2] like:

```python
@sq.kernel
def example():
    qs = sq.qalloc(10)
    ms = sq.broadcast.measure(qs)
    
    # get the last two measurements which, in time, are the
    # most recent so per Stim convention should be accessible
    # via the record index through -1 and -2
    
    sq.set_detector([ms[-1], ms[-2]], coordinates=[0,0])
```

Which SHOULD have come out to the following text form from the Stim eDSL:

```
MZ(0.00000000) 0 1 2 3 4 5 6 7 8 9
DETECTOR(0, 0) rec[-1] rec[-2]
```

But instead I’d get

```
MZ(0.00000000) 0 1 2 3 4 5 6 7 8 9
DETECTOR(0, 0) rec[-2] rec[-2]
```

When I looked at the actual Stim eDSL output there was no `-1` constant statement to be seen either.

I was bewildered! Never in my career had I ever seen something so odd. At first I was convinced some rewrite rule *I* had created as part of the pass was the culprit but it eventually dawned on me that something at the Kirin level must be the culprit.

## The Bug

It wasn’t immediately apparent to me CSE was the problem but in retrospect, if I was even remotely aware Python (CPython to be exact) had this odd behavior[^3] I might have clocked it faster. 

{% important(title="Acknowledgements") %}
Before I go further I’d like to give kudos to my colleagues [Kai-Hsin Wu](https://github.com/kaihsin) and [Xiu-Zhe (Roger) Luo](https://rogerluo.dev/) who helped identify the specifics of the bug and also implemented the fix in Kirin to avoid future incidences of this. Thanks guys!
{% end %}

At a high level, the Kirin CSE rewrite rule at the time of the bug (Kirin v0.18.0) did the following:

1. Generate an `int` from Python’s built-in `hash` function applied on components of the statement including its type, arguments, attributes (which are compile-time values, like constants that are immediately available), etc.
2. Check if the `int` is present in a “`seen`” dictionary that records all statements that have been encountered up to that point.
3. If the `int` is present in `seen`, replace references to the current statement to the previous equivalent statement. 

The actual code we'll be focusing on from steps 1 and 2 looks like this:

```python
seen: dict[int, Statement]

	hash_value = hash(
		(type(stmt),)
		+ tuple(stmt.args)
		+ tuple(stmt.attributes.values())
		+ tuple(stmt.successors)
		+ tuple(stmt.regions)
	)
	
	if hash_value in seen:
		...
```

With the stim constant statements in the program I generated, the only part of those statements that was actually different between the two was the `stmts.attributes` which would contain the original `-1` and `-2` Python integers.

When we started digging deeper we ran into this peculiar behavior:

```python
>>> hash(1)
1
>>> hash(0)
0
>>> # okay, nothing fishy with hashing positive integers...
>>> hash(-1)
-2
>>> # wait, what?
>>> hash(-2)
-2 
>>> hash(-1) == hash(-2)
True
>>> # alright I give up.
```

Thus when you invoke CSE on something like:

{% detail(title="Simplified SSA Format Ahead!", default_open=false) %}
The SSA form shown here is simplified from what you would actually see, there are some intermediate statements I've cut out to keep things concise here.

If you're really curious, there's a `stim.aux.get_rec` statement that accepts the negative integer first, and THEN feeds into a `detector` or `observable` statement. 
{% end %}

```
...
%0 = stim.aux.constant.int -1 
%1 = stim.aux.constant.int -2 
%2 = stim.aux.detector(record_indices = (%0, %1))
...
```

You end up getting:

```
...
%0 = stim.aux.constant.int -1
%1 = stim.aux.constant.int -2
# notice reference to %0 dropped in favor of %1!
%2 = stim.aux.detector(record_indices = (%1, %1)) 
...
```

And when [DCE (Dead Code Elimination)](https://en.wikipedia.org/wiki/Dead-code_elimination) gets run, -1 just vanishes altogether:

```
# bye-bye -1 ):
%1 = stim.aux.constant.int -2
%2 = stim.aux.detector(record_indices = (%1, %1))
```

## Origins

Some googling about this odd behavior led me to [Omair Majid’s blog post](https://omairmajid.com/posts/2021-07-16-why-is-hash-in-python/) which dives into the CPython source code to investigate. Omair points out that `-1` is specifically avoided as a valid returnable hash value in the implementation because it’s used in C to indicate something went wrong.

If you’re like me and wondering that there has to be a better way, there's a cited Reddit answer that points out C does not have the ability to throw exceptions.

If you’re *still* like me and think there has to be some way around it, [there is](https://stackoverflow.com/a/2891916)! But I don’t think this would be wise to have in the CPython codebase (at least, not in this context)…

{% tip(title="Idiomatic C") %}
My colleague [Phillip Weinberg](https://github.com/weinbe58) (who has much more experience working in C/C++ than I do) pointed out to me the `setjmp` + `longjmp` combo linked above is considered idiomatic and can be useful if a problem occurs and you want to immediately go straight to cleaning things up. 
{% end %}

## The Fix

I had to do some digging on the Kirin GitHub PRs and Slack but the [initial fix for this problem was made by Roger](https://github.com/QuEraComputing/kirin/pull/412/files) and it was just to add some additional logic to the statement attribute `__hash__`. The additional logic literally being: 

```python
# in src/kirin/ir/attrs/py.py
def __hash__(self):
	if isinstance(self.data, int):
		return self.data
	# default attribute hash behavior continued if not int
	return hash(self.data) + hash(self.type)
```

This wasn’t the complete solution though and if we jump to Kirin v0.18.1[^4] (the patch version immediately after the bug was found) the final fix was made a bit more robust (albeit with more overhead for me to wrap my head around).

First, the attribute `__hash__` returns to this definition:

```python
# in src/kirin/ir/attrs/py.py
def __hash__(self) -> int:
	return hash((self.type, self.data))
```

BUT, on the CSE side:

1. Instead of generating an `int` as a key into the `seen` dictionary there’s now an `Info` class that consumes pretty much what you saw before to instantiate it and is used as a key instead.
2. `Info` has its own `__hash__` definition that uses the Python build-in `id` on individual elements that would have just been `hash`’ed before. This is a bump up from the old implementation where `hash` was just called directly on things belonging to the statement.

The logic looks something like this:

```python
# in src/kirin/rewrite/cse.py, as part of the `Info` class

def __hash__(self) -> int:
	return hash(
		(id(self.head),)
		+ tuple(id(ssa) for ssa in self.args)
		  # Hmm something feels off about hash(attr)...
		+ tuple(hash(attr) for attr in self.attributes)
		+ tuple(id(succ) for succ in self.successors)
		+ tuple(id(region) for region in self.regions)
	)

```

If you think carefully though this still runs afoul of the identical hash problem that caused this bug in the first place! Indeed, if I take the program from the [Problem section](#the-problem) but run it through CSE after rewriting it:

```python
from bloqade import squin as sq
from bloqade.stim.passes import SquinToStimPass
from kirin.rewrite import Walk, CommonSubexpressionElimination

@sq.kernel
def example():
    qs = sq.qalloc(10)
    ms = sq.broadcast.measure(qs)
    sq.set_detector([ms[-1], ms[-2]], coordinates=[0,0])

SquinToStimPass(dialects=test.dialects)(test)
# SquinToStimPass already has CSE applied inside of it, 
# but I'm doing it again outside for clarity.
Walk(CommonSubexpressionElimination()).rewrite(test.code)
```

(With some `print` statements inside CSE rewrite rule for debugging), the `Info`-generated hash ends up being exactly the same for the `-1` and `-2` constant statements.

How can it be the case that CSE behaves correctly this time though? 

`Info` has one additional magic/”dunder” method that’s been explicitly implemented, which is `__eq__`. When two Python dictionary keys with identical hashes are encountered there’s a fallback to `__eq__`[^5]. `__eq__` in `Info` explicitly looks at the equality between the same elements that went into producing the custom hash. 

Here it should be clear that `False` gets spit out between the `-1` and `-2` constant statements and CSE doesn’t touch anything.

The `__eq__` implementation looks like this: 

```python
def __eq__(self, other: object) -> bool:
	if not isinstance(other, Info):
		return False

	return (
		self.head == other.head
		and self.args == other.args
		# attribute comparison immediately gives False with
		# the ones in our program.
		and self.attributes == other.attributes
		and self.successors == other.successors
		and self.regions == other.regions
	)
```

## Conclusion

If you’re as boggled as I am that `hash(-1) == hash(-2)` has just been a thing in Python, welcome to the club 😅

While the process of writing this post was a lot of fun[^6] I’m certainly glad this is no longer an issue. I do admit feeling a bit of nostalgia seeing how much Kirin has grown and once again, I’m grateful to work on a team where things like this can be quickly hammered out.

In the grander scheme of things this was also a humbling reminder to me that while current Quantum Computers are noisy and prone to error, even in the domain of classical computation there’s still plenty of problems to be found (:

*If you'd like to leave a comment or have a suggestion/concern, please feel free to reach out to me via my e-mail at: (my first name)z(first letter of my last name).dev@gmail.com*

{% detail(title="Changelog", default_open=false) %}

Mar 23, 2026
- Added additional note from talking with Phillip Weinberg on idiomatic C
- Make it clearer Kai-Hsin Wu and Xiu-Zhe (Roger) Luo also helped with finding the bug
- Added missing contact information
- Got rid of the first line of the post, kind of spoiled the conclusion :P

{% end %}


[^1]: The Bloqade SDK is really a namespace package. The eDSLs I mention here all belong in the `bloqade-circuit` package that gets pulled in.
[^2]: The program I cooked up here should be compatible with the latest version of `bloqade-circuit` (v0.13.1). The syntax here is actually slightly different from what I remember working with because back then tuples with constants (ex: `(1,5)`) were the way to go for coordinates. Now lists are preferred, especially considering they offer more flexibility for construction/manipulation.
[^3]: I actually wonder if [other Python implementations](https://www.python.org/download/alternatives/) have this behavior as well but I haven’t confirmed it.
[^4]: The latest version of Kirin (v0.22.8 as of the time of writing this) has even more logic in CSE  but the core remains pretty much the same.
[^5]: [Hynek Schlawack’s](https://hynek.me/articles/hashes-and-equality/) and [Roy Williams’](https://eng.lyft.com/hashing-and-equality-in-python-2ea8c738fb9d) posts on the topic were especially helpful in understanding this behavior!
[^6]: Ha! You thought I was going to refute that in this footnote, but no (: It was however, also a decent amount of work. I wasn’t the most familiar with the `__eq__` fallback behavior before writing this and caught onto it with the two blog posts above as well as help from an embedded `print(f"eq invoked on {obj} against {self}")` statement. 