- Concurrency whilst waiting doing something else, includes parallelism
- Parallelism using multiple cores to complete a task
- Parallel streams supposed to be used for manipulating data/calculations, not supposed to be done for IO.
  ![[Pasted image 20240424195350.png]]
  ![[Pasted image 20240424200200.png]]
  - Concurrency is hard to get right, lot synchronised can lead to deadlock and resource contentions.
  - Few laws to keep in mind while writing concurrent code:
	  1. The Law of the Sabotaged Doorbell - Instead of arbitrarily suppressing interruptions, manage them better. 
	  2. The Law of the Distracted Spear fisherman - Focus on one thread at a time. Don't spin up too many threads at once unnecessarily and keep track of Platform threads. Tough to be done with Virtual Threads now as there can be millions of them 😅. `jstack` tool can be used to get thread dumps.
	  3. The Law of the Overstocked Haberdashery – Having too many threads is bad for our application. Performance will degrade and debugging will become difficult.
	  4. The Law of the Blind Spot – 
	  5. The Law of the Leaked Memo – 
	  6. The Law of the Corrupt Politician – 
	  7. The Law of the Micromanager – 
	  8. The Law of Cretan Driving – 
	  9. The Law of Sudden Riches – 
	  10. The Law of the Uneaten Lutefisk