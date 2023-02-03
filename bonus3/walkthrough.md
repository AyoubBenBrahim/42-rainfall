



```
  0x08048502 <+14>:	mov    $0x80486f0,%edx
   0x08048507 <+19>:	mov    $0x80486f2,%eax
=> 0x0804850c <+24>:	mov    %edx,0x4(%esp)
   0x08048513 <+31>:	call   0x8048410 <fopen@plt>

(gdb) x/s $edx
0x80486f0:	 "r"
(gdb) x/s $eax
0x80486f2:	 "/home/user/end/.pass"
```









