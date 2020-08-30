.. _custom-isa-extensions:

PULP Instruction Set Extensions
===============================

CV32E40P supports the following PULP ISA Extensions, which are part of **Xpulp** and can be enabled by setting ``PULP_XPULP`` == 1.

 * Post-Incrementing load and stores, see :ref:`pulp_load_store`.
 * Hardware Loop extension, see :ref:`pulp_hardware_loop`.
 * ALU extensions, see :ref:`pulp_alu`.
 * Multiply-Accumulate extensions, see :ref:`pulp_multiply_accumulate`.
 * Optional support for Hardware Loops, see :ref:`pulp_simd`.

Additionally the event load instruction (**p.elw**) is supported by setting ``PULP_CLUSTER`` == 1.

To use such instructions, you need to compile your SW with the PULP GCC compiler.

If not specified, all the operands are signed and immediate values are sign-extended.

.. _pulp_load_store:

Post-Incrementing Load & Store Instructions and Register-Register Load & Store Instructions
-------------------------------------------------------------------------------------------

Post-Incrementing load and store instructions perform a load, or a
store, respectively, while at the same time incrementing the address
that was used for the memory access. Since it is a post-incrementing
scheme, the base address is used for the access and the modified address
is written back to the register-file. There are versions of those
instructions that use immediates and those that use registers as
offsets. The base address always comes from a register.

The custom post-incrementing load & store instructions and register-register
load & store instructions are only supported if ``PULP_XPULP`` == 1.

Load Operations
^^^^^^^^^^^^^^^

+----------------------------------------------------+-------------------------------+
| **Mnemonic**                                       | **Description**               |
+====================================================+===============================+
| **Register-Immediate Loads with Post-Increment**   |                               |
+----------------------------------------------------+-------------------------------+
| **p.lb rD, Imm(rs1!)**                             | rD = Sext(Mem8(rs1))          |
|                                                    |                               |
|                                                    | rs1 += Imm[11:0]              |
+----------------------------------------------------+-------------------------------+
| **p.lbu rD, Imm(rs1!)**                            | rD = Zext(Mem8(rs1))          |
|                                                    |                               |
|                                                    | rs1 += Imm[11:0]              |
+----------------------------------------------------+-------------------------------+
| **p.lh rD, Imm(rs1!)**                             | rD = Sext(Mem16(rs1))         |
|                                                    |                               |
|                                                    | rs1 += Imm[11:0]              |
+----------------------------------------------------+-------------------------------+
| **p.lhu rD, Imm(rs1!)**                            | rD = Zext(Mem16(rs1))         |
|                                                    |                               |
|                                                    | rs1 += Imm[11:0]              |
+----------------------------------------------------+-------------------------------+
| **p.lw rD, Imm(rs1!)**                             | rD = Mem32(rs1)               |
|                                                    |                               |
|                                                    | rs1 += Imm[11:0]              |
+----------------------------------------------------+-------------------------------+
| **Register-Register Loads with Post-Increment**    |                               |
+----------------------------------------------------+-------------------------------+
| **p.lb rD, rs2(rs1!)**                             | rD = Sext(Mem8(rs1))          |
|                                                    |                               |
|                                                    | rs1 += rs2                    |
+----------------------------------------------------+-------------------------------+
| **p.lbu rD, rs2(rs1!)**                            | rD = Zext(Mem8(rs1))          |
|                                                    |                               |
|                                                    | rs1 += rs2                    |
+----------------------------------------------------+-------------------------------+
| **p.lh rD, rs2(rs1!)**                             | rD = Sext(Mem16(rs1))         |
|                                                    |                               |
|                                                    | rs1 += rs2                    |
+----------------------------------------------------+-------------------------------+
| **p.lhu rD, rs2(rs1!)**                            | rD = Zext(Mem16(rs1))         |
|                                                    |                               |
|                                                    | rs1 += rs2                    |
+----------------------------------------------------+-------------------------------+
| **p.lw rD, rs2(rs1!)**                             | rD = Mem32(rs1)               |
|                                                    |                               |
|                                                    | rs1 += rs2                    |
+----------------------------------------------------+-------------------------------+
| **Register-Register Loads**                        |                               |
+----------------------------------------------------+-------------------------------+
| **p.lb rD, rs2(rs1)**                              | rD = Sext(Mem8(rs1 + rs2))    |
+----------------------------------------------------+-------------------------------+
| **p.lbu rD, rs2(rs1)**                             | rD = Zext(Mem8(rs1 + rs2))    |
+----------------------------------------------------+-------------------------------+
| **p.lh rD, rs2(rs1)**                              | rD = Sext(Mem16(rs1 + rs2))   |
+----------------------------------------------------+-------------------------------+
| **p.lhu rD, rs2(rs1)**                             | rD = Zext(Mem16(rs1 + rs2))   |
+----------------------------------------------------+-------------------------------+
| **p.lw rD, rs2(rs1)**                              | rD = Mem32(rs1 + rs2)         |
+----------------------------------------------------+-------------------------------+

Store Operations
^^^^^^^^^^^^^^^^

+-----------------------------------------------------+--------------------------+
| **Mnemonic**                                        | **Description**          |
+=====================================================+==========================+
| **Register-Immediate Stores with Post-Increment**   |                          |
+-----------------------------------------------------+--------------------------+
| **p.sb rs2, Imm(rs1!)**                             | Mem8(rs1) = rs2          |
|                                                     |                          |
|                                                     | rs1 += Imm[11:0]         |
+-----------------------------------------------------+--------------------------+
| **p.sh rs2, Imm(rs1!)**                             | Mem16(rs1) = rs2         |
|                                                     |                          |
|                                                     | rs1 += Imm[11:0]         |
+-----------------------------------------------------+--------------------------+
| **p.sw rs2, Imm(rs1!)**                             | Mem32(rs1) = rs2         |
|                                                     |                          |
|                                                     | rs1 += Imm[11:0]         |
+-----------------------------------------------------+--------------------------+
| **Register-Register Stores with Post-Increment**    |                          |
+-----------------------------------------------------+--------------------------+
| **p.sb rs2, rs3(rs1!)**                             | Mem8(rs1) = rs2          |
|                                                     |                          |
|                                                     | rs1 += rs3               |
+-----------------------------------------------------+--------------------------+
| **p.sh rs2, rs3(rs1!)**                             | Mem16(rs1) = rs2         |
|                                                     |                          |
|                                                     | rs1 += rs3               |
+-----------------------------------------------------+--------------------------+
| **p.sw rs2, rs3(rs1!)**                             | Mem32(rs1) = rs2         |
|                                                     |                          |
|                                                     | rs1 += rs3               |
+-----------------------------------------------------+--------------------------+
| **Register-Register Stores**                        |                          |
+-----------------------------------------------------+--------------------------+
| **p.sb rs2, rs3(rs1)**                              | Mem8(rs1 + rs3) = rs2    |
+-----------------------------------------------------+--------------------------+
| **p.sh rs2 rs3(rs1)**                               | Mem16(rs1 + rs3) = rs2   |
+-----------------------------------------------------+--------------------------+
| **p.sw rs2, rs3(rs1)**                              | Mem32(rs1 + rs3) = rs2   |
+-----------------------------------------------------+--------------------------+

Encoding
~~~~~~~~

+-------------+--------+----------+--------+------------+---------------------------+
| 31   :   20 | 19 :15 | 14  : 12 | 11 :07 | 06  :   00 |                           |
+-------------+--------+----------+--------+------------+---------------------------+
| imm[11:0]   | rs1    | funct3   | rd     | opcode     | Mnemonic                  |
+=============+========+==========+========+============+===========================+
| offset      | base   | 000      | dest   | 000 1011   | **p.lb rD, Imm(rs1!)**    |
+-------------+--------+----------+--------+------------+---------------------------+
| offset      | base   | 100      | dest   | 000 1011   | **p.lbu rD, Imm(rs1!)**   |
+-------------+--------+----------+--------+------------+---------------------------+
| offset      | base   | 001      | dest   | 000 1011   | **p.lh rD, Imm(rs1!)**    |
+-------------+--------+----------+--------+------------+---------------------------+
| offset      | base   | 101      | dest   | 000 1011   | **p.lhu rD, Imm(rs1!)**   |
+-------------+--------+----------+--------+------------+---------------------------+
| offset      | base   | 010      | dest   | 000 1011   | **p.lw rD, Imm(rs1!)**    |
+-------------+--------+----------+--------+------------+---------------------------+

+------------+----------+--------+----------+--------+------------+---------------------------+
| 31  :   25 | 24  : 20 | 19 :15 | 14  : 12 | 11 :07 | 06  :   00 |                           |
+------------+----------+--------+----------+--------+------------+---------------------------+
| funct7     | rs2      | rs1    | funct3   | rd     | opcode     | Mnemonic                  |
+============+==========+========+==========+========+============+===========================+
| 000 0000   | offset   | base   | 111      | dest   | 000 1011   | **p.lb rD, rs2(rs1!)**    |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 010 0000   | offset   | base   | 111      | dest   | 000 1011   | **p.lbu rD, rs2(rs1!)**   |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 000 1000   | offset   | base   | 111      | dest   | 000 1011   | **p.lh rD, rs2(rs1!)**    |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 010 1000   | offset   | base   | 111      | dest   | 000 1011   | **p.lhu rD, rs2(rs1!)**   |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 001 0000   | offset   | base   | 111      | dest   | 000 1011   | **p.lw rD, rs2(rs1!)**    |
+------------+----------+--------+----------+--------+------------+---------------------------+

+------------+----------+--------+----------+--------+------------+---------------------------+
| 31  :   25 | 24  : 20 | 19 :15 | 14  : 12 | 11 :07 | 06  :   00 |                           |
+------------+----------+--------+----------+--------+------------+---------------------------+
| funct7     | rs2      | rs1    | funct3   | rd     | opcode     | Mnemonic                  |
+============+==========+========+==========+========+============+===========================+
| 000 0000   | offset   | base   | 111      | dest   | 000 0011   | **p.lb rD, rs2(rs1)**     |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 010 0000   | offset   | base   | 111      | dest   | 000 0011   | **p.lbu rD, rs2(rs1)**    |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 000 1000   | offset   | base   | 111      | dest   | 000 0011   | **p.lh rD, rs2(rs1)**     |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 010 1000   | offset   | base   | 111      | dest   | 000 0011   | **p.lhu rD, rs2(rs1)**    |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 001 0000   | offset   | base   | 111      | dest   | 000 0011   | **p.lw rD, rs2(rs1)**     |
+------------+----------+--------+----------+--------+------------+---------------------------+

+----------------+-------+--------+----------+---------------+------------+---------------------------+
| 31    :     25 | 24:20 | 19 :15 | 14  : 12 | 11   :     07 | 06  :   00 |                           |
+----------------+-------+--------+----------+---------------+------------+---------------------------+
| imm[11:5]      | rs2   | rs1    | funct3   | rd            | opcode     | Mnemonic                  |
+================+=======+========+==========+===============+============+===========================+
| offset[11:5]   | src   | base   | 000      | offset[4:0]   | 010 1011   | **p.sb rs2, Imm(rs1!)**   |
+----------------+-------+--------+----------+---------------+------------+---------------------------+
| offset[11:5]   | src   | base   | 001      | offset[4:0]   | 010 1011   | **p.sh rs2, Imm(rs1!)**   |
+----------------+-------+--------+----------+---------------+------------+---------------------------+
| offset[11:5]   | src   | base   | 010      | offset[4:0]   | 010 1011   | **p.sw rs2, Imm(rs1!)**   |
+----------------+-------+--------+----------+---------------+------------+---------------------------+

