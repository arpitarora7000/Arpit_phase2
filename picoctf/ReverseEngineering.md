# GDB baby step 1
## Description
Can you figure out what is in the `eax` register at the end of the `main` function? Put your answer in the picoCTF flag format: `picoCTF{n}` where n is the contents of the eax register in the decimal number base. If the answer was `0x11` your flag would be `picoCTF{17}`.
Disassemble _this_.

HINTS:   

1. gdb is a very good debugger to use for this problem and many others!
2. `main` is actually a recognized symbol that can be used with gdb commands.

## Solution
- I started by learning that `eax`, is a `CPU register`, which usually stores the `return value`, for a code in say `C`.
- Then I had to look at the `assembly instructions`, which is another new term that I learned about, then I had look for the instruction that writes into `eax`, then convert that value to decimal.
- Then that decimal was supposed to be put in the flag has was mentioned.
- So, first I made the file that was given `debugger0_a`, executable by `chmod +x debugger0_a`, because I had to execute it using `gdb`.
- Then I used gdb to `disassemble main`.
- This was what I found for eax, `$0x86342,%eax`, then which was `549698`, in decimal.

```
arpitarora@Arpits-MacBook-Air ~ % cd downloads
arpitarora@Arpits-MacBook-Air downloads % chmod +x debugger0_a
arpitarora@Arpits-MacBook-Air downloads % gdb debugger0_a     
GNU gdb (GDB) 16.3
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "--host=aarch64-apple-darwin24.4.0 --target=x86_64-apple-darwin20".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from debugger0_a...
(No debugging symbols found in debugger0_a)
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:	endbr64
   0x000000000000112d <+4>:	push   %rbp
   0x000000000000112e <+5>:	mov    %rsp,%rbp
   0x0000000000001131 <+8>:	mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:	mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:	mov    $0x86342,%eax
   0x000000000000113d <+20>:	pop    %rbp
   0x000000000000113e <+21>:	ret
End of assembler dump.
(gdb) 
```
## Flag 
```picoCTF{549698}```

INCOMPLETE RN
