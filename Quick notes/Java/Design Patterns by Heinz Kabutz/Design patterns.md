- Getters and setters tend to break encapsulation if not careful, It limits how we can change class structure going forward. Rather think what instruction can be sent to do using a method call like request opposed to using getters.
- How do you measure coupling and cohesion? What's the difference between them?
- Cyclometric complexity - Measures cost of 
- Writing an interface/factory for every package and only those are public. Everything else is package private. if I then have 2 (public) interfaces in a package which don't interact with each other, it's my cue to break into 2 packages. that is my poor man's measure of cohesion.
  Avoid having lot of details in interfaces and utilise the Java modules.
- Thread safe singleton via Java class loader, also does lazy initialization:
	- 
	  ![[Pasted image 20240404202444.png]]
	- Classloading takes the lock here and main thread can't continue because of `.join()` as it's waiting static block to finish!!!???
	  ![[Pasted image 20240404203623.png]]
- should we always use builders for every object/class/model or it's for only complex objects? 
- Why doesn't Java support default values for parameters?  Chose not to because of complexity in C++. Kotlin does have it.
  Java made decision to not make name of parameters significant, the JDK 
  // Add code
- https://www.javaspecialists.eu/archive/Issue123-Strategy-Pattern-with-Generics.html - Polymorphic builder to use when we use subclass and want 