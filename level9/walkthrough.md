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
