#### Project 5

#### CPU.hdl

```

CHIP CPU {

    IN  inM[16],         // M value input  (M = contents of RAM[A])
        instruction[16], // Instruction for execution
        reset;           // Signals whether to re-start the current
                         // program (reset==1) or continue executing
                         // the current program (reset==0).

    OUT outM[16],        // M value output
        writeM,          // Write to M? 
        addressM[15],    // Address in data memory (of M)
        pc[15];          // address of next instruction

    PARTS:
	//// Replace this comment with your code.
    //decide A-instruction or C-instruction : instruction[15] = 0 then A else C

    //select for A/C
    Mux16(a= instruction, b= outforA, sel= instruction[15], out= ACoutput);
    ARegister(in= ACoutput, load= instruction[2], out= ARout);

    //select for comp A/M
    Mux(a= ARout, b= inM, sel= instruction[12], out= AMout);
    
    //select D Register
    DRegister(in= outforD, load= instruction[1], out= DRout);

    //ALU action
    ALU(x= DRout, y= AMout, zx= instruction[11], nx= instruction[10],
                            zy= instruction[9], ny= instruction[8],
                            f= instruction[7], no= instruction[6],
                            out = outforD, out= outforA, out= outM,
                            zr= zr, ng= ng);
    

}
```
