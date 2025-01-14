# DASM80

**Z80 Disassembler Documentation**
**Z-80(tm) DISASSEMBLER V1.02beta9 - (c) 2015-16 GmEsoft**
**Linux Port V0.5 (c) 2022 ewetoo@gmail.com**

DASM80 is a freeware command-line Z80 disassembler for Windows. It is a symbolic
labeling disassembler that supports all Z80 instructions and can read either TRS-80
CMD files or Intel HEX files.

   The DASM80 command line looks like this:

     DASM80 [options] [file]

   The following options are supported:

     -c:<file> Input file <file> in TRS-80 /CMD format ("-c:" is optional)
     -h:<file> Input file <file> in Intel HEX format
     -s:<file> Use <file>.SCR as a screening data file.
     -e:<file> Use <file>.EQU as an EQUate label definition file
     -m:<file> Use <file>.MAP as a MAP file
     -o:<file> Write disassembled source to <file>.ASM instead of screen.
     -p:<file> Write disassembly listing  to <file>.PRN instead of screen.
     -w -ww    Use wide (63)/superwide(79) format
     -ne       Don't generate new EQUates
     -nh       Don't generate header lines
     -nq       Don't generate literal single quotes ('aaaaa''aa')

For example, to disassemble the program SCRIPSIT.CMD to the screen, the command line
would look like this:

DASM80 SCRIPSIT.CMD

To disassemble the program ROBOT.CMD to the file ROBOT.ASM, using the screening file ROBOT.SCR, the command line would look like this:

     DASM80 -o:ROBOT.ASM -s:ROBOT.SCR ROBOT.CMD

## Screening files

DASM80 supports the use of screening files to separate data from code. By using a
well designed screening file, data areas can be properly marked using DB and DW
pseudo-ops instead of meaningless instructions.

A screening file follows a well-defined format, consisting of address ranges
separated by commas. (Long time TRS-80 users may remember this format as similar to
that used by DSMBLR, the MISOSYS disassembler for the TRS-80.)

Four different types of address ranges are supported:

     aaaa      one or two bytes at aaaa (two bytes for word data #aaaa)
     bbbb-cccc a range from bbbb to cccc
     -dddd     a range from 0x0000 to dddd
     eeee-     a range from eeee to 0xFFFE or to start of next range - 1

Two types of prefixes are supported for ranges:

     ! identify as code
     % identify as byte data (the default)
     $ identify as char data
     # identify as word data

Additionnally, relocatable ranges can be defined anywhere in the screening file.
Relocatable ranges generate MRAS ORG/LORG sequences of pseudo-instructions.
The relocatable range definitions apply for all subsequent regular ranges defined
in the screening file, until a new relocatable range is defined.
Relocatable ranges are defined as:
     @aaaa-bbbb:dddd  defines the range from aaaa to bbbb as relocatable to dddd
     @0000            cancels the relocatable range definition

Relocatable ranges are useful do successfully disassemble files like LS-DOS's
BACKUP/CMD where 2 relocatable blocks exist:

     ; non-relocatable common block
     !2685-
     $28F5-
     ; next area can be overwritten by following block 1 or block 2
     ; this area contains "single drive backup" code
     !2E00-
     ; relocatable block 1
     ; this area contains "mirror image backup" code
     @3200-3C00:2E00
     $35B1-
     ; relocatable block 2
     ; this area contains "backup by class" code
     @3800-4200:2E00
     !3800-
     $3EB7-
     !3FF9-
     $401F-
     ; non-relocatable block with initialization code
     @0000-0000:0000
     !4200-
     $4370-

For example, here is a valid (albeit meaningless) sample screening file:

     $3000,$3A1B-3A1D
     #3010-3014,#E000-
     $-1000

That screening file indicates the following areas of data:

     * One byte of data at 0x3000
     * Byte data between 0x3A1B and 0x3A1D
     * Word data between 0x3010 and 0x3014
     * Word data between 0xE000 and 0xFFFE
     * Byte data between 0x0000 and 0x1000

## NOTE

DASM80 is freeware, but remember that it is not public domain software. All
copyrights connected with the program and its accompanying document files remain
with me (Michel Bernard @ GmEsoft).

## Port Notes

This port has not been significantly altered in anyway other than to be made operational on unix platforms and hopefully can be the basis of more ports (Sean Dwyer ewetoo @ gmail)
