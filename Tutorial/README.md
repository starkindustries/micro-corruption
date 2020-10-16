# Tutorial

## Links

* [Lockitall LockIT Pro User Guide][1]
* [MSP430 Family Instruction Set Summary][2]

## Commands

| Command     | Description  |
|:------------|:-------------|
| `help` | the most important command |
| `c` | continue until next breakpoint |
| `s` | step instruction |
| `out` | step out of current function |
| `f` | finish; same as `out` |
| `break [expr]` | set breakpoint |
| `unbreak [expr]` | unset breakpoint |
| `read [expr] [c]` | read `[c]` bytes at address `[expr]` |
| `track [reg]` | track a register in Live Memory Dump view |
| `untrack [reg]` | un-track a register |
| `let [reg]/[addr]=[expr]` | write a value to register or memory |
| `manual` | show the manual for this challenge | 
| `solve` | exit debugger; solve challenge on real lock |
| `reset` | reset debugger |

## Scripting Commands

* `#define name [commands]` - alias "name" to run [commands].
* `command;command` - run first command, then second comamnd.

Example: 
```
#define dump read sp 20; read r04; read r05; read r06
```

## Registers

| Register    | Description  |
|:------------|:-------------|
| pc | program counter |
| sp | stack pointer |
| sr | status register |
| cg | constant generator |
| r04-r15 | temporary storage registers |


[1]:https://microcorruption.com/manual.pdf
[2]:https://www.ti.com/sc/docs/products/micro/msp430/userguid/as_5.pdf