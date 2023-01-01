
```
0x08048454  n // shell
0x08048468  m // call puts
0x0804847c  main

```

main()
```
char *eax = malloc(0x40)
int32_t *eax_1 = malloc(4)
*eax_1 = 0x8048468
strcpy(eax, argv[1])
return (*eax_1)()
```

n()
```
return system("/bin/cat /home/user/level7/.pass")
```

m()
```
return puts("Nope")
```
