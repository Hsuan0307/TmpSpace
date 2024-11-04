HDL_Program 1 

1. Not
```
CHIP Not {
  IN in;
  OUT out;
  PARTS: 
  Nand(a= in, b= in, out = out);
}
```


3. And
```
CHIP And {
    IN a, b;
    OUT out;
    
    PARTS:
    Nand(a= a, b= b, out= aNandb);
    Not(in= aNandb, out= out);
}
```
   
3. Or
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
7. DMux

8. 
9. Not16
10. And16
11. Or16
12. Mux16
13. Or8way
14. Mux4Way16
15. Mux8Way16
16. DMux4Way
17. DMux8Way
