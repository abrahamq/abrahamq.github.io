---
layout: single
title: "Counting instructions with Intel's PIN tool"
date: 2015-10-09
---

In 6.823, my computer architecture class we're using [PIN](https://software.intel.com/en-us/articles/pin-a-dynamic-binary-instrumentation-tool), which is a program made by Intel that lets you insert your own instructions into a target executable.

I found the documentation super lacking, so I'm going to write up some tutorials on how to use it for my own reference. PIN can be super powerful, but it is also pretty complex.

## Instrumentation Routine

At the core of PIN is the instrumentation function- this function is called every time that PIN encounters a new instruction. Here is where you can tell PIN whether whether to inject or not and also what instructions you would like to inject. Here's an example of this:

```c
VOID Instruction(INS ins, VOID *v) {
    if (INS_IsMemoryRead(ins)) {
        INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR)CountRead, IARG_INST_PTR, IARG_END);
    }
}
```

The Analysis Routine is a function that can be injected by the Instrumentation routine. PIN generally tries to inline these in order to make execution go faster.

```cpp
VOID docount() {
icount++;
}
```

Here's what the complete code looks like (from the Intel examples, which are released under GPL):

```cpp
#include <iostream>
#include <fstream>
#include "pin.H"
ofstream OutFile;
// The running count of instructions is kept here
// make it static to help the compiler optimize docount
static UINT64 icount = 0;
// This function is called before every instruction is executed
VOID docount() { icount++; }
// Pin calls this function every time a new instruction is encountered
VOID Instruction(INS ins, VOID v)
{
// Insert a call to docount before every instruction, no arguments are passed
INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR)docount, IARG_END);
}
KNOB<string> KnobOutputFile(KNOB_MODE_WRITEONCE, "pintool",
"o", "inscount.out", "specify output file name");
// This function is called when the application exits
VOID docount() { icount++; }
// Pin calls this function every time a new instruction is encountered
VOID Instruction(INS ins, VOID v)
{
// Insert a call to docount before every instruction, no arguments are passed
INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR)docount, IARG_END);
}
KNOB<string> KnobOutputFile(KNOB_MODE_WRITEONCE, "pintool",
"o", "inscount.out", "specify output file name");
// This function is called when the application exits
VOID Fini(INT32 code, VOID v)
{
// Write to a file since cout and cerr maybe closed by the application
OutFile.setf(ios::showbase);
OutFile << "Count " << icount << endl;
OutFile.close();
}
/ ===================================================================== /
/ Print Help Message /
/ ===================================================================== /
INT32 Usage()
{
cerr << "This tool counts the number of dynamic instructions executed" << endl;
cerr << endl << KNOB_BASE::StringKnobSummary() << endl;
return -1;
}
/ ===================================================================== /
/ Main /
/ ===================================================================== /
/ argc, argv are the entire command line: pin -t <toolname> -- ... /
/
===================================================================== /
int main(int argc, char argv[])
{
// Initialize pin
if (PIN_Init(argc, argv)) return Usage();
OutFile.open(KnobOutputFile.Value().c_str());
// Register Instruction to be called to instrument instructions
INS_AddInstrumentFunction(Instruction, 0);
// Register Fini to be called when the application exits
PIN_AddFiniFunction(Fini, 0);
// Start the program, never returns
PIN_StartProgram();
return 0;
}
```
