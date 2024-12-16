### Final note
#### Nand2Tetris

##### Project 1 - Boolean Logic
- 1. Nand Chip : All other gate is built from Nand chip 
  2. Gate Logics :
     ```
     CHIP Not {
       IN in;
       OUT out;
       PARTS: 
       Nand(a= in, b= in, out = out);
     }

     CHIP And {
       IN a, b;
       OUT out;

       PARTS:
       Nand(a= a, b= b, out= aNandb);
       Not(in= aNandb, out= out);
     }

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
     



##### Project 2 - Boolean Arithmetic
##### Project 3 - Sequential Logic
##### Project 4 - Machine Language
##### Project 5 - Computer Architecture
##### Project 6 - Assembler
