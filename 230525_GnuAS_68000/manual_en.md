## 8.10 M680x0 Dependent Features
## 8.10.1 M680x0 Options

The Motorola 680x0 version of as has a few machine dependent options.

You can use the `-l` option to shorten the size of references to undefined symbols. If you do not use the `-l` option, references to undefined symbols are wide enough for a full long (32 bits). (Since as cannot know where these symbols end up, as can only allocate space for the linker to fill in later. Since as does not know how far away these symbols are, it allocates as much space as it can.) If you use this option, the references are only one word wide (16 bits). This may be useful if you want the object file to be as small as possible, and you know that the relevant symbols are always less than 17 bits away.

For some configurations, especially those where the compiler normally does not prepend an underscore to the names of user variables, the assembler requires a `%` before any use of a register name. This is intended to let the assembler distinguish between C variables and functions named `a0` through `a7`, and so on. The `%` is always accepted, but is not required for certain configurations, notably `sun3`. The `--register-prefix-optional` option may be used to permit omitting the `%` even for configurations for which it is normally required.  If this is done, it will generally be impossible to refer to C variables and functions with the same names as register names.    Normally the character `|` is treated as a comment character, which means that it can not be used in expressions. The `--bitwise-or` option turns `|` into a normal character.  In this mode, you must either use C style comments, or start comments with a `#` character at the beginning of a line.

If you use an addressing mode with a base register without specifying the size, as will normally use the full 32 bit value. For example, the addressing mode `%a0@(%d0)` is equivalent to `%a0@(%d0:l)`. You may use the `--base-size-default-16` option to tell as to default to using the 16 bit value. In this case, `%a0@(%d0)` is equivalent to `%a0@(%d0:w)`. You may use the `--base-size-default-32` option to restore the default behaviour.

If you use an addressing mode with a displacement, and the value of the displacement is not known, as will normally assume that the value is 32 bits. For example, if the symbol `disp` has not been defined, as will assemble the addressing mode `%a0@(disp,%d0)` as though `disp` is a 32 bit value. You may use the `--disp-size-default-16` option to tell as to instead assume that the displacement is 16 bits. In this case, as will assemble `%a0@(disp,%d0)` as though `disp` is a 16 bit value. You may use the `--disp-size-default-32` option to restore the default behaviour.

`as` can assemble code for several different members of the Motorola 680x0 family. The default depends upon how as was configured when it was built; normally, the default is to assemble code for the 68020 microprocessor. The following options may be used to change the default. These options control which instructions and addressing modes are permitted.  The members of the 680x0 family are very similar. For detailed information about the differences, see the Motorola manuals.

### `-m68000` `-m68ec000` `-m68hc000` `-m68hc001` `-m68008` `-m68302` `-m68306` `-m68307` `-m68322` `-m68356`
Assemble for the 68000. `-m68008`, `-m68302`, and so on are synonyms for `-m68000`, since the chips are the same from the point of view of the assembler.

### `-m68010`
Assemble for the 68010.

### `-m68020` `-m68ec020`
Assemble for the 68020. This is normally the default.

### `-m68030` `-m68ec030`
Assemble for the 68030.

### `-m68040` `-m68ec040` 
Assemble for the 68040.

### `-m68060` `-m68ec060` 
Assemble for the 68060.

### `-mcpu32` `-m68330` `-m68331` `-m68332` `-m68333` `-m68334` `-m68336` `-m68340` `-m68341` `-m68349` `-m68360` 
Assemble for the CPU32 family of chips.

### `-m5200` 
Assemble for the ColdFire family of chips.

### `-m68881` `-m68882` 
Assemble 68881 floating point instructions. This is the default for the 68020, 68030, and the CPU32. The 68040 and 68060 always support floating point instructions.

###  `-mno-68881` 
Do not assemble 68881 floating point instructions. This is the default for 68000 and the 68010. The 68040 and 68060 always support floating point instructions, even if this option is used.

