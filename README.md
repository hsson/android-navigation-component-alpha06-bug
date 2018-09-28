# Bug in Android Navigation Component (alpha06)
(Note: I have only verified this in alpha06, I don't know if it exists in
previous versions)

## The problem
Basically the problem is that the NavOption "clearTask" was removed in 
alpha02, and the recommended substitute was to use app:popUpTo pointing to
the root of the graph/the graph ID, together with app:popUpToInclusive="true".
However, this method does not work. It only seems to work for actions
going from the graph's start destination. If we have the following graph:

```
   x      y 
A ---> B ---> C
```

Where A, B, and C are fragments, A is the start destination of the grapg,
and x, y are actions going from A to B and B to C respectively. Let's say
the graph's ID is "@+id/graph"


Then, if both actions x and y have
```
	app:popUpTo="@+id/graph"
	app:popUpToInclusive="true"
```

Then when navigating from A to B through x, and then hitting the back button
will exit the app (as expected). But if navigating from A to B to C through
x and y, and then hitting the back button we will navigate back to B — this
is not what was expected. The expected behavior with this graph is to always
exit the app when clicking the back button

## Environment
This was verified using:
```
Android Studio 3.2 RC 3
Build #AI-181.5540.7.32.4987877, built on August 31, 2018
JRE: 1.8.0_152-release-1136-b06 x86_64
JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
macOS 10.13.6
```