+------------+----------+--------+----------+--------+------------+---------------------------+
| 31  :   25 | 24  : 20 | 19 :15 | 14  : 12 | 11 :07 | 06   :  00 |                           |
+------------+----------+--------+----------+--------+------------+---------------------------+
| funct7     | rs2      | rs1    | funct3   | rd     | opcode     | Mnemonic                  |
+============+==========+========+==========+========+============+===========================+
| 000 0000   | src      | base   | 100      | offset | 010 1011   | **p.sb rs2, rs3(rs1!)**   |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 000 0000   | src      | base   | 101      | offset | 010 1011   | **p.sh rs2, rs3(rs1!)**   |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 000 0000   | src      | base   | 110      | offset | 010 1011   | **p.sw rs2, rs3(rs1!)**   |
+------------+----------+--------+----------+--------+------------+---------------------------+

+------------+----------+--------+----------+--------+------------+---------------------------+
| 31  :   25 | 24 :  20 | 19 :15 | 14  : 12 | 11 :07 | 06   :  00 |                           |
+------------+----------+--------+----------+--------+------------+---------------------------+
| funct7     | rs2      | rs1    | funct3   | rs3    | opcode     | Mnemonic                  |
+============+==========+========+==========+========+============+===========================+
| 000 0000   | src      | base   | 100      | offset | 010 0011   | **p.sb rs2, rs3(rs1)**    |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 000 0000   | src      | base   | 101      | offset | 010 0011   | **p.sh rs2, rs3(rs1)**    |
+------------+----------+--------+----------+--------+------------+---------------------------+
| 000 0000   | src      | base   | 110      | offset | 010 0011   | **p.sw rs2, rs3(rs1)**    |
+------------+----------+--------+----------+--------+------------+---------------------------+

Event Load Instructions
-----------------------

The event load instruction **p.elw** is only supported if the ``PULP_CLUSTER`` parameter is set to 1.
The event load performs a load word and can cause the CV32E40P to enter a sleep state as explained
in :ref:`pulp_cluster`.

Load Operations
^^^^^^^^^^^^^^^

+----------------------------------------------------+-------------------------------+
| **Mnemonic**                                       | **Description**               |
+====================================================+===============================+
| **Event Load**                                     |                               |
+----------------------------------------------------+-------------------------------+
| **p.elw rD, Imm(rs1)**                             | rD = Mem32(Sext(Imm)+rs1)     |
+----------------------------------------------------+-------------------------------+

Encoding
~~~~~~~~

+-------------+--------+----------+--------+------------+---------------------------+
| 31   :   20 | 19 :15 | 14  : 12 | 11 :07 | 06  :   00 |                           |
+-------------+--------+----------+--------+------------+---------------------------+
| imm[11:0]   | rs1    | funct3   | rd     | opcode     | Mnemonic                  |
+=============+========+==========+========+============+===========================+
| offset      | base   | 110      | dest   | 000 0011   | **p.elw rD, Imm(rs1)**    |
+-------------+--------+----------+--------+------------+---------------------------+

.. _pulp_hardware_loop:

Hardware Loops
--------------

CV32E40P supports 2 levels of nested hardware loops. The loop has to be
setup before entering the loop body. For this purpose, there are two
methods, either the long commands that separately set start- and
end-addresses of the loop and the number of iterations, or the short
command that does all of this in a single instruction. The short command
has a limited range for the number of instructions contained in the loop
and the loop must start in the next instruction after the setup
instruction.

Hardware loop instructions and related CSRs are only supported if ``PULP_XPULP`` == 1.

Details about the hardware loop constraints are provided in :ref:`hwloop-specs`.

In the following tables, the hardware loop instructions are reported.
In assembly, **L** is referred by x0 or x1.

Operations
^^^^^^^^^^

**Long Hardware Loop Setup instructions**

+----------------------------------------------+-----------------------+----------------------------------+
| **Mnemonic**                                 | **Description**       |                                  |
+==============================================+=======================+==================================+
| **lp.starti**                                | **L, uimmL**          | lpstart[L] = PC + (uimmL << 1)   |
+----------------------------------------------+-----------------------+----------------------------------+
| **lp.endi**                                  | **L, uimmL**          | lpend[L] = PC + (uimmL << 1)     |
+----------------------------------------------+-----------------------+----------------------------------+
| **lp.count**                                 | **L, rs1**            | lpcount[L] = rs1                 |
+----------------------------------------------+-----------------------+----------------------------------+
| **lp.counti**                                | **L, uimmL**          | lpcount[L] = uimmL               |
+----------------------------------------------+-----------------------+----------------------------------+

**Short Hardware Loop Setup Instructions**

+----------------------------------------------+-----------------------+----------------------------------+
| **Mnemonic**                                 | **Description**       |                                  |
+==============================================+=======================+==================================+
| **lp.setup**                                 | **L, rs1, uimmL**     | lpstart[L] = pc + 4              |
|                                              |                       | lpend[L] = pc + (uimmL << 1)     |
|                                              |                       | lpcount[L] = rs1                 |
+----------------------------------------------+-----------------------+----------------------------------+
| **lp.setupi**                                | **L, uimmL, uimmS**   | lpstart[L] = pc + 4              |
|                                              |                       | lpend[L] = pc + (uimmS << 1)     |
|                                              |                       | lpcount[L] = uimmL               |
+----------------------------------------------+-----------------------+----------------------------------+

Encoding
~~~~~~~~

+-----------------+------------+----------+--------+----+------------+-------------------------------+
| 31   :   20     | 19 :15     | 14  : 12 | 11 :08 | 07 | 06  :   00 |                               |
+-----------------+------------+----------+--------+----+------------+-------------------------------+
| uimmL[11:0]     | rs1        | funct3   | rd     | L  | opcode     | Mnemonic                      |
+=================+============+==========+========+====+============+===============================+
| uimmL[11:0]     | 00000      | 000      | 0000   | L  | 111 1011   | **lp.starti L, uimmL**        |
+-----------------+------------+----------+--------+----+------------+-------------------------------+
| uimmL[11:0]     | 00000      | 001      | 0000   | L  | 111 1011   | **lp.endi L, uimmL**          |
+-----------------+------------+----------+--------+----+------------+-------------------------------+
| 0000 0000 0000  | src1       | 010      | 0000   | L  | 111 1011   | **lp.count L, rs1**           |
+-----------------+------------+----------+--------+----+------------+-------------------------------+
| uimmL[11:0]     | 00000      | 011      | 0000   | L  | 111 1011   | **lp.counti L, uimmL**        |
+-----------------+------------+----------+--------+----+------------+-------------------------------+
| uimmL[11:0]     | src1       | 100      | 0000   | L  | 111 1011   | **lp.setup L, rs1, uimmL**    |
+-----------------+------------+----------+--------+----+------------+-------------------------------+
| uimmL[11:0]     | uimmS[4:0] | 101      | 0000   |    | 111 1011   | **lp.setupi L, uimmS, uimmL** |
+-----------------+------------+----------+--------+----+------------+-------------------------------+

.. _pulp_alu:

ALU
---

CV32E40P supports advanced ALU operations that allow to perform multiple
instructions that are specified in the base instruction set in one
single instruction and thus increases efficiency of the core. For
example, those instructions include zero-/sign-extension instructions
for 8-bit and 16-bit operands, simple bit manipulation/counting
instructions and min/max/avg instructions. The ALU does also support
saturating, clipping, and normalizing instructions which make fixed-point
arithmetic more efficient.

The custom ALU extensions are only supported if ``PULP_XPULP`` == 1.

The custom extensions to the ALU are split into several subgroups that belong
together.

-  Bit manipulation instructions are useful to work on single bits or
   groups of bits within a word, see :ref:`pulp_bit_manipulation`.

-  General ALU instructions try to fuse common used sequences into a
   single instruction and thus increase the performance of small kernels
   that use those sequence, see :ref:`pulp_general_alu`.

-  Immediate branching instructions are useful to compare a register
   with an immediate value before taking or not a branch, see see :ref:`pulp_immediate_branching`.

Extract, Insert, Clear and Set instructions have the following meaning:

- Extract Is3+1 or rs2[9:5]+1 bits from position Is2 or rs2[4:0] [and sign extend it]

- Insert Is3+1 or rs2[9:5]+1 bits at position Is2 or rs2[4:0]

- Clear Is3+1 or rs2[9:5]+1 bits at position Is2 or rs2[4:0]

- Set Is3+1 or rs2[9:5]+1 bits at position Is2 or rs2[4:0]


Bit Reverse Instruction
^^^^^^^^^^^^^^^^^^^^^^^

This section will describe the `p.bitrev` instruction from a bit manipulation
perspective without describing it's application as part of an FFT. The bit
revserse instruction will reverse bits in groupings of 1, 2 or 3 bits. The
number of grouped bits is described by *Is3* as follows:

* **0** - reverse single bits
* **1** - reverse groups of 2 bits
* **2** - reverse groups of 3 bits

The number of bits that are reversed can be controlled by *Is2*. This will
specify the number of bits that will be removed by a left shift prior to
the reverse operation resulting in the *32-Is2* least significant bits of
the input value being reversed and the *Is2* most significant bits of the
input value being thrown out.

What follows is a few examples.

.. highlight:: none

::

   p.bitrev x18, x20, 0, 4 (groups of 1 bit; radix-2)

   in:    0xC64A5933 11000110010010100101100100110011
   shift: 0x64A59330 01100100101001011001001100110000
   out:   0x0CC9A526 00001100110010011010010100100110

   Swap pattern:
   A B C D E F G H . . . . . . . . . . . . . . . . . . . . . . . .
   0 1 1 0 0 1 0 0 1 0 1 0 0 1 0 1 1 0 0 1 0 0 1 1 0 0 1 1 0 0 0 0
   . . . . . . . . . . . . . . . . . . . . . . . . H G F E D C B A
   0 0 0 0 1 1 0 0 1 1 0 0 1 0 0 1 1 0 1 0 0 1 0 1 0 0 1 0 0 1 1 0

In this example the input value is first shifted by 4 (*Is2*). Each individual
bit is reversed. For example, bits 31 and 0 are swapped, 30 and 1, etc.

::

   p.bitrev x18, x20, 1, 4 (groups of 2 bits; radix-4)

   in:    0xC64A5933 11000110010010100101100100110011
   shift: 0x64A59330 01100100101001011001001100110000
   out:   0x0CC65A19 00001100110001100101101000011001

   Swap pattern:
   A  B  C  D  E  F  G  H  I  J  K  L  M  N  O  P
   01 10 01 00 10 10 01 01 10 01 00 11 00 11 00 00
   P  O  N  M  L  K  J  I  H  G  F  E  D  C  B  A
   00 00 11 00 11 00 01 10 01 01 10 10 00 01 10 01

