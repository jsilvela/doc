% Software Guidelines for Non-Coders
% Jaime Silvela
% 2017-04-07 -ish

# 0.- Admit you own software
* You wrote more than 10 lines of code, and some people rely on them.
* … you own software. Care for it, or it will grow chaotic.
* Be aware of rising **technical debt**.

# 1.- Don’t be clever; be clear
* It’s not about using the quickest algorithm or the fanciest technology.
* It’s about mapping the problem domain into code as clearly as possible.
* Keep clarifying and cleaning your program as it evolves.
* Give good names to things.

> The best writing is rewriting.

E.B. White

> Simplicity does not precede complexity, but follows it.

Alan Perlis

> We should forget about small efficiencies, say about 97% of the 
> time: premature optimization is the root of all evil.

Donald Knuth

# 3.- Focus on your data structures
* Loops and conditionals are a done deal — like bouncing the ball in basketball.
* Your data structures drive your algorithms.

> Show me your flowcharts and conceal your tables, and I shall continue
> to be mystified. Show me your tables, and I won’t usually need your 
> flowcharts; they’ll be obvious.

Fred Brooks in *The Mythical Man-Month* 
<br/>(Recommended book)

# 4.- Program with pen and paper,
# or marker and whiteboard
## … some of the time
* De-focus from syntax and micro detail.
* Focus at a higher level, get a broader view.
* Iterate on design ideas in this medium.

# 5.- Diagram the flow of data in your program
## cf. 3 and 4
* Flow charts for loops/conditionals aren’t much use.
* Things like UML diagrams, again, not that useful.
* Your data flow tells you what needs to happen where.

# 6.- Separate interface from implementation
## ie. think of modules
* Think of each of your modules as a service to be called by others.
* What is relevant for the users? This is your interface.
* Keep your interface small and clean.

> Each module is designed to hide [a design decision] from the others.

David Parnas in *On the Criteria to Be Used in Decomposing Systems into Modules*
<br/>(Recommended paper)

# 2.- Separate data from presentation
* The core of your programs is the problem domain’s data and logic.
* Your presentation layer (UI’s, PDF’s …) should be detangled from domain logic.
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

[Talk with James Coplien and Bob Martin on TDD](https://youtu.be/KtHQGs3zFAM)

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
* Use a good programming text editor. Eg: Emacs, Visual Studio Code, Atom.
* Use a font meant for coding. ```0 = O? 1 = l?```. Eg. Consolas, Source Code Pro.
* Use linters. Especially for dynamic languages (Javascript, Python, Ruby.)
* Collect notes, bugs, TODO items, in a file(s) where you look often.
* If you’re in a team, share that info. Maybe JIRA? (awful but useful)

# 11.- A few gotchas
* Floating point numbers are not exact. **0.2 + 0.1 ≠ 0.3**
* Text encoding: try to read/store all text as UTF-8. Verify.
* Case sensitivity; most modern stuff is case-sensitive. But …
* Case in-sensitive: SQL, Windows file names*, Fortran, Lisp.
* Line endings: Unix/Linux and Windows use different codes for newlines.
* “Smart” programs like Excel may modify your data.
