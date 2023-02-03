

rep stos %eax,%es:(%edi)   <====> memset(buffer, 0, sizeof(buffer));

The "rep" prefix specifies that the instruction should be repeated multiple times,

while the "stos" instruction stores the value in the %eax register to the destination pointed to by %es:%edi.

In other words, this instruction performs a repeating store of the value in the %eax register 
to consecutive memory locations starting from the address specified by %es:%edi.

This is a kind of memset on the buffer start at address esp+0x50

```
(gdb) x/4wx $esp+0x50
0xbffff680:	0x08048287	0x00000000	0x00c30000	0x00000001

(gdb) rep stos %eax,%es:(%edi) 

(gdb) x/4wx $esp+0x50
0xbffff680:	0x00000000	0x00000000	0x00000000	0x00000000
```

```
mov    0xc(%ebp),%eax
add    $0x8,%eax
mov    (%eax),%eax

movl   $0x20,0x8(%esp)
mov    %eax,0x4(%esp)

lea    0x50(%esp),%eax
add    $0x28,%eax
mov    %eax,(%esp)
call   0x80483c0 <strncpy@plt>


movl   $0x20, 0x4(%esp)   ; maximum number of characters to copy (n)
mov    %eax, (%esp)       ; source buffer (src)
lea    0x50(%esp), %eax   ; destination buffer (dest)

The parameters are passed in the following order:
dest: 0x50(%esp)
src: %eax
n: 0x20

strncpy((char*)0x50(%esp) + 0x28, (const char*)%eax, 0x20);


buffer = 0x50
strncpy(buffer + 0x28, argv[2], 0x20);
```

```
run AAAA BBBB

0xbffff690:	0x41414141	0x00000000	0x00000000	0x00000000
0xbffff6a0:	0x00000000	0x00000000	0x00000000	0x00000000
0xbffff6b0:	0x00000000	0x00000000	0x42424242	0x00000000
0xbffff6c0:	0x00000000	0x00000000	0x00000000	0x00000000
0xbffff6d0:	0x00000000	0x00000000	0x00000000	0xbfffff05

(gdb) p/d 0xbffff6d8 - 0xbffff690
$2 = 72
(gdb) p/d 0xbffff6d8 - 0xbffff6b8
$3 = 32

```








