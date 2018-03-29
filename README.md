# Patched-Minesweeper
I hacked Minesweeper into putting flags on all the mined squares when starting. Yoohoo.

### The tools I used:
* [IDA Pro Free](https://www.hex-rays.com/products/ida/support/download.shtml)
* [OllyDbg](http://www.ollydbg.de/)
* [HxD](https://mh-nexus.de/en/hxd/)
* [Online x86 Assembler](https://defuse.ca/online-x86-assembler.htm)
* Windows *calc.exe* Developer mode :muscle:

### The steps I took (very rough outline): 
* found the mine-field in process's memory [0x1005400]
* found the function which draws the initial board [0x10026A7]
* patched this function such that for each mined-square, a flag image will be drawn (an argument different from that of a "clean" square will be passed to Windows's *BitBlt* function).

### The code I overrode:
```
010026E9    AND EAX, 1F
010026EE    PUSH DWORD PTR DS:[EAX*4+1005A20]
```

### The code I added:
```
010026E9    JMP patched_minesweeper.01004A60
010026EE    NOP
010026EF    NOP
010026F0    NOP
010026F1    NOP
010026F2    NOP
```
...
```
1004A60   CMP AL, 8F
1004A62   JNZ SHORT patched_minesweeper.01004A66
1004A64   MOV AL, 0E
1004A66   PUSH DWORD PTR DS:[EAX*4+1005A20]
1004A6D   JMP patched_minesweeper.010026F3
```
