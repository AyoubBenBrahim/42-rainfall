```
(gdb) set hei 40
(gdb) info func


0x08048530  operator new(unsigned int)
0x08048530  _Znwj@plt

0x080486f6  N::N(int)
0x080486f6  N::N(int)
0x0804870e  N::setAnnotation(char*)
0x0804873a  N::operator+(N&)
0x0804874e  N::operator-(N&)

```

for c++ calling convention, the this pointer is passed in ECX 
https://en.wikipedia.org/wiki/X86_calling_conventions#x86-64_calling_conventions

```
(gdb) heap
0x804a000:      0x00000000      0x00000071      0x08048848      0x41414141
0x804a010:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a020:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a030:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a040:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a050:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a060:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a070:      0x00000005      0x00000071      0x08048848      0x00000000
0x804a080:      0x00000000      0x00000000      0x00000000      0x00000000
0x804a090:      0x00000000      0x00000000      0x00000000      0x00000000
```

offset 108

```
(gdb) p/d 0x804a078 - 0x804a00c
$3 = 108
```

```

=> 0x08048684 <+144>:	mov    0x14(%esp),%eax

(gdb) x $eax
0x8048848 <_ZTV1N+8>:	 ":\207\004\bN\207\004\b1N"

(gdb) x *$eax
0x804873a <_ZN1NplERS_>

(gdb) x $edx
0x804873a <_ZN1NplERS_>
```

```
      0x08048733 <+37>:	call   0x8048510 <memcpy@plt>
=> 0x08048738 <+42>:	leave
   0x08048739 <+43>:	ret
End of assembler dump.
(gdb) x/20wx 0x804a000
0x804a000:	0x00000000	0x00000071	0x08048848	0x41414141
0x804a010:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a020:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a030:	0x00000000	0x00000000	0x00000000	0x00000000
0x804a040:	0x00000000	0x00000000	0x00000000	0x00000000
(gdb) i r
eax            0x804a00c	134520844
ecx            0x41414141	1094795585
edx            0x804a010	134520848

```