In this example the input value is first shifted by 4 (*Is2*). Each group of
two bits are reversed. For example, bits 31 and 30 are swapped with 1 and 0
(retaining their position relative to each other), bits 29 and 28 are swapped
with 3 and 2, etc.

::

   p.bitrev x18, x20, 2, 4 (groups of 3 bits; radix-8)

   in:    0xC64A5933 11000110010010100101100100110011
   shift: 0x64A59330 01100100101001011001001100110000
   out:   0x216B244B 00100001011010110010010001001011

   Swap pattern:
   A   B   C   D   E   F   G   H   I   J
   011 001 001 010 010 110 010 011 001 100 00
      J   I   H   G   F   E   D   C   B   A
   00 100 001 011 010 110 010 010 001 001 011

In this last example the input value is first shifted by 4 (*Is2*). Each group
of three bits are reversed. For example, bits 31, 30 and 29 are swapped with
4, 3 and 2 (retaining their position relative to each other), bits 28, 27 and
26 are swapped with 7, 6 and 5, etc. Notice in this example that bits 0 and 1
are lost and the result is shifted right by two with bits 31 and 30 being tied
to zero. Also notice that when J (100) is swapped with A (011), the four most
significant bits are no longer zero as in the other cases. This may not be
desirable if the intention is to pack a specific number of grouped bits
aligned to the least significant bit and zero extended into the result. In
this case care should be taken to set *Is2* appropriately.


.. _pulp_bit_manipulation:

Bit Manipulation Operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^

+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **Mnemonic**      |                         | **Description**                                                                                                                          |
+===================+=========================+==========================================================================================================================================+
| **p.extract**     | **rD, rs1, Is3, Is2**   | rD = Sext(rs1[min(Is3+Is2,31):Is2])                                                                                                      |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.extractu**    | **rD, rs1, Is3, Is2**   | rD = Zext(rs1[min(Is3+Is2,31):Is2])                                                                                                      |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.extractr**    | **rD, rs1, rs2**        | rD = Sext(rs1[min(rs2[9:5]+rs2[4:0],31):rs2[4:0]])                                                                                       |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.extractur**   | **rD, rs1, rs2**        | rD = Zext(rs1[min(rs2[9:5]+rs2[4:0],31):rs2[4:0]])                                                                                       |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.insert**      | **rD, rs1, Is3, Is2**   | rD[min(Is3+Is2,31):Is2] = rs1[Is3:max(Is3+Is2,31)-31]                                                                                    |
|                   |                         | the rest of the bits of rD are passed through and are not modified                                                                       |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.insertr**     | **rD, rs1, rs2**        | rD[min(rs2[9:5]+rs2[4:0],31):rs2[4:0]] = rs1[rs2[9:5]:max(rs2[9:5]+rs2[4:0],31)-31]                                                      |
|                   |                         | the rest of the bits of rD are passed through and are not modified                                                                       |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.bclr**        | **rD, rs1, Is3, Is2**   | rD[min(Is3+Is2,31):Is2] = 0                                                                                                              |
|                   |                         | the rest of the bits of rD are passed through and are not modified                                                                       |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.bclrr**       | **rD, rs1, rs2**        | rD[min(rs2[9:5]+rs2[4:0],31):rs2[4:0]] = 0                                                                                               |
|                   |                         | the rest of the bits of rD are passed through and are not modified                                                                       |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.bset**        | **rD, rs1, Is3, Is2**   | rD[min(Is3+Is2,31):Is2] = (Is3+1)'1 (a vector of Is3+1 ones)                                                                             |
|                   |                         | the rest of the bits of rD are passed through and are not modified                                                                       |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.bsetr**       | **rD, rs1, rs2**        | rD[min(rs2[9:5]+rs2[4:0],31):rs2[4:0]] = (rs2[9:5]+1)'1 (a vector of rs2[9:5]+1                                                          |
|                   |                         | the rest of the bits of rD are passed through and are not modified                                                                       |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.ff1**         | **rD, rs1**             | rD = bit position of the first bit set in rs1, starting from LSB. If bit 0 is set, rD will be 0. If only bit 31 is set, rD will be 31.   |
|                   |                         | If rs1 is 0, rD will be 32.                                                                                                              |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.fl1**         | **rD, rs1**             | rD = bit position of the last bit set in rs1, starting from MSB. If bit 31 is set, rD will be 31. If only bit 0 is set, rD will be 0.    |
|                   |                         | If rs1 is 0, rD will be 32.                                                                                                              |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.clb**         | **rD, rs1**             | rD = count leading bits of rs1                                                                                                           |
|                   |                         | Note: This is the number of consecutive 1’s or 0’s from MSB.                                                                             |
|                   |                         | Note: If rs1 is 0, rD will be 0.                                                                                                         |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.cnt**         | **rD, rs1**             | rD = Population count of rs1, i.e. number of bits set in rs1                                                                             |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.ror**         | **rD, rs1, rs2**        | rD = RotateRight(rs1, rs2)                                                                                                               |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+
| **p.bitrev**      | **rD, rs1, Is3, Is2**   | Given an input rs1 it returns a bit reversed representation assuming                                                                     |
|                   |                         |                                                                                                                                          |
|                   |                         | FFT on 2^Is2 points in Radix 2^(Is3+1)                                                                                                   |
|                   |                         |                                                                                                                                          |
|                   |                         | Note: Is3 can be either 0 (radix-2), 1 (radix-4) or 2 (radix-8)                                                                          |
+-------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------+

**Note:** Sign extension is done over the extracted bit, i.e. the Is2-th bit.


Bit Manipulation Encoding
^^^^^^^^^^^^^^^^^^^^^^^^^

+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 31:30 | 29       :        25 | 24    :    20 | 19 :15 | 14 :  12 | 11 :07 | 06   :  00 |                                    |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| f2    | ls3[4:0]             | ls2[4:0]      | rs1    | funct3   | rd     | opcode     | Mnemonic                           |
+=======+======================+===============+========+==========+========+============+====================================+
| 11    | Luimm5[4:0]          | Iuimm5[4:0]   | src    | 000      | dest   | 011 0011   | **p.extract rD, rs1, Is3, Is2**    |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 11    | Luimm5[4:0]          | Iuimm5[4:0]   | src    | 001      | dest   | 011 0011   | **p.extractu rD, rs1, Is3, Is2**   |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 11    | Luimm5[4:0]          | Iuimm5[4:0]   | src    | 010      | dest   | 011 0011   | **p.insert rD, rs1, Is3, Is2**     |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 11    | Luimm5[4:0]          | Iuimm5[4:0]   | src    | 011      | dest   | 011 0011   | **p.bclr rD, rs1, Is3, Is2**       |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 11    | Luimm5[4:0]          | Iuimm5[4:0]   | src    | 100      | dest   | 011 0011   | **p.bset rD, rs1, Is3, Is2**       |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 10    | 5'b0_0000            | src2          | src1   | 000      | dest   | 011 0011   | **p.extractr rD, rs1, rs2**        |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 10    | 5'b0_0000            | src2          | src1   | 001      | dest   | 011 0011   | **p.extractur rD, rs1, rs2**       |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 10    | 5'b0_0000            | src2          | src1   | 010      | dest   | 011 0011   | **p.insertr rD, rs1, rs2**         |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 10    | 5'b0_0000            | src2          | src1   | 011      | dest   | 011 0011   | **p.bclrr rD, rs1, rs2**           |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 10    | 5'b0_0000            | src2          | scr1   | 100      | dest   | 011 0011   | **p.bsetr rD, rs1, rs2**           |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+
| 11    | {3'bXXX,Luimm2[1:0]} | Iuimm5[4:0]   | src    | 101      | dest   | 011 0011   | **p.bitrev rD, rs1, Is3, Is2**     |
+-------+----------------------+---------------+--------+----------+--------+------------+------------------------------------+

+------------+---------+--------+----------+--------+------------+--------------------------+
| 31   :  25 | 24 : 20 | 19 :15 | 14  : 12 | 11 : 7 | 6   :    0 |                          |
+------------+---------+--------+----------+--------+------------+--------------------------+
| funct7     | rs2     | rs1    | funct3   | rD     | opcode     |                          |
+============+=========+========+==========+========+============+==========================+
| 000 0100   | src2    | src1   | 101      | dest   | 011 0011   | **p.ror rD, rs1, rs2**   |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 1000   | 00000   | src1   | 000      | dest   | 011 0011   | **p.ff1 rD, rs1**        |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 1000   | 00000   | src1   | 001      | dest   | 011 0011   | **p.fl1 rD, rs1**        |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 1000   | 00000   | src1   | 010      | dest   | 011 0011   | **p.clb rD, rs1**        |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 1000   | 00000   | src1   | 011      | dest   | 011 0011   | **p.cnt rD, rs1**        |
+------------+---------+--------+----------+--------+------------+--------------------------+

.. _pulp_general_alu:

General ALU Operations
^^^^^^^^^^^^^^^^^^^^^^

+-----------------+-------------------------+------------------------------------------------------------------------+
| **Mnemonic**    |                         | **Description**                                                        |
+=================+=========================+========================================================================+
| **p.abs**       | **rD, rs1**             | rD = rs1 < 0 ? –rs1 : rs1                                              |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.slet**      | **rD, rs1, rs2**        | rD = rs1 <= rs2 ? 1 : 0                                                |
|                 |                         | Note: Comparison is signed                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.sletu**     | **rD, rs1, rs2**        | rD = rs1 <= rs2 ? 1 : 0                                                |
|                 |                         | Note: Comparison is unsigned                                           |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.min**       | **rD, rs1, rs2**        | rD = rs1 < rs2 ? rs1 : rs2                                             |
|                 |                         | Note: Comparison is signed                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.minu**      | **rD, rs1, rs2**        | rD = rs1 < rs2 ? rs1 : rs2                                             |
|                 |                         | Note: Comparison is unsigned                                           |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.max**       | **rD, rs1, rs2**        | rD = rs1 < rs2 ? rs2 : rs1                                             |
|                 |                         | Note: Comparison is signed                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.maxu**      | **rD, rs1, rs2**        | rD = rs1 < rs2 ? rs2 : rs1                                             |
|                 |                         | Note: Comparison is unsigned                                           |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.exths**     | **rD, rs1**             | rD = Sext(rs1[15:0])                                                   |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.exthz**     | **rD, rs1**             | rD = Zext(rs1[15:0])                                                   |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.extbs**     | **rD, rs1**             | rD = Sext(rs1[7:0])                                                    |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.extbz**     | **rD, rs1**             | rD = Zext(rs1[7:0])                                                    |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.clip**      | **rD, rs1, Is2**        |   if rs1 <= -2^(Is2-1), rD = -2^(Is2-1),                               |
|                 |                         |   else if rs1 >= 2^(Is2-1)–1, rD = 2^(Is2-1)-1,                        |
|                 |                         |   else rD = rs1                                                        |
|                 |                         |                                                                        |
|                 |                         | Note: If ls2 is equal to 0, -2^(Is2-1)= -1 while (2^(Is2-1)-1)=0;      |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.clipr**     | **rD, rs1, rs2**        | if rs1 <= -(rs2+1), rD = -(rs2+1),                                     |
|                 |                         | else if rs1 >=rs2, rD = rs2,                                           |
|                 |                         | else rD = rs1                                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.clipu**     | **rD, rs1, Is2**        |   if rs1 <= 0, rD = 0,                                                 |
|                 |                         |   else if rs1 >= 2^(Is2–1)-1, rD = 2^(Is2-1)-1,                        |
|                 |                         |   else rD = rs1                                                        |
|                 |                         |                                                                        |
|                 |                         | Note: If ls2 is equal to 0, (2^(Is2-1)-1)=0;                           |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.clipur**    | **rD, rs1, rs2**        | if rs1 <= 0, rD = 0,                                                   |
|                 |                         | else if rs1 >= rs2, rD = rs2,                                          |
|                 |                         | else rD = rs1                                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.addN**      | **rD, rs1, rs2, Is3**   | rD = (rs1 + rs2) >>> Is3                                               |
|                 |                         | Note: Arithmetic shift right. Setting Is3 to 2 replaces former p.avg   |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.adduN**     | **rD, rs1, rs2, Is3**   | rD = (rs1 + rs2) >> Is3                                                |
|                 |                         | Note: Logical shift right. Setting Is3 to 2 replaces former p.avg      |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.addRN**     | **rD, rs1, rs2, Is3**   | rD = (rs1 + rs2 + 2^(Is3-1)) >>> Is3                                   |
|                 |                         | Note: Arithmetic shift right.                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.adduRN**    | **rD, rs1, rs2, Is3**   | rD = (rs1 + rs2 + 2^(Is3-1))) >> Is3                                   |
|                 |                         | Note: Logical shift right.                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.addNr**     | **rD, rs1, rs2**        | rD = (rD + rs1) >>> rs2[4:0]                                           |
|                 |                         |                                                                        |
|                 |                         | Note: Arithmetic shift right.                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.adduNr**    | **rD, rs1, rs2**        | rD = (rD + rs1) >> rs2[4:0]                                            |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.addRNr**    | **rD, rs1, rs2**        | rD = (rD + rs1 + 2^(rs2[4:0]-1)) >>> rs2[4:0]                          |
|                 |                         | Note: Arithmetic shift right.                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.adduRNr**   | **rD, rs1, rs2**        | rD = (rD + rs1 + 2^(rs2[4:0]-1))) >> rs2[4:0]                          |
|                 |                         | Note: Logical shift right.                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.subN**      | **rD, rs1, rs2, Is3**   | rD = (rs1 - rs2) >>> Is3                                               |
|                 |                         | Note: Arithmetic shift right.                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.subuN**     | **rD, rs1, rs2, Is3**   | rD = (rs1 - rs2) >> Is3                                                |
|                 |                         | Note: Logical shift right.                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.subRN**     | **rD, rs1, rs2, Is3**   | rD = (rs1 - rs2 + 2^(Is3-1)) >>> Is3                                   |
|                 |                         | Note: Arithmetic shift right.                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.subuRN**    | **rD, rs1, rs2, Is3**   | rD = (rs1 - rs2 + 2^(Is3-1))) >> Is3                                   |
|                 |                         | Note: Logical shift right.                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.subNr**     | **rD, rs1, rs2**        | rD = (rD – rs1) >>> rs2[4:0]                                           |
|                 |                         | Note: Arithmetic shift right.                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.subuNr**    | **rD, rs1, rs2**        | rD = (rD – rs1) >> rs2[4:0]                                            |
|                 |                         | Note: Logical shift right.                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.subRNr**    | **rD, rs1, rs2**        | rD = (rD – rs1+ 2^(rs2[4:0]-1)) >>> rs2[4:0]                           |
|                 |                         | Note: Arithmetic shift right.                                          |
+-----------------+-------------------------+------------------------------------------------------------------------+
| **p.subuRNr**   | **rD, rs1, rs2**        | rD = (rD – rs1+ 2^(rs2[4:0]-1))) >> rs2[4:0]                           |
|                 |                         | Note: Logical shift right.                                             |
+-----------------+-------------------------+------------------------------------------------------------------------+