### `-m68851` 
Assemble 68851 MMU instructions. This is the default for the 68020, 68030, and 68060. The 68040 accepts a somewhat different set of MMU instructions; `-m68851` and `-m68040` should not be used together.

### `-mno-68851` 
Do not assemble 68851 MMU instructions. This is the default for the 68000, 68010, and the CPU32. The 68040 accepts a somewhat different set of MMU instructions.

## 8.10.2 Syntax 
This syntax for the Motorola 680x0 was developed at mit.

The 680x0 version of as uses instructions names and syntax compatible with the Sun assembler. Intervening periods are ignored; for example, `movl` is equivalent to `mov.l`.

In the following table apc stands for any of the address registers (`%a0` through `%a7`), the program counter (`%pc`), the zero-address relative to the program counter (`%zpc`), a suppressed address register (`%za0` through `%za7`), or it may be omitted entirely. The use of size means one of `w` or `l`, and it may be omitted, along with the leading colon, unless a scale is also specified. The use of scale means one of `1`, `2`, `4`, or `8`, and it may always be omitted along with the leading colon.

The following addressing modes are understood: 

#### Immediate 
`#number` 

#### Data Register 
`%d0` through `%d7` 

#### Address Register 
`%a0` through `%a7`　　
`%a7` is also known as `%sp`, i.e. the Stack Pointer. `%a6` is also known as `%fp`, the Frame Pointer.

#### Address Register Indirect 
`%a0@` through `%a7@` 

#### Address Register Postincrement 
`%a0@+` through `%a7@+` 

#### Address Register Predecrement 
`%a0@-` through `%a7@-` 

#### Indirect Plus Offset 
`apc@(number)` 

#### Index 
`apc@(number,register:size:scale)`  
The number may be omitted.

#### Postindex 
`apc@(number)@(onumber,register:size:scale)`  
The onumber or the register, but not both, may be omitted.

#### Preindex 
`apc@(number,register:size:scale)@(onumber)`  
The number may be omitted. Omitting the register produces the Postindex addressing mode.

#### Absolute 
`symbol`, or `digits`, optionally followed by `:b`, `:w`, or `:l`.

## 8.10.3 Motorola Syntax 
The standard Motorola syntax for this chip differs from the syntax already discussed (see Section 8.10.2 [Syntax], page 81). as can accept Motorola syntax for operands, even if mit syntax is used for other operands in the same instruction. The two kinds of syntax are fully compatible.

In the following table apc stands for any of the address registers (`%a0` through `%a7`), the program counter (`%pc`), the zero-address relative to the program counter (`%zpc`), or a suppressed address register (`%za0` through `%za7`). The use of size means one of `w` or `l`, and it may always be omitted along with the leading dot. The use of scale means one of `1`, `2`, `4`, or `8`, and it may always be omitted along with the leading asterisk.

The following additional addressing modes are understood: 

#### Address Register Indirect 
`(%a0)` through `(%a7)`  

`%a7` is also known as `%sp`, i.e. the Stack Pointer. %a6 is also known as `%fp`, the Frame Pointer.

#### Address Register Postincrement 
`(%a0)+` through `(%a7)+` 

#### Address Register Predecrement 
`-(%a0)` through `-(%a7)` 

#### Indirect Plus Offset 
`number(%a0)` through `number(%a7)`, or `number(%pc)`.

The number may also appear within the parentheses, as in `(number,%a0)`.  When used with the pc, the number may be omitted (with an address register, omitting the number produces Address Register Indirect mode).

#### Index 
`number(apc,register.size*scale)`  

The number may be omitted, or it may appear within the parentheses. The apc may be omitted. The register and the apc may appear in either order. If both apc and register are address registers, and the size and scale are omitted, then the first register is taken as the base register, and the second as the index register.

#### Postindex 
`([number,apc],register.size*scale,onumber)` 

