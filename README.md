# ToarouExps

Experiments for ToaruOS (とあるOS), how to make a elf *"static"* binary.


hello.c
```c
#include "syscall.h"
#include "syscall_nums.h"

void _start()
{
	syscall_write(1, "Hello World\n", 12);
}

DEFN_SYSCALL3(write, SYS_WRITE, int, char *, int);
```

```
$ gcc -m32 -O3 hello.c -c
$ ld hello.o
$ ./a.out
```