```
 0x080485f4 <+0>:     push   %ebp
   0x080485f5 <+1>:     mov    %esp,%ebp
   0x080485f7 <+3>:     push   %ebx
   0x080485f8 <+4>:     and    $0xfffffff0,%esp
   0x080485fb <+7>:     sub    $0x20,%esp
   0x080485fe <+10>:    cmpl   $0x1,0x8(%ebp)
   0x08048602 <+14>:    jg     0x8048610 <main+28>
   0x08048604 <+16>:    movl   $0x1,(%esp)
   0x0804860b <+23>:    call   0x80484f0 <_exit@plt>
   0x08048610 <+28>:    movl   $0x6c,(%esp)
   0x08048617 <+35>:    call   0x8048530 <_Znwj@plt>
   0x0804861c <+40>:    mov    %eax,%ebx
   0x0804861e <+42>:    movl   $0x5,0x4(%esp)
   0x08048626 <+50>:    mov    %ebx,(%esp)
   0x08048629 <+53>:    call   0x80486f6 <_ZN1NC2Ei>
   0x0804862e <+58>:    mov    %ebx,0x1c(%esp)
   0x08048632 <+62>:    movl   $0x6c,(%esp)
   0x08048639 <+69>:    call   0x8048530 <_Znwj@plt>
   0x0804863e <+74>:    mov    %eax,%ebx
   0x08048640 <+76>:    movl   $0x6,0x4(%esp)
   0x08048648 <+84>:    mov    %ebx,(%esp)
   0x0804864b <+87>:    call   0x80486f6 <_ZN1NC2Ei>
   0x08048650 <+92>:    mov    %ebx,0x18(%esp)
   0x08048654 <+96>:    mov    0x1c(%esp),%eax
   0x08048658 <+100>:   mov    %eax,0x14(%esp)
   0x0804865c <+104>:   mov    0x18(%esp),%eax
   0x08048660 <+108>:   mov    %eax,0x10(%esp)
   0x08048664 <+112>:   mov    0xc(%ebp),%eax
   0x08048667 <+115>:   add    $0x4,%eax
   0x0804866a <+118>:   mov    (%eax),%eax
   0x0804866c <+120>:   mov    %eax,0x4(%esp)
   0x08048670 <+124>:   mov    0x14(%esp),%eax
   0x08048674 <+128>:   mov    %eax,(%esp)
   0x08048677 <+131>:   call   0x804870e <_ZN1N13setAnnotationEPc>
   0x0804867c <+136>:   mov    0x10(%esp),%eax
   0x08048680 <+140>:   mov    (%eax),%eax
   0x08048682 <+142>:   mov    (%eax),%edx
   0x08048684 <+144>:   mov    0x14(%esp),%eax
   0x08048688 <+148>:   mov    %eax,0x4(%esp)
   0x0804868c <+152>:   mov    0x10(%esp),%eax
   0x08048690 <+156>:   mov    %eax,(%esp)
   0x08048693 <+159>:   call   *%edx
   0x08048695 <+161>:   mov    -0x4(%ebp),%ebx
   0x08048698 <+164>:   leave
   0x08048699 <+165>:   ret

This appears to be x86 assembly code for a program written in C or C++. It's difficult to say exactly what the program does without additional context and knowledge of the specific libraries and functions being called, but some observations can be made:

At the beginning of the code, the value of the base pointer (EBP) is being pushed onto the stack and then moved into the stack pointer (ESP) to set up a new stack frame.
EBX is then pushed onto the stack and ESP is aligned to a multiple of 16 bytes.
Subtracts 0x20 from ESP, which probably means it is allocating 0x20 bytes of space on the stack.
The instruction at 0x080485fe compares the value at memory location [EBP + 0x8] with the immediate value 1.
If the value is greater than 1, it jumps to an instruction at 0x08048610.
Instruction at 0x08048604 loads the value 1 into the memory location pointed by ESP which is the argument to the call to _exit@plt at 0x0804860b.
An instruction at 0x08048617 calls _Znwj@plt which is a memory allocation function.
There are more function calls like _ZN1NC2Ei, _ZN1N13setAnnotationEPc, etc.
Instruction at 0x08048698 pops the value of EBX and ESP is set to the value of EBP.
Finally, instruction at 0x08048699 returns to the caller.
This program likely contains various function calls, stack manipulation, and memory allocation. Without more context and information about the specific libraries and functions being called, it is difficult to say exactly what the program does.




بركة خان
 assembler code for function _ZN1N13setAnnotationEPc:
   0x0804870e <+0>:     push   %ebp
   0x0804870f <+1>:     mov    %esp,%ebp
   0x08048711 <+3>:     sub    $0x18,%esp
   0x08048714 <+6>:     mov    0xc(%ebp),%eax
   0x08048717 <+9>:     mov    %eax,(%esp)
   0x0804871a <+12>:    call   0x8048520 <strlen@plt>
   0x0804871f <+17>:    mov    0x8(%ebp),%edx
   0x08048722 <+20>:    add    $0x4,%edx
   0x08048725 <+23>:    mov    %eax,0x8(%esp)
   0x08048729 <+27>:    mov    0xc(%ebp),%eax
   0x0804872c <+30>:    mov    %eax,0x4(%esp)
   0x08048730 <+34>:    mov    %edx,(%esp)
   0x08048733 <+37>:    call   0x8048510 <memcpy@plt>
   0x08048738 <+42>:    leave
   0x08048739 <+43>:    ret
This appears to be x86 assembly code for a function named "_ZN1N13setAnnotationEPc" which is likely written in C++. Some observations can be made:

The function starts by setting up a new stack frame by pushing the value of the base pointer (EBP) onto the stack and moving the stack pointer (ESP) into EBP.
Subtracts 0x18 from the ESP, which probably means it is allocating 0x18 bytes of space on the stack.
The instruction at 0x08048714 loads the value of the first argument passed to the function (a pointer to a char) into EAX.
The instruction at 0x0804871a calls the strlen@plt function, passing the pointer in EAX as an argument. This probably calculates the length of a string.
The instruction at 0x0804871f loads the second argument passed to the function (a pointer to a char) into EDX.
The instruction at 0x08048722 adds 4 to EDX.
The instruction at 0x08048725 stores the length of the string into [ESP + 0x8], the third argument for the next function call.
The instruction at 0x08048729 loads the first argument passed to the function (a pointer to a char) into EAX, and the instruction at 0x0804872c loads the value of EAX into [ESP + 0x4], the second argument for the next function call.
The instruction at 0x08048730 loads the value of EDX into [ESP], the first argument for the next function call.
The instruction at 0x08048733 calls the memcpy@plt function, passing the three arguments on the stack. This probably copies a block of memory.
The instruction at 0x08048738 leaves the stack frame and restore the original value of ESP to EBP.
Finally, the instruction at 0x08048739 returns to the caller.
This function appears to be copying a string passed as an argument to another memory location, likely to set some sort of annotation. The function first uses strlen to get the length of the string and then memcpy to copy the string.




بركة خان
assembler code for function  N::operator+(N&):
   0x0804873a <+0>:     push   %ebp
   0x0804873b <+1>:     mov    %esp,%ebp
   0x0804873d <+3>:     mov    0x8(%ebp),%eax
   0x08048740 <+6>:     mov    0x68(%eax),%edx
   0x08048743 <+9>:     mov    0xc(%ebp),%eax
   0x08048746 <+12>:    mov    0x68(%eax),%eax
   0x08048749 <+15>:    add    %edx,%eax
   0x0804874b <+17>:    pop    %ebp
   0x0804874c <+18>:    ret
This appears to be x86 assembly code for a C++ member function named "N::operator+", which is likely an operator overload for the '+' operator for a class 'N'. Some observations can be made:

The function starts by setting up a new stack frame by pushing the value of the base pointer (EBP) onto the stack and moving the stack pointer (ESP) into EBP.
The instruction at 0x0804873d loads the value of the first argument passed to the function (a pointer to an object of class N) into EAX.
The instruction at 0x08048740 loads the value of the member variable at offset 0x68(%eax) into EDX.
The instruction at 0x08048743 loads the value of the second argument passed to the function (a pointer to an object of class N) into EAX.
The instruction at 0x08048746 loads the value of the member variable at offset 0x68(%eax) into EAX.
The instruction at 0x08048749 adds the values in EDX and EAX, and stores the result back in EAX.
The instruction at 0x0804874b pops the value of EBP, which removes the stack frame.
Finally, the instruction at 0x0804874c returns to the caller.
This function appears to be an operator overload for the '+' operator for class 'N'. It loads the value of member variable at offset 0x68 for both of the objects passed as arguments, adds them and returns the sum as the result.




بركة خان
comment this code
int32_t *eax= operator new(0x6c)
    
N::N (eax, 5)
    
int32_t *eax_1 = operator new(0x6c)
    
N::N( eax_1, 6)
    
N::setAnnotation( eax, argv[1])
    
return (**eax_1)(eax_1, eax)
This appears to be a code snippet written in C++. It does the following:

it creates a new block of memory of size 0x6c bytes using the operator new function and stores its address in the variable "eax".
it calls the constructor of the class N passing in the address of the block of memory stored in "eax" and an integer value of 5.
it creates another new block of memory of size 0x6c bytes using the operator new function and stores its address in the variable "eax_1".
it calls the constructor of the class N passing in the address of the block of memory stored in "eax_1" and an integer value of 6.
it calls the setAnnotation member function of class N passing in the address of the block of memory stored in "eax" and the first argument passed to the program (argv[1]).
it returns the result of calling the operator() member function of the object pointed to by "eax_1" passing in the address of the block of memory stored in "eax_1" and the address of the block of memory stored in "eax".
It is difficult to say exactly what the program does without more context and knowledge of the specific classes and functions being used, but it seems to be creating two objects of class N and passing them to some other function which then calls a member function on those objects.
```