The onumber, or the register, or both, may be omitted. Either the number or the apc may be omitted, but not both.

#### Preindex 
`([number,apc,register.size*scale],onumber)` 

The number, or the apc, or the register, or any two of them, may be omitted.

The onumber may be omitted. The register and the apc may appear in either order. If both apc and register are address registers, and the size and scale are omitted, then the first register is taken as the base register, and the second as the index register.

## 8.10.4 Floating Point
Packed decimal (P) format floating literals are not supported. Feel free to add the code! The floating point formats generated by directives are these.

`.float` Single precision floating point constants.

`.double` Double precision floating point constants.

`.extend` `.ldouble` Extended precision (long double) floating point constants.

## 8.10.5 680x0 Machine Directives
In order to be compatible with the Sun assembler the 680x0 assembler understands the following directives.

`.data1` This directive is identical to a .data 1 directive.

`.data2` This directive is identical to a .data 2 directive.

`.even` This directive is a special case of the .align directive; it aligns the output to an even byte boundary.

`.skip` This directive is identical to a .space directive.

## 8.10.6 Opcodes 
## 8.10.6.1 Branch Improvement 
Certain pseudo opcodes are permitted for branch instructions. They expand to the shortest branch instruction that reach the target. 
 Generally these mnemonics are made by substituting `j` for `b` at the start of a Motorola mnemonic.

The following table summarizes the pseudo-operations. A * flags cases that are more fully described after the table:

```
          Displacement 
          +------------------------------------------------- 
          |          68020 68000/10
Pseudo-Op |BYTE WORD LONG  LONG      non-PC relative 
          +------------------------------------------------- 
jbsr      |bsrs bsr  bsrl  jsr       jsr
jra       |bras bra  bral  jmp       jmp 
*     jXX |bXXs bXX  bXXl  bNXs;jmpl bNXs;jmp
*    dbXX |dbXX dbXX dbXX; bra; jmpl 
*    fjXX |fbXXw fbXXw fbXXl         fbNXw;jmp

XX: condition 
NX: negative of condition XX 
*—see full description below
```
`jbsr` `jra` These are the simplest jump pseudo-operations; they always map to one particular machine instruction, depending on the displacement to the branch target.

`jXX` Here, `jXX` stands for an entire family of pseudo-operations, where XX is a conditional branch or condition-code test. The full list of pseudo-ops in this family is: 
```
jhi jls jcc jcs jne jeq jvc
jvs jpl jmi jge jlt jgt jle
```
For the cases of non-PC relative displacements and long displacements on the 68000 or 68010, as issues a longer code fragment in terms of NX, the opposite condition to XX. For example, for the non-PC relative case:
```
    jXX foo
```
gives
```
    bNXs oof
    jmp foo
  oof:
```  
`dbXX` The full family of pseudo-operations covered here is
```
dbhi dbls dbcc dbcs dbne dbeq dbvc 
dbvs dbpl dbmi dbge dblt dbgt dble 
dbf dbra dbt 
```
Other than for word and byte displacements, when the source reads `dbXX foo`, as emits 
```
    dbXX oo1 
    bra oo2 
  oo1:jmpl foo
  oo2:
```
`fjXX` This family includes 
```
fjne fjeq fjge fjlt fjgt fjle fjf
fjt fjgl fjgle fjnge fjngl fjngle fjngt
fjnle fjnlt fjoge fjogl fjogt fjole fjolt
fjor fjseq fjsf fjsne fjst fjueq fjuge
fjugt fjule fjult fjun 
```
For branch targets that are not PC relative, as emits 
```
    fbNX oof 
    jmp foo 
  oof: 
```
when it encounters `fjXX foo`.

## 8.10.6.2 Special Characters 
The immediate character is `#` for Sun compatibility. The line-comment character is `|` (unless the `--bitwise-or` option is used). If a `#` appears at the beginning of a line, it is treated as a comment unless it looks like `# line file`, in which case it is treated normally.
