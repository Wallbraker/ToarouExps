# ToaruExps

Experiments for ToaruOS (とあるOS), how to make a elf *"static"* binary.

hello.c
```c
#define SYS_WRITE 4
#define DECL_SYSCALL3(fn,p1,p2,p3)       int syscall_##fn(p1,p2,p3)
#define DEFN_SYSCALL3(fn, num, P1, P2, P3) \
	int syscall_##fn(P1 p1, P2 p2, P3 p3) { \
		int __res; __asm__ __volatile__("push %%ebx; movl %2,%%ebx; int $0x7F; pop %%ebx" \
				: "=a" (__res) \
				: "0" (num), "r" ((int)(p1)), "c"((int)(p2)), "d"((int)(p3))); \
		return __res; \
	}


static DECL_SYSCALL3(write, int, char *, int);

void _start()
{
	syscall_write(1, "Hello crossing worlds!\n", 23);
}

static DEFN_SYSCALL3(write, SYS_WRITE, int, char *, int);
```

On the host that has clang and lld installed do:
```bash
$ clang -target "i686-pc-none-elf" -O3 -c hello.c
$ ld.lld hello.o
$ python -m SimpleHTTPServer 8000
```

In the VM do:
```bash
$ fetch -Ov http://10.0.2.1:8000/a.out
$ ./a.out
Hello crossing worlds!
```
