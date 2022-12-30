

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
