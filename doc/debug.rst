.. _debug-unit:

Debug Unit
==========


Address Map
-----------

+--------------+-----------------+--------------------------------------------------+
| Address      | Name            | Description                                      |
+==============+=================+==================================================+
| **0x0000** - | Debug Registers | Always accessible, even when the core is running |
| **0x007F**   |                 |                                                  |
+--------------+-----------------+--------------------------------------------------+
| **0x400** -  | GPR (x0-x31)    | General Purpose Registers. Only accessible if    |
| **0x47F**    |                 | the core is halted                               |
+--------------+-----------------+--------------------------------------------------+
| **0x500** -  | FPR (f0-f31)    | Reserved. Not used in the ZERO-RISCY core.       |
| **0x5FF**    |                 |                                                  |
+--------------+-----------------+--------------------------------------------------+
| **0x2000** - | Debug Registers | Only accessible if the core is halted            |
| **0x20FF**   |                 |                                                  |
+--------------+-----------------+--------------------------------------------------+
| **0x4000** - | CSR             | Control and Status Registers                     |
| **0x7FFF**   |                 | Only accessible if the core is halted            |
+--------------+-----------------+--------------------------------------------------+

Addresses are intended for a bus system with 32-bit wide words.
FPR get more address space than GPR because they can be 64-bit wide even in a 32-bit system.
Addresses have to be aligned to word-boundaries.


Debug Registers
---------------

+--------------+-----------------+--------------------------------------------------+
| Address      | Name            | Description                                      |
+==============+=================+==================================================+
| **0x00**     | DBG_CTRL        | Debug Control                                    |
+--------------+-----------------+--------------------------------------------------+
| **0x04**     | DBG_HIT         | Debug Hit                                        |
+--------------+-----------------+--------------------------------------------------+
| **0x08**     | DBG_IE          | Debug Interrupt Enable                           |
+--------------+-----------------+--------------------------------------------------+
| **0x0C**     | DBG_CAUSE       | Debug Cause (Why we entered debug state)         |
+--------------+-----------------+--------------------------------------------------+
| **0x40**     | DBG_BPCTRL0     | HW BP0 Control                                   |
+--------------+-----------------+--------------------------------------------------+
| **0x44**     | DBG_BPDATA0     | HW BP0 Data                                      |
+--------------+-----------------+--------------------------------------------------+
| **0x48**     | DBG_BPCTRL1     | HW BP1 Control                                   |
+--------------+-----------------+--------------------------------------------------+
| **0x4C**     | DBG_BPDATA1     | HW BP1 Data                                      |
+--------------+-----------------+--------------------------------------------------+
| **0x50**     | DBG_BPCTRL2     | HW BP2 Control                                   |
+--------------+-----------------+--------------------------------------------------+
| **0x54**     | DBG_BPDATA2     | HW BP2 Data                                      |
+--------------+-----------------+--------------------------------------------------+
| **0x58**     | DBG_BPCTRL3     | HW BP3 Control                                   |
+--------------+-----------------+--------------------------------------------------+
| **0x5C**     | DBG_BPDATA3     | HW BP3 Data                                      |
+--------------+-----------------+--------------------------------------------------+
| **0x60**     | DBG_BPCTRL4     | HW BP4 Control                                   |
+--------------+-----------------+--------------------------------------------------+
| **0x64**     | DBG_BPDATA4     | HW BP4 Data                                      |
+--------------+-----------------+--------------------------------------------------+
| **0x68**     | DBG_BPCTRL5     | HW BP5 Control                                   |
+--------------+-----------------+--------------------------------------------------+
| **0x6C**     | DBG_BPDATA5     | HW BP5 Data                                      |
+--------------+-----------------+--------------------------------------------------+
| **0x70**     | DBG_BPCTRL6     | HW BP6 Control                                   |
+--------------+-----------------+--------------------------------------------------+
| **0x74**     | DBG_BPDATA6     | HW BP6 Data                                      |
+--------------+-----------------+--------------------------------------------------+
| **0x78**     | DBG_BPCTRL7     | HW BP7 Control                                   |
+--------------+-----------------+--------------------------------------------------+
| **0x7C**     | DBG_BPDATA7     | HW BP7 Data                                      |
+--------------+-----------------+--------------------------------------------------+
| **0x2000**   | DBG_NPC         | Next PC                                          |
+--------------+-----------------+--------------------------------------------------+
| **0x2004**   | DBG_PPC         | Previous PC                                      |
+--------------+-----------------+--------------------------------------------------+


