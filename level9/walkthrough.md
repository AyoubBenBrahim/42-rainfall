```
(gdb) set hei 40
(gdb) info func


0x08048530  operator new(unsigned int)
0x08048530  _Znwj@plt

0x080486f6  N::N(int)
0x080486f6  N::N(int)
0x0804870e  N::setAnnotation(char*)
0x0804873a  N::operator+(N&)
0x0804874e  N::operator-(N&)

```

for c++ calling convention, the this pointer is passed in ECX 
https://en.wikipedia.org/wiki/X86_calling_conventions#x86-64_calling_conventions

```
(gdb) heap
0x804a000:      0x00000000      0x00000071      0x08048848      0x41414141
0x804a010:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a020:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a030:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a040:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a050:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a060:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a070:      0x00000005      0x00000071      0x08048848      0x00000000
0x804a080:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a090:      0x00000000      0x00000000      0x00000000      0x00000000
```

offset 108

```
(gdb) p/d 0x804a078 - 0x804a00c
$3 = 108
```

```

=> 0x08048684 <+144>:	mov    0x14(%esp),%eax

(gdb) x $eax
0x8048848 <_ZTV1N+8>:	 ":\207\004\bN\207\004\b1N"

(gdb) x *$eax
0x804873a <_ZN1NplERS_>

(gdb) x $edx
0x804873a <_ZN1NplERS_>
```

```
      0x08048733 <+37>:	call   0x8048510 <memcpy@plt>
=> 0x08048738 <+42>:	leave
   0x08048739 <+43>:	ret
End of assembler dump.
(gdb) x/20wx 0x804a000
0x804a000:	0x00000000	0x00000071	0x08048848	0x41414141
0x804a010:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a020:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a030:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a040:	0x00000000	0x00000000	0x00000000	0x00000000
(gdb) i r
eax            0x804a00c	134520844
ecx            0x41414141	1094795585
edx            0x804a010	134520848

```

