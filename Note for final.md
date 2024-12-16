### Final note
#### Nand2Tetris

---
#### Project 1 - Boolean Logic
 1. Nand Chip : All other gate is built from Nand chip
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
     


---
#### Project 2 - Boolean Arithmetic

---
#### Project 3 - Sequential Logic

##### Function of In/Out/Load/Clock
 1. **1-bit Register**
     ```
     if load(t) True: out(t+1) = in(t)   [new write] 
                else: out(t+1) = out(t)  [DFF output (keep)]
     ```
 2. **Multi-bit Register (combine of several 1-bit registers)**  
     ```
     if load(t) True: out(t+1) = in(t)  
                else: out(t+1) = out(t)
     ```
 3. **RAM (Random Access Memory / combine of several multi-bit registers)**
     ``` 
     in      : write value
     address : assign certain register
     load    : write or not
     out     : output & keep value of t+1


     Full RAM Structure

             |--------------------------------------------------------|
        in   |                          ⭣ load                        |
     ------->|        .       in   |------------|  out      .         |
             |     .  .      ----> |  register  | ---->     .  .      |
       load  |   .    .            |------------|           .Mux .    |   out
     ------->| . DMux .                 ⭣ load              .(n-1) .  |------->
             |   .    .       in   |------------|  out      .    .    |
     address |     .  .      ----> |  register  | ---->     .  .      |
     ------->|        .            |------------|           .         |
             |--------------------------------------------------------|

     ```
 4. **Counter (Keep address of START of next instruction)**
    ```
    increment [PC++] : Normally go to next instruction
    load      [PC=n] : Jump to certain line
    reset     [PC=0] : Back to the first line

    if    reset(t) True: out(t+1) = 0
    elif   load(t) True: out(t+1) = in(t)
    elif    inc(t) True: out(t+1) = out(t) + 1
    else               : out(t+1) = out(t)
    ```

 5. **Data Flip Flop**
    ```
    out(t+1) = in(t)
    ```
    
#### Project 4 - Machine Language

 1. **A-instruction**
    
     |15 |14 |13 |12 |11 |10 | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
     |---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
     | 0 | x | x | x | x | x | x | x | x | x | x | x | x | x | x | x |
    
 2. **C-instruction**
    
     |15 |14 |13 |12 |11 |10 | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
     |---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
     | 1 | 1 | 1 | a | c | c | c | c | c | c | A | D | M | j | j | j |
    
     ----Code for C----- -A/M- -----------comp(ALU)------------ ----registers---- ---jump type---   



#### Project 5 - Computer Architecture
![avatar](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRB5TqnwXkVGh1MSFw7u1L0xBH26fw5iRqRfQ&s)
 1. Memory-mapped I/O

#### Project 6 - Assembler