Debug Control (DBG_CTRL)
------------------------

+-------+-----+------------------------------------------------------------------+
| Bit#  | R/W | Description                                                      |
+=======+=====+==================================================================+
| 16    | W1  | **HALT:** When 1 written, core enters debug mode, when 0         |
|       |     | written, core exits debug mode.                                  |
|       |     | When read, 1 means core is in debug mode                         |
+-------+-----+------------------------------------------------------------------+
| 0     | R/W | **SSTE:** Single-step enable                                     |
+-------+-----+------------------------------------------------------------------+


Debug Hit (DBG_HIT)
-------------------

+-------+-----+------------------------------------------------------------------+
| Bit#  | R/W | Description                                                      |
+=======+=====+==================================================================+
| 16    | R   | **SLEEP:** Set when the core is in a sleeping state and waits    |
|       |     | for an event                                                     |
+-------+-----+------------------------------------------------------------------+
| 0     | R/W | **SSTH:** Single-step hit, sticky bit that must be cleared by    |
|       |     | external debugger                                                |
+-------+-----+------------------------------------------------------------------+


Debug Interrupt Enable (DBG_IE)
-------------------------------

+-------+-----+------------------------------------------------------------------+
| Bit#  | R/W | Description                                                      |
+=======+=====+==================================================================+
| 11    | R/W | **ECALL:** Environment call from M-Mode                          |
+-------+-----+------------------------------------------------------------------+
| 7     | R/W | **SAF:** Store Access Fault (together with **LAF**)              |
+-------+-----+------------------------------------------------------------------+
| 6     | R/W | **SAM:** Store Address Misaligned (never traps)                  |
+-------+-----+------------------------------------------------------------------+
| 5     | R/W | **LAF:** Load Access Fault (together with **SAF**)               |
+-------+-----+------------------------------------------------------------------+
| 4     | R/W | **LAM:** Load Address Misaligned (never traps)                   |
+-------+-----+------------------------------------------------------------------+
| 3     | R/W | **BP:** EBREAK instruction causes trap                           |
+-------+-----+------------------------------------------------------------------+
| 2     | R/W | **ILL:** Illegal Instruction                                     |
+-------+-----+------------------------------------------------------------------+
| 1     | R/W | **IAF:** Instruction Access Fault (not implemented)              |
+-------+-----+------------------------------------------------------------------+
| 0     | R/W | **IAM:** Instruction Address Misaligned (never traps)            |
+-------+-----+------------------------------------------------------------------+

When ‘1’ exceptions cause traps, otherwise normal exceptions.


Debug Cause (DBG_CAUSE)
-----------------------

+-------+-----+------------------------------------------------------------------+
| Bit#  | R/W | Description                                                      |
+=======+=====+==================================================================+
| 31    | R   | **IRQ:** Interrupt caused us to enter debug mode                 |
+-------+-----+------------------------------------------------------------------+
| 4:0   | R   | **CAUSE:** Exception/interrupt number                            |
+-------+-----+------------------------------------------------------------------+


Debug Hardware Breakpoint x Control (DBG_BPCTRLx)
-------------------------------------------------

+-------+-----+------------------------------------------------------------------+
| Bit#  | R/W | Description                                                      |
+=======+=====+==================================================================+
| 0     | R   | **IMPL:** ZERO-RISCY does not implement hardware breakpoints.    |
|       |     | Always read as 0.                                                |
+-------+-----+------------------------------------------------------------------+


Debug Next Program Counter (DBG_NPC)
------------------------------------

