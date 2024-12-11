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
    ARegister(in= ACoutput, load= instruction[5], out= ARout, out[0..14]= addressM);

    //select for comp A/M
    Mux16(a= ARout, b= inM, sel= instruction[12], out= AMout);
    
    //select D Register
    DRegister(in= outforD, load= instruction[4], out= DRout);

    //ALU action
    ALU(x= DRout, y= AMout, zx= instruction[11], nx= instruction[10],
                            zy= instruction[9], ny= instruction[8],
                            f= instruction[7], no= instruction[6],
                            out= outM, out = outforD, out= outforA,
                            zr= zr, ng= ng);
    
    //Write of ALU output
    //writeM
    And(a= instruction[15], b= instruction[3], out= writeM);

    //writeD
    And(a= instruction[15], b= instruction[4], out= writeinD);
    DRegister(in= outforD, load= writeinD, out= Dvalue);


    //series of Jump (2, 1, 0) & C-instruction
    Not(in= zr, out= notzr);
    Not(in= ng, out= notng);

    //1. JGT
    And(a= instruction[15], b= instruction[0], out= JGTins);
    And(a= notzr, b= notng, out= JGTALU);
    And(a= JGTALU, b= JGTins, out= dJGT);

    //2. JEQ
    And(a= instruction[15], b= instruction[1], out= JEQins);
    And(a= zr, b= JEQins, out= dJEQ);

    //3. JGE
    And(a= instruction[1], b= instruction[0], out= JGEpre);
    And(a= instruction[15], b= JGEpre, out= JGEins);
    And(a= notng, b= JGEins, out= dJGE);

    //4. JLT
    And(a= instruction[15], b= instruction[2], out= JLTins);
    And(a= ng, b= JLTins, out= dJLT);

    //5. JNE
    And(a= instruction[2], b= instruction[0], out= JNEpre);
    And(a= instruction[15], b= JNEpre, out= JNEins);
    And(a= notzr, b= JNEins, out= dJNE);

    //6. JLE
    And(a= instruction[2], b= instruction[1], out= JLEpre);
    And(a= instruction[15], b=JLEpre, out= JLEins);
    Not(in= JGTALU, out= notJGT);
    And(a= notJGT, b= JLEins, out= dJLE);

    //7. JMP
    And(a= instruction[1], b= instruction[0], out= JMPpre1);
    And(a= instruction[15], b= instruction[2], out= JMPpre2);
    And(a= JMPpre1, b= JMPpre2, out= dJMP);

    //sum-up jump condition into JUMP
    Or(a= dJGT, b= dJEQ, out= j1);
    Or(a= dJGE, b= dJLT, out= j2);
    Or(a= dJNE, b= dJLE, out= j3);
    Or(a= dJMP, b= j1, out= j4);
    Or(a= j2, b= j3, out= j5);
    Or(a= j4, b= j5, out= jumpdecision);

    //PC
    PC(in= ARout, load= jumpdecision, inc= true,reset= reset, out[0..14]= pc);
    

}
```
