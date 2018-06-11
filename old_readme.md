# scalable_bash_programs
What you need to know to build scalable bash programs. A few lessons learned.

source [wikipedia](https://en.wikipedia.org/wiki/Ousterhout%27s_dichotomy)
<pre>
Ousterhout's dichotomy:
System programming languages tend to be used for components and applications with large amounts of internal functionality such as operating systems, database servers, and Web browsers. These applications typically employ complex algorithms and data structures and require high performance. Prototypical examples of system programming languages include C and Modula-2.

Scripting languages tend to be used for applications where most of the functionality comes from other programs (often implemented in system programming languages); the scripts are used to glue together other programs or add additional layers of functionality on top of existing programs. Ousterhout claims that scripts tend to be short and are often written by less sophisticated programmers

Criticism:
Many believe that the dichotomy is highly arbitrary, and refer to it as Ousterhout's fallacy or Ousterhout's false dichotomy.
</pre>

The Elephant in the room has been adressed.

Most operating systems are aggreable to bash so interesting programs can 

Structured programs can be built using bash. 
<br/>However, a number of challenges must be addressed and resolved.

## Getting past the first file, calling other scripts:
Running a script from afar is easy, 
The first challenge a developer runs into is trying to run another script that is nearby the script you are running from afar. 

**Unfinished in favor of the main readme.md**
