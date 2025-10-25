# GDB baby step 1
## Description
> Can you figure out what is in the `eax` register at the end of the `main` function? Put your answer in the picoCTF flag format: `picoCTF{n}` where n is the contents of the eax register in the decimal number base. If the answer was `0x11` your flag would be `picoCTF{17}`.
Disassemble _this_.
> 
> HINTS:   
> 
> 1. gdb is a very good debugger to use for this problem and many others!
> 2. `main` is actually a recognized symbol that can be used with gdb commands.

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

## Concepts Learnt:
- Learned about the `gdb`, which stands for `GNU debugger`, which allows examining what happens inside a given program while running.
- Learned about cpu registers, and in particular `eax`.
- Learned how one can disassemble `main function`, using `gdb`.

## Incorrect tangents that I was stuck on
- None, but took some time to figure out things about the new terminologies, and when I wanted to make the file execuateble, It wasn't initially working, then I found out by searching a little, that the terminal needs `full disk access`, to be able to use `chmod`.

## Resources
- Learned about CPU registers, through the youtube video given in resources, https://youtu.be/1d-6Hv1c39c?si=2VYRB51nZQt3UI0b.
- Then about `gdb`, on wikipedia https://en.wikipedia.org/wiki/GNU_Debugger

# ARMssembly 1
## Description
> For what argument does this program print `win` with variables `58, 2 and 3`? File: `chall_1.S` Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})
> 
> HINT: Shifts

## Solution
- So in the function given, the assembly instruction `sub	sp, sp, #32`, is reserving 32 bytes of space on the stack, further `w0`, is stored at the 12th place in the stackpointer, then `58`, is moved into `w0`, as it's value, by the instructions: `str w0, [sp, 12]`, and `mov w0, 58`
- Then similarly, `w0`, is further stored in `sp+16`, then 2 is moved as the value of w0, then stored at `20`
, then 3 is moved as the value of w0, and stored at 24.
- Then instruction `ldr	w0, [sp, 20]`, is used to load w0 from the address `sp+20`, where `2` was stored, and `ldr w1, [sp, 16]`, to load w1 from 16, where `58` was stored.
- Then the next instruction `lsl w0, w1, w0` performs a logical shift left on the value in register w1 by the number of positions specified by the value in register w0, storing the result in register w0, which is equivalent of multiplying w1 by 2^(vlue of w0), and storing the answer in w0.
- So by the latest instruction, we get 58*4=232, which now stored in the register w0 at sp+28, which temporarily stores the value.
- Then w1 is loaded from address sp+28, which is 232, and w0 from 24 which is 3, then the instruction `sdiv	w0, w1, w0`, is used to integer divide the value in w1 by the value in w0 which gives 232/3=77, and store in w0, and then temporarily store w0 in sp+28.
- Then w1 is loaded from the sp+28, where 77 was stored temporarily, and w0 is loaded from sp+12, which'll be the user input from the main function, when func is called.
- Thenn by `sub	w0, w1, w0`, where w0 is subtracted from w1 and stored in w0, now as was mentioned in the main function that if w0 and 0 are equal it'll print "you win", so for finally w0 to be 0 input value should be 77.
- Then by converting decimal value 77 to hexadecimal, i got `4d`, and then put it into the flag format specified.

## Flag  
`picoCTF{0000004d}`

INCOMPLETE YET

