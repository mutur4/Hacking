
>## Heap Introduction

- The heap this is a region that is used to allocate memory at runtime.
- There are 2 basic functions that make up the foundation of the heap manipulation in c 
and they are the following.


1. malloc()
	* prototype `void malloc(size_t)`
	* This is a function that takes the size to be allocated as the argument and 
	returns a pointer to the allocated memory on the heap.
	* The function returns a void pointer therefore typecasting is encouraged =).
	
2. free():
	* prototype `void free(* ptr)`
	* This is a function that takes the pointer returned by malloc as the argument
	and frees or releases that region of memory.
	* The function returns _NULL_


### Defination of basic terms

* __program break/brk__: This is a pointer that points to the end of the heap segment
* **end_data**: This is a pointer that points to the end of the data segment
* **start_brk** : This is a pointer that points to the start of the heap segment.
* __heap Segment__ : This is a region of memory that contains the heap and all its structures this is similar to the data segment, stack.
* __arena__ : This is a contigous region of the heap memory.This is th extra space that is returned when the first call to `malloc()` is initiated. 
* __Allocators__: These are algorithms that interact with the kernel via syscalls to allocate space at runtime.
	- There are different allocators and they include but not limited to the following:
	> * DlMalloc: This is the general purpose allocator
	>
	> * PtMalloc: (ptmalloc2) this is the default allocator for glibc
	>
	> * Jemalloc: This is used in FreeBSd and Firefox 
	>
	> * TcMalloc: This is used by GoogleChrome.
	>
	> * Magazine Malloc: This is used by IOS/OSX
	
	- These allocators communicate with the operating system using syscalls. Some of the syscalls are listed below.


* syscalls include 
	1. brk() syscall:
		- This is a pointer that is used to initizalid the heap segment
		- This is done by initializing the `end_data && start_brk = brk`
	2. sbrk() syscall: 
		- This is syscall that is used to increment the brk pointer
		- This syscall called with an argument of (0) can be used to return the
		current position of the program break pointer.

> TL;DR
>
> * When a call to `malloc()` is made, you will noticed that that the size retured back to the 
> user is somehow higher than what was requested.
>
> * This is because of the extra chunk metadata and the size that is returned by malloc 
> can be calculated using the following formulae.

```
- Assume we request a size of (n) bytes from malloc in x64 bit systems.
	if (n < 0x20):
		malloc_return == 0x20
	else:
		malloc_return == (n + 0x8 + 0xf) &~ 0xf
```


*When a call is made to `malloc()`, the size should be aligned to the `SZ_SIZE` value that is 0x4 in 32-bit systems and 0x8 in 64-bit systems
Malloc will allocate that size plus the size of the chunk metadata that is 0x8 in 32-bit systems and 0x10 in 64-bit systems 🥶* 