General ALU Encoding
^^^^^^^^^^^^^^^^^^^^

+------------+---------+--------+----------+--------+------------+--------------------------+
| 31   :  25 | 24 : 20 | 19 :15 | 14 :  12 | 11 : 7 | 6  :     0 |                          |
+------------+---------+--------+----------+--------+------------+--------------------------+
| funct7     | rs2     | rs1    | funct    | rD     | opcode     |                          |
+============+=========+========+==========+========+============+==========================+
| 000 0010   | 00000   | src1   | 000      | dest   | 011 0011   | **p.abs rD, rs1**        |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 0010   | src2    | src1   | 010      | dest   | 011 0011   | **p.slet rD, rs1, rs2**  |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 0010   | src2    | src1   | 011      | dest   | 011 0011   | **p.sletu rD, rs1, rs2** |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 0010   | src2    | src1   | 100      | dest   | 011 0011   | **p.min rD, rs1, rs2**   |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 0010   | src2    | src1   | 101      | dest   | 011 0011   | **p.minu rD, rs1, rs2**  |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 0010   | src2    | src1   | 110      | dest   | 011 0011   | **p.max rD, rs1, rs2**   |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 0010   | src2    | src1   | 111      | dest   | 011 0011   | **p.maxu rD, rs1, rs2**  |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 1000   | 00000   | src1   | 100      | dest   | 011 0011   | **p.exths rD, rs1**      |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 1000   | 00000   | src1   | 101      | dest   | 011 0011   | **p.exthz rD, rs1**      |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 1000   | 00000   | src1   | 110      | dest   | 011 0011   | **p.extbs rD, rs1**      |
+------------+---------+--------+----------+--------+------------+--------------------------+
| 000 1000   | 00000   | src1   | 111      | dest   | 011 0011   | **p.extbz rD, rs1**      |
+------------+---------+--------+----------+--------+------------+--------------------------+


+------------+---------------+--------+----------+--------+------------+-----------------------------+
| 31  :   25 | 24   :     20 | 19 :15 | 14  : 12 | 11 : 7 | 6   :    0 |                             |
+------------+---------------+--------+----------+--------+------------+-----------------------------+
| funct7     | Is2[4:0]      | rs1    | funct3   | rD     | opcode     |                             |
+============+===============+========+==========+========+============+=============================+
| 000 1010   | Iuimm5[4:0]   | src1   | 001      | dest   | 011 0011   | **p.clip rD, rs1, Is2**     |
+------------+---------------+--------+----------+--------+------------+-----------------------------+
| 000 1010   | Iuimm5[4:0]   | src1   | 010      | dest   | 011 0011   | **p.clipu rD, rs1, Is2**    |
+------------+---------------+--------+----------+--------+------------+-----------------------------+
| 000 1010   | src2          | src1   | 010      | dest   | 011 0011   | **p.clipr rD, rs1, Is2**    |
+------------+---------------+--------+----------+--------+------------+-----------------------------+
| 000 1010   | src2          | src1   | 110      | dest   | 011 0011   | **p.clipur rD, rs1, Is2**   |
+------------+---------------+--------+----------+--------+------------+-----------------------------+

+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 31:30 | 29   :    25  | 24 :20 | 19 :15 | 14  : 12 | 11 : 7 | 6   :    0 |                                  |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| f2    | Is3[4:0]      | rs2    | rs1    | funct3   | rD     | opcode     |                                  |
+=======+===============+========+========+==========+========+============+==================================+
| 00    | Luimm5[4:0]   | src2   | src1   | 010      | dest   | 101 1011   | **p.addN rD, rs1, rs2, Is3**     |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 10    | Luimm5[4:0]   | src2   | src1   | 010      | dest   | 101 1011   | **p.adduN rD, rs1, rs2, Is3**    |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 00    | Luimm5[4:0]   | src2   | src1   | 110      | dest   | 101 1011   | **p.addRN rD, rs1, rs2, Is3**    |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 10    | Luimm5[4:0]   | src2   | src1   | 110      | dest   | 101 1011   | **p.adduRN rD, rs1, rs2, Is3**   |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 00    | Luimm5[4:0]   | src2   | src1   | 011      | dest   | 101 1011   | **p.subN rD, rs1, rs2, Is3**     |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 10    | Luimm5[4:0]   | src2   | src1   | 011      | dest   | 101 1011   | **p.subuN rD, rs1, rs2, Is3**    |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 00    | Luimm5[4:0]   | src2   | src1   | 111      | dest   | 101 1011   | **p.subRN rD, rs1, rs2, Is3**    |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 10    | Luimm5[4:0]   | src2   | src1   | 111      | dest   | 101 1011   | **p.subuRN rD, rs1, rs2, Is3**   |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 01    | Luimm5[4:0]   | src2   | src1   | 010      | dest   | 101 1011   | **p.addNr rD, rs1, rs2**         |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 11    | 00000         | src2   | src1   | 010      | dest   | 101 1011   | **p.adduNr rD, rs1, rs**         |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 01    | 00000         | src2   | src1   | 110      | dest   | 101 1011   | **p.addRNr rD, rs1, rs**         |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 11    | 00000         | src2   | src1   | 110      | dest   | 101 1011   | **p.adduRNr rD, rs1, rs2**       |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 01    | 00000         | src2   | src1   | 011      | dest   | 101 1011   | **p.subNr rD, rs1, rs2**         |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 11    | 00000         | src2   | src1   | 011      | dest   | 101 1011   | **p.subuN r rD, rs1, rs2**       |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 01    | 00000         | src2   | src1   | 111      | dest   | 101 1011   | **p.subRNr rD, rs1, rs2**        |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+
| 11    | 00000         | src2   | src1   | 111      | dest   | 101 1011   | **p.subuRNr rD, rs1, rs2**       |
+-------+---------------+--------+--------+----------+--------+------------+----------------------------------+

.. _pulp_immediate_branching:

Immediate Branching Operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+---------------------------------+------------------------------------------------------------------------+
| **Mnemonic**                    | **Description**                                                        |
+=================================+========================================================================+
| **p.beqimm rs1, Imm5, Imm12**   | Branch to PC + (Imm12 << 1) if rs1 is equal to Imm5. Imm5 is signed.   |
+---------------------------------+------------------------------------------------------------------------+
| **p.bneimm rs1, Imm5, Imm12**   | Branch to PC + (Imm12 << 1) if rs1 is not equal to Imm5.               |
|                                 | Imm5 is signed.                                                        |
+---------------------------------+------------------------------------------------------------------------+

