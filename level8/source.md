


```
   0x08048575 <+17>:	mov    SERVICE***,%ecx
   0x0804857b <+23>:	mov    AUTH**,%edx

   0x08048586 <+34>:	mov    %ecx,0x8(%esp) SERVICE***
   0x0804858a <+38>:	mov    %edx,0x4(%esp) AUTH**
 
  
   0x080485ae <+74>:	call   0x8048440 <fgets@plt>

   0x080485bb <+87>:	lea     INPUT*** ,%eax
   0x080485bf <+91>:	mov    %eax, INPUT***


   0x080485cb <+103>:	mov     INPUT***, INPUT***

   0x080485cf <+107>:	repz cmpsb %es:("auth "),%ds:( INPUT***)
 
AUTH** = MALLOC(4)


   0x0804862a <+198>:	lea    INPUT*** ,%eax
   0x0804862e <+202>:	lea    0x5(%eax),INPUT***
   0x08048631 <+205>:	mov    AUTH**, %eax
   0x08048636 <+210>:	mov    INPUT***, 0x4(%esp)
   0x0804863a <+214>:	mov    AUTH**,(%esp)

strcpy(AUTH**, INPUT*** + 5)


strncmp(RESET********, INPUT***, 5)
   0x0804866b <+263>:	mov    AUTH**,%eax
   0x08048673 <+271>:	call   0x8048420 <free@plt>


   0x08048678 <+276>:	lea     INPUT*** ,%eax
   0x0804867c <+280>:	mov    %eax,INPUT***
   0x0804867e <+282>:	mov    "service" ,%eax
   0x08048683 <+287>:	mov    $0x6,%ecx
   0x08048688 <+292>:	mov    INPUT***,%esi
   0x0804868a <+294>:	mov    %eax,%edi
   0x0804868c <+296>:	repz cmpsb %es:( "service"),%ds:(INPUT***)

strncmp( "service", INPUT***)

   0x080486a1 <+317>:	lea     INPUT*** ,%eax
   0x080486a5 <+321>:	add    $0x7,%eax
   0x080486a8 <+324>:	mov    %eax,(%esp)
   0x080486ab <+327>:	call   0x8048430 <strdup@plt>

strdup(INPUT***)


   0x080486b0 <+332>:	mov    %eax,SERVICE***

SERVICE*** = strdup(INPUT***)


   0x080486b5 <+337>:	lea     INPUT*** ,%eax
   0x080486b9 <+341>:	mov    %eax,%edx
   0x080486bb <+343>:	mov    "login", %eax
   0x080486c0 <+348>:	mov    $0x5,%ecx
   0x080486c5 <+353>:	mov    %edx,%esi
   0x080486c7 <+355>:	mov    %eax,%edi
   0x080486c9 <+357>:	repz cmpsb %es:(%edi),%ds:(%esi)

  strcmp("login",  INPUT***)


   0x080486e7 <+387>:	mov    AUTH** + 0x20 ,%eax
   0x080486ea <+390>:	test   %eax,%eax
   0x080486ec <+392>:	je     0x80486ff <main+411>
   0x080486ee <+394>:	movl   "/bin/sh", (%esp)
   0x080486f5 <+401>:	call   0x8048480 <system@plt>
   0x080486fa <+406>:	jmp    0x8048574 <main+16>

if (AUTH** + 0x20)
   system("/bin/sh")


test is a non-destructive and, it doesnt return the result of the operation but it sets the flags
test eax, eax is the same as and eax, eax  except that it doesnt store the result in eax

TEST instruction performs a bitwise logical AND, 
discards the actual result and sets/unsets the zero-flag (ZF) according to the result of the logical and:
if the result is zero it sets ZF = 1, otherwise it sets ZF = 0.

Conditional jump instructions like JE are designed to look 
at the ZF for jumping/notjumping so using TEST and JE together is equivalent 
to perform a conditional jump based on the value of a specific register

```

```
while (1)
	{
		printf("%p, %p\n", auth, service);
		if (fgets(buffer, 128, stdin) == 0)
			break;
		if (strncmp(buffer, "auth ", 5) == 0)
		{
			auth = malloc(4);
			auth[0] = 0;
			if (strlen(buffer + 5) <= 30)
				strcpy(auth, buffer + 5);
		}
		if (strncmp(buffer, "reset", 5) == 0)
			free(auth);
		if (strncmp(buffer, "service", 6) == 0)
			service = strdup(buffer + 7);
		if (strncmp(buffer, "login", 5) == 0)
		{
			if (auth[32] != 0)
				system("/bin/sh");
			else
				fwrite("Password:\n", 10, 1, stdout);
		}
	}
```
