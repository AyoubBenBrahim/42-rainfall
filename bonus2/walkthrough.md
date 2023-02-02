
0x08048484  greetuser


rep stos %eax,%es:(%edi)   <====> memset(buffer, 0, sizeof(buffer));

The "rep" prefix specifies that the instruction should be repeated multiple times,

while the "stos" instruction stores the value in the %eax register to the destination pointed to by %es:%edi.

In other words, this instruction performs a repeating store of the value in the %eax register 
to consecutive memory locations starting from the address specified by %es:%edi.

This is a kind of memset on the buffer start at address esp+0x50












