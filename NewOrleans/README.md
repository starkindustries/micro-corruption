# New Orleans

Investigate the `create_password` function. 

```
447e <create_password>
447e:  3f40 0024      mov      #0x2400, r15
^ move value 2400 into r15 register
4482:  ff40 7700 0000 mov.b    #0x77, 0x0(r15)    w
^ move value 0x77 into address at r15+0x0
4488:  ff40 7b00 0100 mov.b    #0x7b, 0x1(r15)    {
448e:  ff40 6400 0200 mov.b    #0x64, 0x2(r15)    d
4494:  ff40 2400 0300 mov.b    #0x24, 0x3(r15)    $
449a:  ff40 5700 0400 mov.b    #0x57, 0x4(r15)    W
44a0:  ff40 5100 0500 mov.b    #0x51, 0x5(r15)    Q
44a6:  ff40 3a00 0600 mov.b    #0x3a, 0x6(r15)    :
44ac:  cf43 0700      mov.b    #0x0, 0x7(r15)
44b0:  3041           ret
```

Password:
```
w{d$WQ:
```