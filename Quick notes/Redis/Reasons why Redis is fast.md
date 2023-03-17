- https://levelup.gitconnected.com/4-reasons-why-single-threaded-redis-is-so-fast-414e0106f921
1. In-memory data storage
2. Optimised data structure
3. Single threaded architecture
4. Non-blocking IO

- Things to look at:
	- Single threaded even loop (I/O Multiplexing)
	- Reactor pattern