Immediate Branching Encoding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+---------+----------+---------+----------+---------+----------+--------+------------+---------------------------------+
| 31 : 29 | 28 :  24 | 23 : 19 | 18  : 16 | 15 : 13 | 12 :   9 | 8 :  7 | 6  :     0 |                                 |
+---------+----------+---------+----------+---------+----------+--------+------------+---------------------------------+
| Imm12   | Imm5     | rs1     | funct3   | Imm12   |          |        | opcode     |                                 |
+=========+==========+=========+==========+=========+==========+========+============+=================================+
|         |          |         | src1     | 010     | [4:1]    | [11]   | 110 0011   | **p.beqimm rs1, Imm5, Imm12**   |
+---------+----------+---------+----------+---------+----------+--------+------------+---------------------------------+
|         |          |         | src1     | 011     | [4:1]    | [11]   | 1100011    | **p.bneimm rs1, Imm5, Imm12**   |
+---------+----------+---------+----------+---------+----------+--------+------------+---------------------------------+

.. _pulp_multiply_accumulate:

Multiply-Accumulate
-------------------

CV32E40P supports custom extensions for multiply-accumulate and half-word multiplications with
an optional post-multiplication shift.

The custom multiply-accumulate extensions are only supported if ``PULP_XPULP`` == 1.

MAC Operations
^^^^^^^^^^^^^^

32-Bit x 32-Bit Multiplication Operations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------------+-------------------------+------------------------------------------------------------------------------+
| **Mnemonic**      | **Description**         |                                                                              |
+===================+=========================+==============================================================================+
| **p.mac**         | **rD, rs1, rs2**        | rD = rD + rs1 \* rs2                                                         |
+-------------------+-------------------------+------------------------------------------------------------------------------+
| **p.msu**         | **rD, rs1, rs2**        | rD = rD - rs1 \* rs2                                                         |
+-------------------+-------------------------+------------------------------------------------------------------------------+

16-Bit x 16-Bit Multiplication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------------+---------------------------+------------------------------------------------------------------------------+
| **Mnemonic**      | **Description**           |                                                                              |
+===================+===========================+==============================================================================+
| **p.muls**        | **rD, rs1, rs2**          | rD[31:0] = Sext(rs1[15:0]) \* Sext(rs2[15:0])                                |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulhhs**      | **rD, rs1, rs2**          | rD[31:0] = Sext(rs1[31:16]) \* Sext(rs2[31:16])                              |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulsN**       | **rD, rs1, rs2, Is3**     | rD[31:0] = (Sext(rs1[15:0]) \* Sext(rs2[15:0])) >>> Is3                      |
|                   |                           | Note: Arithmetic shift right                                                 |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulhhsN**     | **rD, rs1, rs2, Is3**     | rD[31:0] = (Sext(rs1[31:16]) \* Sext(rs2[31:16])) >>> Is3                    |
|                   |                           | Note: Arithmetic shift right                                                 |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulsRN**      | **rD, rs1, rs2, Is3**     | rD[31:0] = (Sext(rs1[15:0]) \* Sext(rs2[15:0]) + 2^(Is3-1)) >>> Is3          |
|                   |                           | Note: Arithmetic shift right                                                 |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulhhsRN**    | **rD, rs1, rs2, Is3**     | rD[31:0] = (Sext(rs1[31:16]) \* Sext(rs2[31:16]) + 2^(Is3-1)) >>> Is3        |
|                   |                           | Note: Arithmetic shift right                                                 |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulu**        | **rD, rs1, rs2**          | rD[31:0] = Zext(rs1[15:0]) \* Zext(rs2[15:0])                                |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulhhu**      | **rD, rs1, rs2**          | rD[31:0] = Zext(rs1[31:16]) \* Zext(rs2[31:16])                              |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.muluN**       | **rD, rs1, rs2, Is3**     | rD[31:0] = (Zext(rs1[15:0]) \* Zext(rs2[15:0])) >> Is3                       |
|                   |                           | Note: Logical shift right                                                    |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulhhuN**     | **rD, rs1, rs2, Is3**     | rD[31:0] = (Zext(rs1[31:16]) \* Zext(rs2[31:16])) >> Is3                     |
|                   |                           | Note: Logical shift right                                                    |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.muluRN**      | **rD, rs1, rs2, Is3**     | rD[31:0] = (Zext(rs1[15:0]) \* Zext(rs2[15:0]) + 2^(Is3-1)) >> Is3           |
|                   |                           | Note: Logical shift right                                                    |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.mulhhuRN**    | **rD, rs1, rs2, Is3**     | rD[31:0] = (Zext(rs1[31:16]) \* Zext(rs2[31:16]) + 2^(Is3-1)) >> Is3         |
|                   |                           | Note: Logical shift right                                                    |
+-------------------+---------------------------+------------------------------------------------------------------------------+

16-Bit x 16-Bit Multiply-Accumulate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------------+---------------------------+------------------------------------------------------------------------------+
| **Mnemonic**      | **Description**           |                                                                              |
+===================+===========================+==============================================================================+
| **p.macsN**       | **rD, rs1, rs2, Is3**     | rD[31:0] = (Sext(rs1[15:0]) \* Sext(rs2[15:0]) + rD) >>> Is3                 |
|                   |                           | Note: Arithmetic shift right                                                 |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.machhsN**     | **rD, rs1, rs2, Is3**     | rD[31:0] = (Sext(rs1[31:16]) \* Sext(rs2[31:16]) + rD) >>> Is3               |
|                   |                           | Note: Arithmetic shift right                                                 |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.macsRN**      | **rD, rs1, rs2, Is3**     | rD[31:0] = (Sext(rs1[15:0]) \* Sext(rs2[15:0]) + rD + 2^(Is3-1)) >>> Is3     |
|                   |                           | Note: Arithmetic shift right                                                 |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.machhsRN**    | **, rD, rs1, rs2, Is3**   | rD[31:0] = (Sext(rs1[31:16]) \* Sext(rs2[31:16]) + rD + 2^(Is3-1)) >>> Is3   |
|                   |                           | Note: Arithmetic shift right                                                 |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.macuN**       | **rD, rs1, rs2, Is3**     | rD[31:0] = (Zext(rs1[15:0]) \* Zext(rs2[15:0]) + rD) >> Is3                  |
|                   |                           | Note: Logical shift right                                                    |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.machhuN**     | **rD, rs1, rs2, Is3**     | rD[31:0] = (Zext(rs1[31:16]) \* Zext(rs2[31:16]) + rD) >> Is3                |
|                   |                           | Note: Logical shift right                                                    |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.macuRN**      | **rD, rs1, rs2, Is3**     | rD[31:0] = (Zext(rs1[15:0]) \* Zext(rs2[15:0]) + rD + 2^(Is3-1)) >> Is3      |
|                   |                           | Note: Logical shift right                                                    |
+-------------------+---------------------------+------------------------------------------------------------------------------+
| **p.machhuRN**    | **rD, rs1, rs2, Is3**     | rD[31:0] = (Zext(rs1[31:16]) \* Zext(rs2[31:16]) + rD + 2^(Is3-1)) >> Is3    |
|                   |                           | Note: Logical shift right                                                    |
+-------------------+---------------------------+------------------------------------------------------------------------------+

MAC Encoding
^^^^^^^^^^^^

+------------+--------+--------+----------+--------+------------+--------------------------+
| 31   :  25 | 24 :20 | 19 :15 | 14  : 12 | 11 : 7 | 6   :    0 |                          |
+------------+--------+--------+----------+--------+------------+--------------------------+
| funct7     | rs2    | rs1    | funct3   | rD     | opcode     |                          |
+============+========+========+==========+========+============+==========================+
| 010 0001   | src2   | src1   | 000      | dest   | 011 0011   | **p.mac rD, rs1, rs2**   |
+------------+--------+--------+----------+--------+------------+--------------------------+
| 010 0001   | src2   | src1   | 001      | dest   | 011 0011   | **p.msu rD, rs1, rs2**   |
+------------+--------+--------+----------+--------+------------+--------------------------+

+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 31:30 | 29   :    25  | 24 :20 | 19 :15 | 14  : 12 | 11 : 7 | 6   :    0 |                                    |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| f2    | Is3[4:0]      | rs2    | rs1    | funct3   | rD     | opcode     |                                    |
+=======+===============+========+========+==========+========+============+====================================+
| 10    | 00000         | src2   | src1   | 000      | dest   | 101 1011   | **p.muls rD, rs1, rs2**            |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 11    | 00000         | src2   | src1   | 000      | dest   | 101 1011   | **p.mulhhs rD, rs1, rs2**          |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 10    | Luimm5[4:0]   | src2   | src1   | 000      | dest   | 101 1011   | **p.mulsN rD, rs1, rs2, Is3**      |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 11    | Luimm5[4:0]   | src2   | src1   | 000      | dest   | 101 1011   | **p.mulhhsN rD, rs1, rs2, Is3**    |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 10    | Luimm5[4:0]   | src2   | src1   | 100      | dest   | 101 1011   | **p.mulsRN rD, rs1, rs2, Is3**     |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 11    | Luimm5[4:0]   | src2   | src1   | 100      | dest   | 101 1011   | **p.mulhhsRN rD, rs1, rs2, Is3**   |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 00    | 00000         | src2   | src1   | 000      | dest   | 101 1011   | **p.mulu rD, rs1, rs2**            |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 01    | 00000         | src2   | src1   | 000      | dest   | 101 1011   | **p.mulhhu rD, rs1, rs2**          |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 00    | Luimm5[4:0]   | src2   | src1   | 000      | dest   | 101 1011   | **p.muluN rD, rs1, rs2, Is3**      |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 01    | Luimm5[4:0]   | src2   | src1   | 000      | dest   | 101 1011   | **p.mulhhuN rD, rs1, rs2, Is3**    |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 00    | Luimm5[4:0]   | src2   | src1   | 100      | dest   | 101 1011   | **p.muluRN rD, rs1, rs2, Is3**     |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 01    | Luimm5[4:0]   | src2   | src1   | 100      | dest   | 101 1011   | **p.mulhhuRN rD, rs1, rs2, Is3**   |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 10    | Luimm5[4:0]   | src2   | src1   | 001      | dest   | 101 1011   | **p.macsN rD, rs1, rs2, Is3**      |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 11    | Luimm5[4:0]   | src2   | src1   | 001      | dest   | 101 1011   | **p.machhsN rD, rs1, rs2, Is3**    |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 10    | Luimm5[4:0]   | src2   | src1   | 101      | dest   | 101 1011   | **p.macsRN rD, rs1, rs2, Is3**     |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 11    | Luimm5[4:0]   | src2   | src1   | 101      | dest   | 101 1011   | **p.machhsRN rD, rs1, rs2, Is3**   |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 00    | Luimm5[4:0]   | src2   | src1   | 001      | dest   | 101 1011   | **p.macuN rD, rs1, rs2, Is3**      |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 01    | Luimm5[4:0]   | src2   | src1   | 001      | dest   | 101 1011   | **p.machhuN rD, rs1, rs2, Is3**    |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 00    | Luimm5[4:0]   | src2   | src1   | 101      | dest   | 101 1011   | **p.macuRN rD, rs1, rs2, Is3**     |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+
| 01    | Luimm5[4:0]   | src2   | src1   | 101      | dest   | 101 1011   | **p.machhuRN rD, rs1, rs2, Is3**   |
+-------+---------------+--------+--------+----------+--------+------------+------------------------------------+

.. _pulp_simd:

SIMD
---------

