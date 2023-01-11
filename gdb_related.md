

Getting inputs from stdin:

The other option is to pipe the output of a command to the stdin of the program

$> python -c 'print("\xef\xbe\xad\xde")' | ./program

In gdb we can use the bash process substitution <(cmd) trick:

(gdb) run < <(python -c 'print("\xef\xbe\xad\xde")')

This way is much quicker than effectively creating a named pipe and branch your program on it. Creating the named pipe outside of gdb requires a lot of unnecessary steps where you have it instantly with the previous technique.

Note also that, some people are using <<$(cmd) like this:

(gdb) run <<< $(python -c 'print("\xef\xbe\xad\xde")')

But, this last technique seems to filter out all NULL bytes (for whatever reason), so you should prefer the first one (especially if you want to pass NULL bytes).

https://reverseengineering.stackexchange.com/questions/13928/managing-inputs-for-payload-injection

https://bitvijays.github.io/LFC-BinaryExploitation.html#appendix-i-gdb-basics

Keep the stdin open after injection

$> (cat ./mycommands.txt; cat) | ./program

Or, if you want a shell command:

$> (python -c 'print("\xef\xbe\xad\xde")'; cat) | ./program

https://bitvijays.github.io/LFC-BinaryExploitation.html#appendix-i-gdb-basics



(gdb) info breakpoints

(gdb) info func

p $ebp - 0x4c

define hook-stop

>x/100wx $esp

>x/10i $eip

>end

(gdb) ni

(gdb) p /x $eax



(gdb) overflow the binary

(gdb) info frame

 Saved registers: eip at 0xbffff700

x/24wx $esp

p/d eip - xyz



info registers

esp            0xbffff5d8

x/s 0xbffff5d8 ==> content 


If you do not exit GDB when you change and recompile your code, the next time you issue GDB’s run command, GDB will sense that your code has changed and automatically reload the new version.


(gdb) break foo

Breakpoint 2 at

You can delete that breakpoint either with

(gdb) clear foo

(gdb) delete 2



There are three classes of methods for resuming execution. The first involves “single stepping” through your program with step and next, executing only the next line of code and then pausing again. The second consists of using continue, which makes GDB unconditionally resume execution of the program until it hits another breakpoint or the program finishes. The last class of methods involves conditions: resuming with the finish or until commands. In this case, GDB will resume execution and the program will run until either some predetermined condition is met (e.g., the end of a function is reached), another breakpoint is reached, or the program finishes.

 you can use next and step to execute just the very next line of code. After the line is executed, GDB will again pause and give a command prompt.

GDB is now at line 7 of the program, meaning that line 7 has not been executed yet.


int *x;

...

x = (int *) malloc(25*sizeof(int));

If you wanted to print out the array in GDB, you could not type

(gdb) p x

This would simply print the address of the array. Nor could you type

(gdb) p *x

That would print out only one element of the array, x[0]. 



In some cases, you might wish to examine memory at a given address, rather than via the name of a variable. GDB provides the x (“examine”) command for this purpose. 

(gdb) p/x y

will display the variable y in hex format instead of decimal. Other commonly used formats are c for character, s for string, and f for floating-point.


08048360<fgets@plt>

the call to fgets() leads to 8048360, which is the PLT jump table entry for fgets(). As we can see, there is an indirect jump to the address stored at 0x804a000 in the preceding disassembled code output. This address is a GOT (Global offset table) entry that holds the address to the actual fgets() function in the libc
shared library.

This address is not the address to the fgets() function since it has not yet been resolved by the linker, but instead points back down into the PLT entry for fgets().

==


GOT The Global Offset Table,

print instructions

x/4i 0x45855


==

 une fois que le programme rentre dans la fonction, il va devoir se souvenir d’où il vient. Et pour cela, il va falloir qu’il enregistre le registre EIP
 c’est que l’instruction call est un alias des deux instructions suivantes :

call <adresse>
 
; est un alias de
 
PUSH EIP
 
JMP <adresse>

 ==
 

‘x/4xw $sp’ 
 
 prints the four words (‘w’) of memory above the stack pointer 

you could print the program counter in hex with
 
p/x $pc
 
or print the instruction to be executed next with
 
x/i $pc


(gdb) print system
 
$2 = {<text variable, no debug info>} 0x8048360 <system@plt>


r <<< "$(perl -e 'print "A"x200')"

define hook-stop
      
disass main
      
r i
      
x/40wx $esp
      
end

(gdb) x/s  0xbffff77c
      
0xbffff77c:      'A' <repeats 44 times>
 
(gdb) p (char *) 0xbffff77c
 
$18 = 0xbffff77c 'A' <repeats 44 times>

ناقص كتزيد لقدام
 
زائد كترجع للور
 
 x/20s $esp

x : eXamine
 
20s : 20 values in string
 
$esp : for register ESP (Stack Pointer)
 
 
 
 ==
 "The PLT is the glue that holds a library function to a binary."
 
 https://web.archive.org/web/20120622232123/http://www.mentalcases.net/texts/security/TextSectionStudy.txt
 
 ==
 MyFunction2(10, 5, 2);
 
 
|  2 | [ebp + 16] (3rd function argument)
 
|  5 | [ebp + 12] (2nd argument)   ..... argv1
 
| 10 | [ebp + 8]  (1st argument)    .... argc
 
| RA | [ebp + 4]  (return address)
 
 
