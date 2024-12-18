HDL_Project 1 

1. Not
```
CHIP Not {
  IN in;
  OUT out;
  PARTS: 
  Nand(a= in, b= in, out = out);
}
```


2. And
```
CHIP And {
    IN a, b;
    OUT out;
    
    PARTS:
    Nand(a= a, b= b, out= aNandb);
    Not(in= aNandb, out= out);
}
```
   
3. Or (只有a b同時為false才為false -> True的logic = Not(Nota and Notb)  
       = (a' and b')' )
```
CHIP Or {
    IN a, b;
    OUT out;

    PARTS:
    //A' & B'
    Nand(a= a, b= a, out= Nota);
    Nand(a= b, b= b, out= Notb);

    //A' and B'
    Nand(a= Nota, b= Notb, out= NotaNandNotb);
    Nand(a= NotaNandNotb, b= NotaNandNotb, out= NotAbAndNotBb);

    //(A' and B')'
    Nand(a= NotAbAndNotBb, b= NotAbAndNotBb, out= out);
}
```

4. Xor
```
CHIP Xor {
    IN a, b;
    OUT out;

    PARTS:
    //a Nand b
    Nand(a= a, b= b, out= aNandb);

    //a Nand aNandb
    Nand(a= a, b= aNandb, out= aNandaNandb);

    //b Nand aNandb
    Nand(a= b, b= aNandb, out= bNandaNandb);

    //aNandaNandb Nand bNandaNandb
    Nand(a= aNandaNandb, b= bNandaNandb, out= out);
}
```

5. Mux
```
CHIP Mux {
    IN a, b, sel;
    OUT out;

    PARTS:
    //Not sel
    Nand(a= sel, b= sel, out= Notsel);

    //Notsel Nand a
    Nand(a= Notsel, b= a, out= NotselNanda);

    //sel Nand b
    Nand(a=sel ,b= b, out= selNandb);

    //out
    Nand(a= NotselNanda, b= selNandb, out= out);
}
```
6. DMux
```
CHIP DMux {
    IN in, sel;
    OUT a, b;

    PARTS:
    ////B
    //in Nand sel
    Nand(a= in, b= sel, out= inNandsel);
    //Not inNandsel
    Nand(a= inNandsel, b= inNandsel, out= b);

    ////A
    //Notsel
    Nand(a= sel, b= sel, out= Notsel);
    //in Nand Notsel
    Nand(a= in, b= Notsel, out= inNandNotsel);
    //inverse
    Nand(a= inNandNotsel, b= inNandNotsel, out= a);
}
```

7. Not16
```
CHIP Not16 {
    IN in[16];
    OUT out[16];

    PARTS:
    Nand(a=in[0], b=in[0], out=out[0]);
    Nand(a=in[1], b=in[1], out=out[1]);
    Nand(a=in[2], b=in[2], out=out[2]);
    Nand(a=in[3], b=in[3], out=out[3]);
    Nand(a=in[4], b=in[4], out=out[4]);
    Nand(a=in[5], b=in[5], out=out[5]);
    Nand(a=in[6], b=in[6], out=out[6]);
    Nand(a=in[7], b=in[7], out=out[7]);
    Nand(a=in[8], b=in[8], out=out[8]);
    Nand(a=in[9], b=in[9], out=out[9]);
    Nand(a=in[10], b=in[10], out=out[10]);
    Nand(a=in[11], b=in[11], out=out[11]);
    Nand(a=in[12], b=in[12], out=out[12]);
    Nand(a=in[13], b=in[13], out=out[13]);
    Nand(a=in[14], b=in[14], out=out[14]);
    Nand(a=in[15], b=in[15], out=out[15]);
}
```

8. And16
```
CHIP And16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
    And(a=a[0], b=b[0], out=out[0]);
    And(a=a[1], b=b[1], out=out[1]);
    And(a=a[2], b=b[2], out=out[2]);
    And(a=a[3], b=b[3], out=out[3]);
    And(a=a[4], b=b[4], out=out[4]);
    And(a=a[5], b=b[5], out=out[5]);
    And(a=a[6], b=b[6], out=out[6]);
    And(a=a[7], b=b[7], out=out[7]);
    And(a=a[8], b=b[8], out=out[8]);
    And(a=a[9], b=b[9], out=out[9]);
    And(a=a[10], b=b[10], out=out[10]);
    And(a=a[11], b=b[11], out=out[11]);
    And(a=a[12], b=b[12], out=out[12]);
    And(a=a[13], b=b[13], out=out[13]);
    And(a=a[14], b=b[14], out=out[14]);
    And(a=a[15], b=b[15], out=out[15]);
}
```

9. Or16
```
CHIP Or16 {
    IN a[16], b[16];
    OUT out[16];
    PARTS:
    Not16(in= a, out= tmpa);
    Not16(in= b, out= tmpb);
    And16(a= tmpa, b= tmpb, out= tmpab);
    Not16(in= tmpab, out= out);
}
```
10.Mux16
```
CHIP Mux16 {
    IN a[16], b[16], sel;
    OUT out[16];

    PARTS:
    Mux(a=a[0], b=b[0],sel= sel, out=out[0]);
    Mux(a=a[1], b=b[1],sel= sel, out=out[1]);
    Mux(a=a[2], b=b[2],sel= sel, out=out[2]);
    Mux(a=a[3], b=b[3],sel= sel, out=out[3]);
    Mux(a=a[4], b=b[4],sel= sel, out=out[4]);
    Mux(a=a[5], b=b[5],sel= sel, out=out[5]);
    Mux(a=a[6], b=b[6],sel= sel, out=out[6]);
    Mux(a=a[7], b=b[7],sel= sel, out=out[7]);
    Mux(a=a[8], b=b[8],sel= sel, out=out[8]);
    Mux(a=a[9], b=b[9],sel= sel, out=out[9]);
    Mux(a=a[10], b=b[10],sel= sel, out=out[10]);
    Mux(a=a[11], b=b[11],sel= sel, out=out[11]);
    Mux(a=a[12], b=b[12],sel= sel, out=out[12]);
    Mux(a=a[13], b=b[13],sel= sel, out=out[13]);
    Mux(a=a[14], b=b[14],sel= sel, out=out[14]);
    Mux(a=a[15], b=b[15],sel= sel, out=out[15]);
}
```

11. Or8way
```
CHIP Or8Way {
    IN in[8];
    OUT out;

    PARTS:
    Or(a= in[0], b= in[1], out= t1);
    Or(a=in[2], b= t1, out= t2);
    Or(a=in[3], b= t2, out= t3);
    Or(a=in[4], b= t3, out= t4);
    Or(a=in[5], b= t4, out= t5);
    Or(a=in[6], b= t5, out= t6);
    Or(a=in[7], b= t6, out= out);
}
```

12. Mux4Way16
```
CHIP Mux4Way16 {
    IN a[16], b[16], c[16], d[16], sel[2];
    OUT out[16];
    
    PARTS:
    //// ab group by sel[0]
    Mux16(a= a, b= b, sel= sel[0], out= ab);
    //// cd groupby sel[0]
    Mux16(a= c, b= d, sel= sel[0], out= cd);

    ////ab cd group by sel[1]
    Mux16(a= ab, b= cd, sel= sel[1], out= out); 
}
```

13. Mux8Way16
```
CHIP Mux8Way16 {
    IN a[16], b[16], c[16], d[16],
       e[16], f[16], g[16], h[16],
       sel[3];
    OUT out[16];

    PARTS:
    ////sel[0..1]
    //abcd
    Mux4Way16(a= a, b= b, c= c, d= d, sel= sel[0..1], out= abcd);
    //efgh
    Mux4Way16(a= e, b= f, c= g, d= h, sel= sel[0..1], out= efgh);

    ////sel[2]
    Mux16(a= abcd, b= efgh, sel= sel[2], out= out); 
}
```

14. DMux4Way
```
CHIP DMux4Way {
    IN in, sel[2];
    OUT a, b, c, d;

    PARTS:
    ////sel[1]
    DMux(in= in, sel= sel[1], a= ab, b= cd);

    ////sel[0]
    DMux(in= ab, sel= sel[0], a= a, b= b);
    DMux(in= cd, sel= sel[0], a= c, b= d);
}
```

15. DMux8Way
```
CHIP DMux8Way {
    IN in, sel[3];
    OUT a, b, c, d, e, f, g, h;

    PARTS:
    //// sel[2]
    //abcd efgh
    DMux(in= in, sel= sel[2], a= abcd, b= efgh);

    ////sel[0..1]
    DMux4Way(in= abcd, sel= sel[0..1], a= a, b= b, c= c, d= d);
    DMux4Way(in= efgh, sel= sel[0..1], a= e, b= f, c= g, d= h);
}
```