The SIMD instructions perform operations on
multiple sub-word elements at the same time. This is done by segmenting
the data path into smaller parts when 8 or 16-bit operations should be
performed.

The custom SIMD extensions are only supported if ``PULP_XPULP`` == 1.

SIMD instructions are available in two flavors:

-  8-Bit, to perform four operations on the 4 bytes inside a 32-bit word
   at the same time (.b)

-  16-Bit, to perform two operations on the 2 half-words inside a 32-bit
   word at the same time (.h)

All the operations are rounded to the specified bidwidth as for the original
RISC-V arithmetic operations. This is described by the "and" operation with a
MASK. No overflow or carry-out flags are generated as for the 32-bit operations.

Additionally, there are three modes that influence the second operand:

1. Normal mode, vector-vector operation. Both operands, from rs1 and
   rs2, are treated as vectors of bytes or half-words.

   e.g. pv.add.h x3,x2,x1 performs:

    x3[31:16] = x2[31:16] + x1[31:16]

    x3[15: 0] = x2[15: 0] + x1[15: 0]


2. Scalar replication mode (.sc), vector-scalar operation. Operand 1 is
   treated as a vector, while operand 2 is treated as a scalar and
   replicated two or four times to form a complete vector. The LSP is
   used for this purpose.

   e.g. pv.add.sc.h x3,x2,x1 performs:

    x3[31:16] = x2[31:16] + x1[15: 0]

    x3[15: 0] = x2[15: 0] + x1[15: 0]



3. Immediate scalar replication mode (.sci), vector-scalar operation.
   Operand 1 is treated as vector, while operand 2 is treated as a
   scalar and comes from an immediate. The immediate is either sign- or
   zero-extended, depending on the operation. If not specified, the
   immediate is sign-extended.

   e.g. pv.add.sci.h x3,x2,0xDA performs:

    x3[31:16] = x2[31:16] + 0xFFDA

    x3[15: 0] = x2[15: 0] + 0xFFDA

In the following Table, the index i ranges from 0 to 1 for 16-Bit
operations and from 0 to 3 for 8-Bit operations.

- The index 0 is 15:0  for 16-Bit operations, or   7:0 for 8-Bit operations.
- The index 1 is 31:16 for 16-Bit operations, or  15:8 for 8-Bit operations.
- The index 2 is 23:16 for 8-Bit operations.
- The index 3 is 31:24 for 8-Bit operations.

SIMD ALU Operations
^^^^^^^^^^^^^^^^^^^

