cloud.binary.ninja
```
if (atoi(*(arg1 + 4)) != 0x1a7) // 423
	fwrite(0x80c5350, 1, 5, _IO_stderr)  {"No !\n"}
else
    char* var_20 = strdup("/bin/sh")
    execv(0x80c5348, &var_20)  {"/bin/sh"}
```
```
level0@RainFall:~$ ./level0 423

	cat /home/user/level1/.pass
	1fe8a524fa4bec01ca4ea2a869af2a02260d4a7d5fe7e7c24d8617e6dca12d3a
```
