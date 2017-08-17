# ToarouExps

Experiments for ToaruOS (とあるOS), how to make a elf *"static"* binary.


hello.c
```c
#include "syscall.h"
#include "syscall_nums.h"

DEFN_SYSCALL3(SYS_WRITE, int, char *, int);

void _start()
{
	syscall_write(1, "Hello World\n", 12);
}
```

```
$ gcc hello.c -c
$ ld hello.o
$ ./a.out
```