+-------+-----+------------------------------------------------------------------+
| Bit#  | R/W | Description                                                      |
+=======+=====+==================================================================+
| 31:0  | R/W | **NPC:** Next PC to be executed                                  |
+-------+-----+------------------------------------------------------------------+

When written core jumps to PC.


Debug Previous Program Counter (DBG_PPC)
----------------------------------------

+-------+-----+------------------------------------------------------------------+
| Bit#  | R/W | Description                                                      |
+=======+=====+==================================================================+
| 31:0  | W   | **PPC:** Previous PC, already executed                           |
+-------+-----+------------------------------------------------------------------+

Values of PPC and NPC when entering debug mode:

+---------------------+------------------------+------------------+---------+------------+
| Reason              | PPC                    | NPC              | Cause   | GDB Sigval |
+=====================+========================+==================+=========+============+
| ebreak              | ebreak instruction     | next instruction | BP      | TRAP       |
+---------------------+------------------------+------------------+---------+------------+
| ecall               | ecall instruction      | IVT entry        | ECALL   | TRAP       |
+---------------------+------------------------+------------------+---------+------------+
| illegal instruction | illegal instruction    | IVT entry        | ILL     | ILL        |
+---------------------+------------------------+------------------+---------+------------+
| invalid mem access  | load/store instruction | IVT entry        | LAF/SAF | SEGV       |
+---------------------+------------------------+------------------+---------+------------+
| interrupt           | last instruction       | IVT entry        | ?       | INT        |
+---------------------+------------------------+------------------+---------+------------+
| halt                | last instruction       | next instruction | ?       | TRAP       |
+---------------------+------------------------+------------------+---------+------------+
| single-step         | last instruction       | next instruction | ?       | TRAP       |
+---------------------+------------------------+------------------+---------+------------+


Control and Status Registers
----------------------------

+--------------+------------------+--------------------------------------------------+
| Address      | Name             | Description                                      |
+==============+==================+==================================================+
| 0x4000       | CSR 0 = 0x000    | CSR                                              |
+--------------+------------------+--------------------------------------------------+
| ...          | ...              | ...                                              |
+--------------+------------------+--------------------------------------------------+
| 0x7FFC       | CSR 4095 = 0xFFF | CSR                                              |
+--------------+------------------+--------------------------------------------------+

Can only be accessed when core is in debug mode.

Interface
---------

+-------------------------+-----------+----------------------------------------------+
| Signal                  | Direction | Description                                  |
+=========================+===========+==============================================+
| ``debug_req_i``         | input     | Request                                      |
+-------------------------+-----------+----------------------------------------------+
| ``debug_gnt_o``         | output    | Grant                                        |
+-------------------------+-----------+----------------------------------------------+
| ``debug_rvalid_o``      | output    | Read data valid                              |
+-------------------------+-----------+----------------------------------------------+
| ``debug_addr_i[14:0]``  | input     | Address for write/read                       |
+-------------------------+-----------+----------------------------------------------+
| ``debug_we_i``          | input     | Write Enable                                 |
+-------------------------+-----------+----------------------------------------------+
| ``debug_wdata_i[31:0]`` | input     | Write data                                   |
+-------------------------+-----------+----------------------------------------------+
| ``debug_rdata_o[31:0]`` | output    | Read data                                    |
+-------------------------+-----------+----------------------------------------------+
| ``debug_halted_o``      | output    | Is high when core is in debug mode           |
+-------------------------+-----------+----------------------------------------------+
| ``debug_halt_i``        | input     | Set high when core should enter debug mode   |
+-------------------------+-----------+----------------------------------------------+
| ``debug_resume_i``      | input     | Set high when core should exit debug mode    |
+-------------------------+-----------+----------------------------------------------+

``debug_halted_o``, ``debug_halt_i`` and ``debug_resume_i`` are intended for cross-triggering between multiple cores. They are not required for single-core debug, thus ``debug_halt_i`` and ``debug-resume_i`` can be tied to 0.

``debug_halt_i`` and ``debug_resume_i`` should be high for only one single cycle to avoid deadlock issues.
