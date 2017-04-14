% Software Guidelines for Non-Coders
% Jaime Silvela
% 2017-04-07 -ish

# 0.- Admit you own software
* You wrote more than 10 lines of code, and some people rely on them.
* … you own software. Care for it, or it will grow chaotic.
* Be aware of rising **technical debt**.

# 1.- Don’t be clever; be clear
> The best writing is rewriting.

E.B. White

> Simplicity does not precede complexity, but follows it.

Alan Perlis

> We should forget about small efficiencies, say about 97% of the 
> time: premature optimization is the root of all evil.

Donald Knuth

# 1.- Don’t be clever; be clear
* It’s not about using the quickest algorithm or the fanciest technology.
* It’s about mapping the problem domain into code as clearly as possible.
* Keep clarifying and cleaning your program as it evolves.
* Give good names to things.

# 2.- Focus on your data structures
* Loops and conditionals are a done deal — like bouncing the ball in basketball.
* Your data structures drive your algorithms.

> Show me your flowcharts and conceal your tables, and I shall continue
> to be mystified. Show me your tables, and I won’t usually need your 
> flowcharts; they’ll be obvious.

Fred Brooks in *The Mythical Man-Month* 
<br/>(Recommended book)

# 2.- Focus on your data structures
## Example 1/3
Design and algorithm to solve:

* Problem: given an array of numbers ```nums```, and a radius ```r```,
cluster the numbers in order to get a (kind-of) sparse histogram
with precision ```r```.
* We don't want a proper histogram, because most entries in the histogram
would be 0. We're likely to have fewer than 10 clusters, and want 1/100-th
precision.
* Example: ```nums``` = [1.2, 1.3, 5, 5.1, 5.5], ```r``` = 0.2 ➔ 1.2 × 2, 5 × 2, 5.5 × 1

# 2.- Focus on your data structures
## Example 2/3
I asked my engineering colleagues. Solutions proposed tended to center on signal
processing, kernels and convolutions or, generally, "matlabby" thinking.

# 2.- Focus on your data structures
## Example 3/3
Now, define:

	cluster {
		value float,
		count int
	}

Restate the problem: we want algorithm ```clusterNumbers```, such that

	clusterNumbers(nums float[], r float) ➔ cluster[]

Imagine that we have a cluster array. If we are given a new number
```x``` to cluster, do we know what to do?

	addToClusters(x float, r float, clusters cluster[]) ➔ cluster[]

Does this clarify the problem?


# 3.- Program with pen and paper
## or marker and whiteboard
## … some of the time
* De-focus from syntax and micro detail.
* Focus on high level, get a broad view.
* Iterate on design ideas in this medium.

# 4.- Diagram the flow of data in your program
## cf. 2 and 3
* Flow charts for loops/conditionals aren’t much use.
* Things like UML diagrams, again, not that useful.
* Your data flow tells you what needs to happen where.

# 5.- Separate interface from implementation
## ie. think of modules
* Think of each of your modules as a service to be called by others.
* What is relevant for the users? This is your interface.
* Keep your interface small and clean.

> Each module is designed to hide [a design decision] from the others.

David Parnas in *On the Criteria to Be Used in Decomposing Systems into Modules*
<br/>(Recommended paper)

# 5.- Separate interface from implementation
## Example 1/5
We're building a game with several kinds of enemy ships. We commision
Giulio to write a relativistic motion enemy. We commission Nuria to write
a magneto-hydrodynamic submarine enemy.

# 5.- Separate interface from implementation
## Example 2/5
Giulio's code:

	func GravityAt(...) {...}
	func Speed(...) {...}
	...

Nuria's code:

	func MagneticFieldAt(...) {...}
	func FriggingLaserBeam(...) {...}
	...

# 5.- Separate interface from implementation
## Example 3/5
The animation loop is very complex, and needs to know details of relativity
and magneto-hydrodynamics.

	func AnimationLoop() {
		forever {
			Sleep(deltaT)
			canvas = clearCanvas()
			for foe in AllFoes {
				if foe is Relativistic {
					foe.GravityAt(...)
					...
				} else if is MagnetoHydrodynamic {
					..
				}
			}
		}
	}

# 5.- Separate interface from implementation
## Example 4/5
Let's do it another way. The animation loop requires only that enemies be
able to compute their next state after some time has elapsed, and that
they be able to draw themselves on a canvas.

Giulio's new code: 

	func NextState(dt time.Interval) {...}
	func DrawOn(canvas Canvas) {...}
	-- Hidden ---
	func GravityAt(...) {...}
	...

Nuria's new code:

	func NextState(dt time.Interval) {...}
	func DrawOn(canvas Canvas) {...}
	-- Hidden ---
	func MagneticFieldAt(...) {...}
	...

# 5.- Separate interface from implementation
## Example 5/5
The new animation loop.

	func AnimationLoop() {
		forever {
			Sleep(deltaT)
			canvas = clearCanvas()
			for foe in AllFoes {
				foe.NextState(deltaT)
				foe.DrawOn(canvas)
			}
		}
	}

Everything is more robust. Nuria can change her implementation details if she
wants, as long as she upholds the interface. Giulio can use his code
for another simulation that expects the same interface.

# 6.- Separate data from presentation
* The core of your programs is the problem domain’s data and logic.
* Your presentation layer (UI’s, PDF’s …) should be detached from domain logic.
* Do the main computations in domain logic, have “dumb” presentation code.

# 7.- Inject your functional dependencies
* Don’t connect to a DB or other service in the middle of your code.
* Every function/object that uses an external service should name it as a parameter.
* I.e. the dependency gets injected.
* You should establish service connections very visibly in one place.

# 8.- Test in code. Don’t overdo it
* Test domain logic.
* Test interfaces.
* Testing low level implementation detail is not as clearly beneficial.

[James Coplien on unit testing](http://rbcs-us.com/documents/Why-Most-Unit-Testing-is-Waste.pdf)

# 9.- Limit your external dependencies
* You may like an external library that makes something easier for you.
* In a few months, it may be incompatible, bloated or vanished.
* Try to depend only on well established libraries.
* If you need only a tiny fraction of an external library, think of copying.
	(giving proper attribution of course)
* You may want to store your dependencies as part of your code repository.

[Three fallacies of dependencies](https://youtu.be/yi5A3cK1LNA)

# 10.- Use good tools
* Use version control. Even for small projects. Even for documentation.
* Use a good programming text editor. Eg: Visual Studio Code, Emacs, Atom.
* Use a font meant for coding. ```0 = O? 1 = l = I?```. Eg. Consolas, Source Code Pro.
* Use linters. Especially for dynamic languages (Javascript, Python, Ruby.)
* Collect notes, bugs, TODO items, in a file(s) where you look often.
* If you’re in a team, share that info. Maybe JIRA? (awful but useful)

# 11.- A few gotchas
* Floating point numbers are not exact. **0.2 + 0.1 ≠ 0.3**. Really.
* Text encoding: try to read/store all text as UTF-8. Verify.
* Case sensitivity; most modern stuff is case-sensitive. But …
* Case in-sensitive: SQL, Windows file names*, Fortran, Lisp.
* Line endings: Unix/Linux and Windows use different codes for newlines.
* “Smart” programs like Excel may modify your data.
