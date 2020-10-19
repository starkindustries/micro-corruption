# Sydney

Before calling the `check_password` function, the program stores the password's address into `r15`. Right after this function, r15 is tested and 
```
4438 <main>
...
444a:  0f41           mov	sp, r15        r15=password
444c:  b012 8a44      call	#0x448a <check_password>
4450:  0f93           tst	r15
4452:  0520           jnz	#0x445e <main+0x26>
...
445e:  3f40 f144      mov	#0x44f1 "Access Granted!", r15 << jump to 4453 unlocks the lock
```

Verify that the password is at `r15`:
```
> read r15
   439c:   7465 7374 0000 0000  test....
```

Examine the `check_password` function:
```
448a <check_password>
448a:  bf90 515d 0000 cmp	#0x5d51, 0x0(r15) << this is comparing the first two characters of the password to 0x5d51
4490:  0d20           jnz	$+0x1c
4492:  bf90 6056 0200 cmp	#0x5660, 0x2(r15) << this is comparing the next two characters to 0x5660
4498:  0920           jnz	$+0x14
449a:  bf90 4a7c 0400 cmp	#0x7c4a, 0x4(r15)
44a0:  0520           jne	#0x44ac <check_password+0x22>
44a2:  1e43           mov	#0x1, r14
44a4:  bf90 6865 0600 cmp	#0x6568, 0x6(r15)
44aa:  0124           jeq	#0x44ae <check_password+0x24>
44ac:  0e43           clr	r14
44ae:  0f4e           mov	r14, r15
44b0:  3041           ret
```