I was going through this famous paper "[Needle in a haystack](https://www.usenix.org/legacy/event/osdi10/tech/full_papers/Beaver.pdf)", as interesting the paper is I'd recommend it to be a must read for every computer science enthusiast.
The paper is around building an object storage system for storing Facebook's images. Key concerns of the authors revolved around the limitations of traditional filesystems and it was taking several disk operations to fetch a given photo.
It peaked my interest to delve into how the read from disk of a file actually works in LINUX so that I can actually understand the concerns authors are projecting in the paper. And hence this series of articles around Journey of a file in LINUX. This is starting part of the series more parts will follow as Files have many eccentricities which would need to be covered.

So what's a file, In Linux, a "file" is a fundamental unit of storage that contains data or information. There's a popular quote "On a UNIX system, everything is a file; if something is not a file, it is a process."  And just for clarification a "directory" is also a file in Linux, as it's just another file containing name of other files. So now we know since everything is a file let's see what all sorts of files are there:
- Directories
- Special files i.e. mechanism used for input and output like `/dev` 
- Links
- Sockets: Yes, sockets are also a type of file
- Named pipes: Acts as communication channel between processes without using network socket semantics.
The best way to view these types of files and their types would be using the command which every LINUX user learns first i.e. `ls` follow this command with a `-l` and voila you're able to view the all details of a file.

```shell
> ls -l
total 80
-rw-rw-r--   1 prasha   prasha   41472 Feb 21 17:56 Linux.doc
drwxrwxr-x   2 prasha   prasha    4096 Feb 25 11:50 courses
```

If we look at the above command output `-` represents a regular file while `d` is representation of directory. You might be wondering why other `-` are there those are part of file permissions and we don't need to look into that for now. Only the first character from `ls` output depicts what sort of file is that. Below is the list of different characters representing corresponding file types:
- `-`: Regular file
- `d`: Directory
- `l`: Symbolic link
- `c`: Character device file
- `b`: Block device file
- `p`: Named pipe (FIFO)
- `s`: Socket
Now since we have understood file types, we'll need how these files are created, how are they accessed, deleted etc. Will be following these up as part of other parts of this article.