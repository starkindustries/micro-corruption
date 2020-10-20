# Hanoi

Inspect the login function:
```
4520 <login>
4520:  c243 1024      mov.b	#0x0, &0x2410
4524:  3f40 7e44      mov	#0x447e "Enter the password to continue.", r15
4528:  b012 de45      call	#0x45de <puts>
452c:  3f40 9e44      mov	#0x449e "Remember: passwords are between 8 and 16 characters.", r15
4530:  b012 de45      call	#0x45de <puts>
4534:  3e40 1c00      mov	#0x1c, r14
4538:  3f40 0024      mov	#0x2400, r15
453c:  b012 ce45      call	#0x45ce <getsn>
4540:  3f40 0024      mov	#0x2400, r15
4544:  b012 5444      call	#0x4454 <test_password_valid>
4548:  0f93           tst	r15
454a:  0324           jz	$+0x8
454c:  f240 0100 1024 mov.b	#0x1, &0x2410
4552:  3f40 d344      mov	#0x44d3 "Testing if password is valid.", r15
4556:  b012 de45      call	#0x45de <puts>
455a:  f290 ab00 1024 cmp.b	#0xab, &0x2410
4560:  0720           jne	#0x4570 <login+0x50>
4562:  3f40 f144      mov	#0x44f1 "Access granted.", r15
4566:  b012 de45      call	#0x45de <puts>
456a:  b012 4844      call	#0x4448 <unlock_door>
456e:  3041           ret
4570:  3f40 0145      mov	#0x4501 "That password is not correct.", r15
4574:  b012 de45      call	#0x45de <puts>
4578:  3041           ret
```

This section is moving 0x1c (28 decimal) into r14, 0x2400 (storage address) into r15, and calling the `getsn` function:
```
4534:  3e40 1c00      mov	#0x1c, r14
4538:  3f40 0024      mov	#0x2400, r15
453c:  b012 ce45      call	#0x45ce <getsn>
```

The `getsn` function pushes `r14` and `r15` onto the stack and calls the `INT` function, which will trigger the IO interrupt and get the user's input. 

After the user input screen, the entered password is now stored on the stack at address 0x2400:
```
         0 1  2 3  4 5  6 7  8 9  A B  C D  E F
2400:   4141 4141 4141 4141 4141 4141 4141 4141   AAAAAAAAAAAAAAAA
2410:   4141 4141 4141 4141 4141 4141 0000 0000   AAAAAAAAAAAA....
```

Each line of the memory dump screen contains 16 bytes. A total of 28 bytes were read in from the IO screen, the same number from instruction 4534: `mov #0x1c, r14`.

Inspect test_password_valid function:
```
4454 <test_password_valid>
4454:  0412           push	r4
4456:  0441           mov	sp, r4
4458:  2453           incd	r4
445a:  2183           decd	sp
445c:  c443 fcff      mov.b	#0x0, -0x4(r4)
4460:  3e40 fcff      mov	#0xfffc, r14
4464:  0e54           add	r4, r14
4466:  0e12           push	r14
4468:  0f12           push	r15
446a:  3012 7d00      push	#0x7d
446e:  b012 7a45      call	#0x457a <INT>
4472:  5f44 fcff      mov.b	-0x4(r4), r15
4476:  8f11           sxt	r15
4478:  3152           add	#0x8, sp
447a:  3441           pop	r4
447c:  3041           ret
```

This section is pushing `r14` and `r15` onto the stack and then calling the 0x7d interrupt.
```
4466:  0e12           push	r14
4468:  0f12           push	r15
446a:  3012 7d00      push	#0x7d
446e:  b012 7a45      call	#0x457a <INT>
```

According to the [manual](https://microcorruption.com/manual.pdf), the 0x7d interrupt is:
```
INT 0x7D.

Interface with the HSM-1 (hardware security module). Set a flag in memory if the password passed in is correct. Takes two arguments. The first argument is the password to test, the second is the location of a flag to overwrite if the password is correct.
```

In the `login` function, the instruction at 455a compares byte 0xAB to the value at address 0x2410:
```
455a:  f290 ab00 1024 cmp.b	#0xab, &0x2410
```

The password needs to be modified so that 0xAB is saved at address 0x2410:
```
4141 4141 4141 4141 4141 4141 4141 4141 ab41 4141 4141 4141 4141
```

Solved.