+---------------------------------------+---------------------------------------------------------------------------------------+
| **Mnemonic**                          | **Description**                                                                       |
+=======================================+=======================================================================================+
| **pv.add[.sc,.sci]{.h,.b}**           | rD[i] = (rs1[i] + op2[i]) & 0xFFFF                                                    |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.add{.div2,.div4, .div8}**        | rD[i] = ((rs1[i] + op2[i]) & 0xFFFF)>>{1,2,3}                                         |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sub[.sc,.sci]{.h,.b}**           | rD[i] = (rs1[i] - op2[i]) & 0xFFFF                                                    |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sub{.div2,.div4, .div8}**        | rD[i] = ((rs1[i] – op2[i]) & 0xFFFF)>>{1,2,3}                                         |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.avg[.sc,.sci]{.h,.b}**           | rD[i] = ((rs1[i] + op2[i]) & {0xFFFF, 0xFF}) >> 1                                     |
|                                       | Note: Arithmetic right shift                                                          |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.avgu[.sc,.sci]{.h,.b}**          | rD[i] = ((rs1[i] + op2[i]) & {0xFFFF, 0xFF}) >> 1                                     |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.min[.sc,.sci]{.h,.b}**           | rD[i] = rs1[i] < op2[i] ? rs1[i] : op2[i]                                             |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.minu[.sc,.sci]{.h,.b}**          | rD[i] = rs1[i] < op2[i] ? rs1[i] : op2[i]                                             |
|                                       | Note: Immediate is zero-extended, comparison is unsigned                              |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.max[.sc,.sci]{.h,.b}**           | rD[i] = rs1[i] > op2[i] ? rs1[i] : op2[i]                                             |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.maxu[.sc,.sci]{.h,.b}**          | rD[i] = rs1[i] > op2[i] ? rs1[i] : op2[i]                                             |
|                                       | Note: Immediate is zero-extended, comparison is unsigned                              |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.srl[.sc,.sci]{.h,.b}**           | rD[i] = rs1[i] >> op2[i]                                                              |
|                                       | Note: Immediate is zero-extended, shift is logical                                    |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sra[.sc,.sci]{.h,.b}**           | rD[i] = rs1[i] >>> op2[i]                                                             |
|                                       | Note: Immediate is zero-extended, shift is arithmetic                                 |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sll[.sc,.sci]{.h,.b}**           | rD[i] = rs1[i] << op2[i]                                                              |
|                                       | Note: Immediate is zero-extended, shift is logical                                    |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.or[.sc,.sci]{.h,.b}**            | rD[i] = rs1[i] \| op2[i]                                                              |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.xor[.sc,.sci]{.h,.b}**           | rD[i] = rs1[i] ^ op2[i]                                                               |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.and[.sc,.sci]{.h,.b}**           | rD[i] = rs1[i] & op2[i]                                                               |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.abs{.h,.b}**                     | rD[i] = rs1 < 0 ? –rs1 : rs1                                                          |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.extract.h**                      | rD = Sext(rs1[((I+1)\*16)-1 : I\*16])                                                 |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.extract.b**                      | rD = Sext(rs1[((I+1)\*8)-1 : I\*8])                                                   |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.extractu.h**                     | rD = Zext(rs1[((I+1)\*16)-1 : I\*16])                                                 |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.extractu.b**                     | rD = Zext(rs1[((I+1)\*8)-1 : I\*8])                                                   |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.insert.h**                       | rD[((I+1)\*16-1:I\*16] = rs1[15:0]                                                    |
|                                       | Note: The rest of the bits of rD are untouched and keep their previous value          |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.insert,b**                       | rD[((I+1)\*8-1:I\*8] = rs1[7:0]                                                       |
|                                       | Note: The rest of the bits of rD are untouched and keep their previous value          |
+---------------------------------------+---------------------------------------------------------------------------------------+

Dot Product Instructions
~~~~~~~~~~~~~~~~~~~~~~~~

+---------------------------------------+---------------------------------------------------------------------------------------+
| **Mnemonic**                          | **Description**                                                                       |
+=======================================+=======================================================================================+
| **pv.dotup[.sc,.sci].h**              | rD = rs1[0] \* op2[0] + rs1[1] \* op2[1]                                              |
|                                       | Note: All operations are unsigned                                                     |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.dotup[.sc,.sci].b**              | rD = rs1[0] \* op2[0] + rs1[1] \* op2[1] + rs1[2] \* op2[2] + rs1[3] \* op2[3]        |
|                                       | Note: All operations are unsigned                                                     |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.dotusp[.sc,.sci].h**             | rD = rs1[0] \* op2[0] + rs1[1] \* op2[1]                                              |
|                                       | Note: rs1 is treated as unsigned, while rs2 is treated as signed                      |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.dotusp[.sc,.sci].b**             | rD = rs1[0] \* op2[0] + rs1[1] \* op2[1] + rs1[2] \* op2[2] + rs1[3] \* op2[3]        |
|                                       | Note: rs1 is treated as unsigned, while rs2 is treated as signed                      |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.dotsp[.sc,.sci].h**              | rD = rs1[0] \* op2[0] + rs1[1] \* op2[1]                                              |
|                                       | Note: All operations are signed                                                       |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.dotsp[.sc,.sci].b**              | rD = rs1[0] \* op2[0] + rs1[1] \* op2[1] + rs1[2] \* op2[2] + rs1[3] \* op2[3]        |
|                                       | Note: All operations are signed                                                       |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sdotup[.sc,.sci].h**             | rD = rD + rs1[0] \* op2[0] + rs1[1] \* op2[1]                                         |
|                                       | Note: All operations are unsigned                                                     |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sdotup[.sc,.sci].b**             | rD = rD + rs1[0] \* op2[0] + rs1[1] \* op2[1] + rs1[2] \* op2[2] + rs1[3] \* op2[3]   |
|                                       | Note: All operations are unsigned                                                     |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sdotusp[.sc,.sci].h**            | rD = rD + rs1[0] \* op2[0] + rs1[1] \* op2[1]                                         |
|                                       | Note: rs1 is treated as unsigned, while rs2 is treated as signed                      |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sdotusp[.sc,.sci].b**            | rD = rD + rs1[0] \* op2[0] + rs1[1] \* op2[1] + rs1[2] \* op2[2] + rs1[3] \* op2[3]   |
|                                       | Note: rs1 is treated as unsigned, while rs2 is treated as signed                      |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sdotsp[.sc,.sci].h**             | rD = rD + rs1[0] \* op2[0] + rs1[1] \* op2[1]                                         |
|                                       | Note: All operations are signed                                                       |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.sdotsp[.sc,.sci].b**             | rD = rD + rs1[0] \* op2[0] + rs1[1] \* op2[1] + rs1[2] \* op2[2] + rs1[3] \* op2[3]   |
|                                       | Note: All operations are signed                                                       |
+---------------------------------------+---------------------------------------------------------------------------------------+

Shuffle and Pack Instructions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+---------------------------------------+---------------------------------------------------------------------------------------+
| **Mnemonic**                          | **Description**                                                                       |
+=======================================+=======================================================================================+
| **pv.shuffle.h**                      | rD[31:16] = rs1[rs2[16]\*16+15:rs2[16]\*16]                                           |
|                                       | rD[15:0] = rs1[rs2[0]\*16+15:rs2[0]\*16]                                              |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.shuffle.sci.h**                  | rD[31:16] = rs1[I1\*16+15:I1\*16]                                                     |
|                                       | rD[15:0] = rs1[I0\*16+15:I0\*16]                                                      |
|                                       | Note: I1 and I0 represent bits 1 and 0 of the immediate                               |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.shuffle.b**                      | rD[31:24] = rs1[rs2[25:24]\*8+7:rs2[25:24]\*8]                                        |
|                                       | rD[23:16] = rs1[rs2[17:16]\*8+7:rs2[17:16]\*8]                                        |
|                                       | rD[15:8] = rs1[rs2[9:8]\*8+7:rs2[9:8]\*8]                                             |
|                                       | rD[7:0] = rs1[rs2[1:0]\*8+7:rs2[1:0]\*8]                                              |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.shuffleI0.sci.b**                | rD[31:24] = rs1[7:0]                                                                  |
|                                       | rD[23:16] = rs1[(I5:I4)\*8+7: (I5:I4)\*8]                                             |
|                                       | rD[15:8] = rs1[(I3:I2)\*8+7: (I3:I2)\*8]                                              |
|                                       | rD[7:0] = rs1[(I1:I0)\*8+7:(I1:I0)\*8]                                                |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.shuffleI1.sci.b**                | rD[31:24] = rs1[15:8]                                                                 |
|                                       | rD[23:16] = rs1[(I5:I4)\*8+7: (I5:I4)\*8]                                             |
|                                       | rD[15:8] = rs1[(I3:I2)\*8+7: (I3:I2)\*8]                                              |
|                                       | rD[7:0] = rs1[(I1:I0)\*8+7:(I1:I0)\*8]                                                |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.shuffleI2.sci.b**                | rD[31:24] = rs1[23:16]                                                                |
|                                       | rD[23:16] = rs1[(I5:I4)\*8+7: (I5:I4)\*8]                                             |
|                                       | rD[15:8] = rs1[(I3:I2)\*8+7: (I3:I2)\*8]                                              |
|                                       | rD[7:0] = rs1[(I1:I0)\*8+7:(I1:I0)\*8]                                                |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.shuffleI3.sci.b**                | rD[31:24] = rs1[31:24]                                                                |
|                                       | rD[23:16] = rs1[(I5:I4)\*8+7: (I5:I4)\*8]                                             |
|                                       | rD[15:8] = rs1[(I3:I2)\*8+7: (I3:I2)\*8]                                              |
|                                       | rD[7:0] = rs1[(I1:I0)\*8+7:(I1:I0)\*8]                                                |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.shuffle2.h**                     | rD[31:16] = ((rs2[17] == 1) ? rs1 : rD)[rs2[16]\*16+15:rs2[16]\*16]                   |
|                                       | rD[15:0] = ((rs2[1] == 1) ? rs1 : rD)[rs2[0]\*16+15:rs2[0]\*16]                       |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.shuffle2.b**                     | rD[31:24] = ((rs2[26] == 1) ? rs1 : rD)[rs2[25:24]\*8+7:rs2[25:24]\*8]                |
|                                       | rD[23:16] = ((rs2[18] == 1) ? rs1 : rD)[rs2[17:16]\*8+7:rs2[17:16]\*8]                |
|                                       | rD[15:8] = ((rs2[10] == 1) ? rs1 : rD)[rs2[9:8]\*8+7:rs2[9:8]\*8]                     |
|                                       | rD[7:0] = ((rs2[2] == 1) ? rs1 : rD)[rs2[1:0]\*8+7:rs2[1:0]\*8]                       |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.pack**                           | rD[31:16] = rs1[15:0]                                                                 |
|                                       | rD[15:0] = rs2[15:0]                                                                  |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.pack.h**                         | rD[31:16] = rs1[31:16]                                                                |
|                                       | rD[15:0] = rs2[31:16]                                                                 |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.packhi.b**                       | rD[31:24] = rs1[7:0]                                                                  |
|                                       | rD[23:16] = rs2[7:0]                                                                  |
|                                       | Note: The rest of the bits of rD are untouched and keep their previous value          |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.packlo.b**                       | rD[15:8] = rs1[7:0]                                                                   |
|                                       | rD[7:0] = rs2[7:0]                                                                    |
|                                       | Note: The rest of the bits of rD are untouched and keep their previous value          |
+---------------------------------------+---------------------------------------------------------------------------------------+

SIMD ALU Encoding
^^^^^^^^^^^^^^^^^

+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 31  : 27 | 26  | 25 | 24 : 20 | 19 : 15 | 14 :12 | 11  :  7 | 6   :  0 |                                  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| funct5   | F   |    | rs2     | rs1     | funct3 | rD       | opcode   |                                  |
+==========+=====+====+=========+=========+========+==========+==========+==================================+
| 0 0000   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.add.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0000   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.add.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0000   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.add.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0000   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.add.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0000   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.add.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0000   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.add.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 1   | X  | src2    | src1    | 01x    | dest     | 101 0111 | **pv.add.div2 rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 1   | X  | src2    | src1    | 10x    | dest     | 101 0111 | **pv.add.div4 rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 1   | x  | src2    | src1    | 11x    | dest     | 101 0111 | **pv.add.div8 rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0001   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.sub.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0001   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.sub.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0001   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.sub.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0001   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.sub.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0001   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.sub.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0001   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.sub.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 1   | x  | src2    | src1    | 01x    | dest     | 101 0111 | **pv.sub.div2 rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 1   | x  | src2    | src1    | 10x    | dest     | 101 0111 | **pv.sub.div4 rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 1   | x  | src2    | src1    | 11x    | dest     | 101 0111 | **pv.sub.div8 rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0010   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.avg.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0010   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.avg.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0010   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.avg.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0010   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.avg.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0010   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.avg.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0010   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.avg.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0011   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.avgu.h rD, rs1, rs2**       |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0011   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.avgu.sc.h rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0011   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.avgu.sci.h rD, rs1, Imm6**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0011   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.avgu.b rD, rs1, rs2**       |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0011   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.avgu.sc.b rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0011   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.avgu.sci.b rD, rs1, Imm6**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0100   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.min.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0100   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.min.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0100   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.min.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0100   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.min.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0100   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.min.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0100   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.min.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0101   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.minu.h rD, rs1, rs2**       |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0101   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.minu.sc.h rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0101   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.minu.sci.h rD, rs1, Imm6**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0101   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.minu.b rD, rs1, rs2**       |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0101   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.minu.sc.b rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0101   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.minu.sci.b rD, rs1, Imm6**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0110   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.max.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0110   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.max.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0110   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.max.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0110   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.max.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0110   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.max.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0110   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.max.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0111   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.maxu.h rD, rs1, rs2**       |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0111   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.maxu.sc.h rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0111   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.maxu.sci.h rD, rs1, Imm6**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0111   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.maxu.b rD, rs1, rs2**       |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0111   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.maxu.sc.b rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 0111   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.maxu.sci.b rD, rs1, Imm6**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1000   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.srl.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1000   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.srl.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1000   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.srl.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1000   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.srl.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1000   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.srl.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1000   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.srl.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1001   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.sra.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1001   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.sra.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1001   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.sra.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1001   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.sra.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1001   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.sra.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1001   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.sra.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.sll.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.sll.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.sll.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.sll.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.sll.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.sll.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.or.h rD, rs1, rs2**         |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.or.sc.h rD, rs1, rs2**      |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.or.sci.h rD, rs1, Imm6**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.or.b rD, rs1, rs2**         |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.or.sc.b rD, rs1, rs2**      |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.or.sci.b rD, rs1, Imm6**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.xor.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.xor.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.xor.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.xor.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.xor.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1100   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.xor.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.and.h rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.and.sc.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.and.sci.h rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.and.b rD, rs1, rs2**        |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.and.sc.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.and.sci.b rD, rs1, Imm6**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1110   | 0   | 0  | xxxxx   | src1    | 000    | dest     | 101 0111 | **pv.abs.h rD, rs1**             |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1110   | 0   | 0  | xxxxx   | src1    | 001    | dest     | 101 0111 | **pv.abs.b rD, rs1**             |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 1   | x  | xxxxx   | src1    | 000    | dest     | 101 0111 | **pv.cplxconj rD, rs1**          |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1111   | 0   | Imm6[5:0]    | src1    | 110    | dest     | 101 0111 | **pv.extract.h rD, Imm6**        |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+
| 0 1111   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.extract.b rD, Imm6**        |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+
| 1 0010   | 0   | Imm6[5:0]    | src1    | 110    | dest     | 101 0111 | **pv.extractu.h rD, Imm6**       |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+
| 1 0010   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.extractu.b rD, Imm6**       |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+
| 1 0110   | 0   | Imm6[5:0]    | src1    | 110    | dest     | 101 0111 | **pv.insert.h rD, Imm6**         |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+
| 1 0110   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.insert.b rD, Imm6**         |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0000   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.dotup.h rD, rs1, rs2**      |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0000   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.dotup.sc.h rD, rs1, rs2**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0000   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.dotup.sci.h rD, rs1, Imm6** |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0000   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.dotup.b rD, rs1, rs2**      |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0000   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.dotup.sc.b rD, rs1, rs2**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0000   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.dotup.sci.b rD, rs1, Imm6** |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0001   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.dotusp.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0001   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.dotusp.sc.h rD, rs1, rs2**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0001   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 |   pv.dotusp.sci.h rD, rs1, Imm6  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0001   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.dotusp.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0001   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.dotusp.sc.b rD, rs1, rs2**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0001   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 |   pv.dotusp.sci.b rD, rs1, Imm6  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0011   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.dotsp.h rD, rs1, rs2**      |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0011   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.dotsp.sc.h rD, rs1, rs2**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0011   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 | **pv.dotsp.sci.h rD, rs1, Imm6** |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0011   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.dotsp.b rD, rs1, rs2**      |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0011   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.dotsp.sc.b rD, rs1, rs2**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0011   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | **pv.dotsp.sci.b rD, rs1, Imm6** |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0100   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.sdotup.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0100   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.sdotup.sc.h rD, rs1, rs2**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0100   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 |   pv.sdotup.sci.h rD, rs1, Imm6  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0100   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.sdotup.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0100   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.sdotup.sc.b rD, rs1, rs2**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0100   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 |   pv.sdotup.sci.b rD, rs1, Imm6  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0101   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.sdotusp.h rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0101   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.sdotusp.sc.h rD, rs1, rs2** |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0101   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 |   pv.sdotusp.sci.h rD, rs1, Imm6 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0101   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.sdotusp.b rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0101   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.sdotusp.sc.b rD, rs1, rs2** |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0101   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 |   pv.sdotusp.sci.b rD, rs1, Imm6 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0111   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.sdotsp.h rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0111   | 0   | 0  | src2    | src1    | 100    | dest     | 101 0111 | **pv.sdotsp.sc.h rD, rs1, rs2**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0111   | 0   | Imm6[5:0]s   | src1    | 110    | dest     | 101 0111 |   pv.sdotsp.sci.h rD, rs1, Imm6  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0111   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.sdotsp.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0111   | 0   | 0  | src2    | src1    | 101    | dest     | 101 0111 | **pv.sdotsp.sc.b rD, rs1, rs2**  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 0111   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 |   pv.sdotsp.sci.b rD, rs1, Imm6  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1000   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.shuffle.h rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1000   | 0   | Imm6[5:0]    | src1    | 110    | dest     | 101 0111 |   pv.shuffle.sci.h rD, rs1, Imm6 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1000   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.shuffle.b rD, rs1, rs2**    |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1000   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | pv.shuffleI0.sci.b rD, rs1, Imm6 |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+
| 1 1101   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | pv.shuffleI1.sci.b rD, rs1, Imm6 |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+
| 1 1110   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | pv.shuffleI2.sci.b rD, rs1, Imm6 |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+
| 1 1111   | 0   | Imm6[5:0]    | src1    | 111    | dest     | 101 0111 | pv.shuffleI3.sci.b rD, rs1, Imm6 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1001   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.shuffle2.h rD, rs1, rs2**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1001   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.shuffle2.b rD, rs1, rs2**   |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1010   | 0   | 0  | src2    | src1    | 000    | dest     | 101 0111 | **pv.pack rD, rs1, rs2**         |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1010   | 0   | 1  | src2    | src1    | 000    | dest     | 101 0111 | **pv.pack.h rD, rs1, rs2**       |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1011   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.packhi.b rD, rs1, rs2**     |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 1 1100   | 0   | 0  | src2    | src1    | 001    | dest     | 101 0111 | **pv.packlo.b rD, rs1, rs2**     |
+----------+-----+--------------+---------+--------+----------+----------+----------------------------------+

**Note:** Imm6[5:0] is encoded as { Imm6[0], Imm6[5:1] }, LSB at the 25th bit of the instruction


SIMD Comparison Operations
^^^^^^^^^^^^^^^^^^^^^^^^^^

SIMD comparisons are done on individual bytes (.b) or half-words
(.h), depending on the chosen mode. If the comparison result is true,
all bits in the corresponding byte/half-word are set to 1. If the
comparison result is false, all bits are set to 0.

The default mode (no .sc, .sci) compares the lowest byte/half-word of
the first operand with the lowest byte/half-word of the second operand,
and so on. If the mode is set to scalar replication (.sc), always the
lowest byte/half-word of the second operand is used for comparisons,
thus instead of a vector comparison a scalar comparison is performed. In
the immediate scalar replication mode (.sci), the immediate given to the
instruction is used for the comparison.

+----------------------------------+----------------------------+-----------------------------------+
| **Mnemonic**                     |                            | **Description**                   |
+==================================+============================+===================================+
| **pv.cmpeq[.sc,.sci]{.h,.b}**    | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] == op2 ? ‘1 : ‘0   |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmpne[.sc,.sci]{.h,.b}**    | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] != op2 ? ‘1 : ‘0   |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmpgt[.sc,.sci]{.h,.b}**    | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] > op2 ? ‘1 : ‘0    |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmpge[.sc,.sci]{.h,.b}**    | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] >=op2 ? ‘1 : ‘0    |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmplt[.sc,.sci]{.h,.b}**    | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] < op2 ? ‘1 : ‘0    |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmple[.sc,.sci]{.h,.b}**    | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] <= op2 ? ‘1 : ‘0   |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmpgtu[.sc,.sci]{.h,.b}**   | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] > op2 ? ‘1 : ‘0    |
|                                  |                            | Note: Unsigned comparison         |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmpgeu[.sc,.sci]{.h,.b}**   | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] >= op2 ? ‘1 : ‘0   |
|                                  |                            | Note: Unsigned comparison         |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmpltu[.sc,.sci]{.h,.b}**   | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] < op2 ? ‘1 : ‘0    |
|                                  |                            | Note: Unsigned comparison         |
+----------------------------------+----------------------------+-----------------------------------+
| **pv.cmpleu[.sc,.sci]{.h,.b}**   | **rD, rs1, {rs2, Imm6}**   | rD[i] = rs1[i] <= op2 ? ‘1 : ‘0   |
|                                  |                            | Note: Unsigned comparison         |
+----------------------------------+----------------------------+-----------------------------------+

