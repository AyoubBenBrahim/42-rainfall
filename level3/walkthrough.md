

fuzzing or fuzz testing = testing technique that involves providing invalid, unexpected, or random data as inputs to a computer program = The program then crashes

retrieve five parameters from the stack and display them as 8-digit padded hexa

level3@RainFall:~$ ./level3

%08x %08x %08x %08x %08x

00000200 b7fd1ac0 b7ff37d0 78383025 38302520

Uncontrolled format string

"One may also write arbitrary data to arbitrary locations using the %n format token, which commands printf() and similar functions to write the number of bytes formatted to an address stored on the stack."

wiki

%n is a special format specifier which instead of printing something causes printf() to load the variable pointed by the corresponding argument with a value equal to the number of characters that have been printed by printf()  before the occurrence of %n