SIMD Comparison Encoding
^^^^^^^^^^^^^^^^^^^^^^^^
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 31 :  27 | 26 | 25 | 24   :   20 | 19 : 15  | 14 : 12 | 11  :  7 | 6  :    0  |                                   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| funct5   | F  |    | rs2         | rs1      | funct3  | rD       | opcode     |                                   |
+==========+====+====+=============+==========+=========+==========+============+===================================+
| 0 0000   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmpeq.h rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0000   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmpeq.sc.h rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0000   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmpeq.sci.h rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0000   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmpeq.b rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0000   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmpeq.sc.b rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0000   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmpeq.sci.b rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0001   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmpne.h rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0001   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmpne.sc.h rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0001   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmpne.sci.h rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0001   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmpne.b rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0001   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmpne.sc.b rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0001   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmpne.sci.b rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0010   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmpgt.h rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0010   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmpgt.sc.h rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0010   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmpgt.sci.h rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0010   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmpgt.b rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0010   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmpgt.sc.b rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0010   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmpgt.sci.b rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0011   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmpge.h rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0011   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmpge.sc.h rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0011   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmpge.sci.h rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0011   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmpge.b rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0011   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmpge.sc.b rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0011   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmpge.sci.b rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0100   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmplt.h rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0100   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmplt.sc.h rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0100   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmplt.sci.h rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0100   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmplt.b rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0100   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmplt.sc.b rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0100   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmplt.sci.b rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0101   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmple.h rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0101   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmple.sc.h rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0101   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmple.sci.h rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0101   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmple.b rD, rs1, rs2**       |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0101   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmple.sc.b rD, rs1, rs2**    |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0101   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmple.sci.b rD, rs1, Imm6**  |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0110   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmpgtu.h rD, rs1, rs2**      |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0110   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmpgtu.sc.h rD, rs1, rs2**   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0110   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmpgtu.sci.h rD, rs1, Imm6** |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0110   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmpgtu.b rD, rs1, rs2**      |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0110   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmpgtu.sc.b rD, rs1, rs2**   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0110   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmpgtu.sci.b rD, rs1, Imm6** |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0111   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmpgeu.h rD, rs1, rs2**      |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0111   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmpgeu.sc.h rD, rs1, rs2**   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0111   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmpgeu.sci.h rD, rs1, Imm6** |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0111   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmpgeu.b rD, rs1, rs2**      |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0111   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmpgeu.sc.b rD, rs1, rs2**   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 0111   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmpgeu.sci.b rD, rs1, Imm6** |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1000   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmpltu.h rD, rs1, rs2**      |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1000   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmpltu.sc.h rD, rs1, rs2**   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1000   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmpltu.sci.h rD, rs1, Imm6** |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1000   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmpltu.b rD, rs1, rs2**      |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1000   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmpltu.sc.b rD, rs1, rs2**   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1000   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmpltu.sci.b rD, rs1, Imm6** |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1001   | 1  | 0  | src2        | src1     | 000     | dest     | 101 0111   | **pv.cmpleu.h rD, rs1, rs2**      |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1001   | 1  | 0  | src2        | src1     | 100     | dest     | 101 0111   | **pv.cmpleu.sc.h rD, rs1, rs2**   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1001   | 1  | Imm6[5:0]        | src1     | 110     | dest     | 101 0111   | **pv.cmpleu.sci.h rD, rs1, Imm6** |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1001   | 1  | 0  | src2        | src1     | 001     | dest     | 101 0111   | **pv.cmpleu.b rD, rs1, rs2**      |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1001   | 1  | 0  | src2        | src1     | 101     | dest     | 101 0111   | **pv.cmpleu.sc.b rD, rs1, rs2**   |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+
| 0 1001   | 1  | Imm6[5:0]        | src1     | 111     | dest     | 101 0111   | **pv.cmpleu.sci.b rD, rs1, Imm6** |
+----------+----+----+-------------+----------+---------+----------+------------+-----------------------------------+

**Note:** Imm6[5:0] is encoded as { Imm6[0], Imm6[5:1] }, LSB at the 25th bit of the instruction

SIMD Complex-number Operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

SIMD Complex-number operations are extra instructions
that uses the packed-SIMD extentions to represent Complex-numbers.
These extentions use only the half-words mode and only operand in registers.
A number C = {Re, Im} is represented as a vector of two 16-Bits signed numbers.
C[0] is the real part [15:0], C[1] is the
imaginary part [31:16].
Such operations are subtraction of 2 complexes with post rotation by -j, the complex and conjugate,
and Complex multiplications.
The complex multiplications are performed in two separate instructions, one to compute the real part,
and one to compute the imaginary part.


As for all the other SIMD instructions, no flags are raised and CSR register are unmodified.
No carry, overflow is generated. Instructions are rounded up as the mask & 0xFFFF explicits.

+---------------------------------------+---------------------------------------------------------------------------------------+
| **Mnemonic**                          | **Description**                                                                       |
+=======================================+=======================================================================================+
| **pv.subrotmj{/,div2,div4,div8}**     | rD[0] = ((rs1[1] – rs2[1]) & 0xFFFF)>>{0,1,2,3}                                       |
|                                       |                                                                                       |
|                                       | rD[1] = ((rs2[0] – rs1[0]) & 0xFFFF)>>{0,1,2,3}                                       |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.cplxconj**                       | rD[0] = rs1[0]                                                                        |
|                                       |                                                                                       |
|                                       | rD[1] = -rs1[1]                                                                       |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.cplxmul.r.{/,div2,div4,div8}**   | rD[15:0 ] = (rs1[0]\*rs2[0] – rs1[1]\*rs2[1])>>{15,16,17,18}                          |
|                                       |                                                                                       |
|                                       | rD[31:16] = rD[31:16]                                                                 |
+---------------------------------------+---------------------------------------------------------------------------------------+
| **pv.cplxmul.i.{/,div2,div4,div8}**   | rD[31:16] = (rs1[0]\*rs2[1] + rs1[1]\*rs2[0])>>{15,16,17,18}                          |
|                                       |                                                                                       |
|                                       | rD[15:0 ] = rD[15:0 ]                                                                 |
+---------------------------------------+---------------------------------------------------------------------------------------+

SIMD Complex-numbers Encoding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 31  : 27 | 26  | 25 | 24 : 20 | 19 : 15 | 14 :12 | 11  :  7 | 6   :  0 |                                  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| funct5   | F   |    | rs2     | rs1     | funct3 | rD       | opcode   |                                  |
+==========+=====+====+=========+=========+========+==========+==========+==================================+
| 0 1101   | 1   | x  | src2    | src1    | 00x    | dest     | 101 0111 |   pv.subrotmj rD, rs1, rs2       |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 1   | x  | src2    | src1    | 01x    | dest     | 101 0111 |   pv.subrotmj.div2 rD, rs1, rs2  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 1   | x  | src2    | src1    | 10x    | dest     | 101 0111 |   pv.subrotmj.div4 rD, rs1, rs2  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1101   | 1   | x  | src2    | src1    | 11x    | dest     | 101 0111 |   pv.subrotmj.div8 rD, rs1, rs2  |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1011   | 1   | x  | xxxxx   | src1    | 000    | dest     | 101 0111 |   pv.cplxconj rD, rs1            |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 1   | 0  | src2    | src1    | 00x    | dest     | 101 0111 |   pv.cplxmul.r rD, rs1, rs2      |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 1   | 0  | src2    | src1    | 01x    | dest     | 101 0111 |   pv.cplxmul.r.div2 rD, rs1, rs2 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 1   | 0  | src2    | src1    | 10x    | dest     | 101 0111 |   pv.cplxmul.r.div4 rD, rs1, rs2 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 1   | 0  | src2    | src1    | 11x    | dest     | 101 0111 |   pv.cplxmul.r.div8 rD, rs1, rs2 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 1   | 1  | src2    | src1    | 00x    | dest     | 101 0111 |   pv.cplxmul.i rD, rs1, rs2      |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 1   | 1  | src2    | src1    | 01x    | dest     | 101 0111 |   pv.cplxmul.i.div2 rD, rs1, rs2 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 1   | 1  | src2    | src1    | 10x    | dest     | 101 0111 |   pv.cplxmul.i.div4 rD, rs1, rs2 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
| 0 1010   | 1   | 1  | src2    | src1    | 11x    | dest     | 101 0111 |   pv.cplxmul.i.div8 rD, rs1, rs2 |
+----------+-----+----+---------+---------+--------+----------+----------+----------------------------------+
