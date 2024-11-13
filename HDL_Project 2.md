HDL_Project 2

1. HalfAdder
```
CHIP HalfAdder {
  IN a, b;
  OUT sum, carry;
  PARTS: 
  Xor(a= a, b= b, out = sum);
  And(a= a, b= b, out = carry);
}
```


2. FullAdder
```
CHIP FullAdder {
    IN a, b, c;  // 1-bit inputs
    OUT sum,     // Right bit of a + b + c
        carry;   // Left bit of a + b + c

    PARTS:
    //a + b = c1 s1
    Xor(a= a, b= b, out= sum1);
    And(a= a, b= b, out= carry1);
    //s1 + c = sum c2
    Xor(a= sum1, b= c, out= sum);
    And(a= sum1, b= c, out= carry2);
    //c1 + c2 = carry
    Xor(a= carry1, b= carry2, out= carry);
}
```
   
3. Add16

```
CHIP Add16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
    HalfAdder(a= a[0], b= b[0], sum= out[0], carry = c1);
    FullAdder(a= a[1], b= b[1], c= c1,sum= out[1], carry= c2);
    FullAdder(a= a[2], b= b[2], c=  c2,  sum= out[2], carry= c3);
    FullAdder(a= a[3], b= b[3], c=  c3,  sum= out[3], carry= c4);
    FullAdder(a= a[4], b= b[4], c=  c4,  sum= out[4], carry= c5);
    FullAdder(a= a[5], b= b[5], c=  c5,  sum= out[5], carry= c6);
    FullAdder(a= a[6], b= b[6], c=  c6,  sum= out[6], carry= c7);
    FullAdder(a= a[7], b= b[7], c=  c7,  sum= out[7], carry= c8);
    FullAdder(a= a[8], b= b[8], c=  c8,  sum= out[8], carry= c9);
    FullAdder(a= a[9], b= b[9], c=  c9,  sum= out[9], carry= c10);
    FullAdder(a= a[10], b= b[10], c=  c10,  sum= out[10], carry= c11);
    FullAdder(a= a[11], b= b[11], c=  c11,  sum= out[11], carry= c12);
    FullAdder(a= a[12], b= b[12], c=  c12,  sum= out[12], carry= c13);
    FullAdder(a= a[13], b= b[13], c=  c13,  sum= out[13], carry= c14);
    FullAdder(a= a[14], b= b[14], c=  c14,  sum= out[14], carry= c15);
    FullAdder(a= a[15], b= b[15], c=  c15,  sum= out[15], carry= c16);
}
```

4. Inc16
```
CHIP Inc16 {
    IN in[16];
    OUT out[16];

    PARTS:
    HalfAdder(a= in[0], b= true, sum= out[0], carry = c1);
    HalfAdder(a= in[1], b= c1, sum= out[1], carry= c2);
    HalfAdder(a= in[2], b= c2, sum= out[2], carry= c3);
    HalfAdder(a= in[3], b= c3, sum= out[3], carry= c4);
    HalfAdder(a= in[4], b= c4, sum= out[4], carry= c5);
    HalfAdder(a= in[5], b= c5, sum= out[5], carry= c6);
    HalfAdder(a= in[6], b= c6, sum= out[6], carry= c7);
    HalfAdder(a= in[7], b= c7, sum= out[7], carry= c8);
    HalfAdder(a= in[8], b= c8, sum= out[8], carry= c9);
    HalfAdder(a= in[9], b= c9, sum= out[9], carry= c10);
    HalfAdder(a= in[10], b= c10, sum= out[10], carry= c11);
    HalfAdder(a= in[11], b= c11, sum= out[11], carry= c12);
    HalfAdder(a= in[12], b= c12, sum= out[12], carry= c13);
    HalfAdder(a= in[13], b= c13, sum= out[13], carry= c14);
    HalfAdder(a= in[14], b= c14, sum= out[14], carry= c15);
    HalfAdder(a= in[15], b= c15, sum= out[15], carry= c16);
}
```

5. ALU
```
// This file is part of www.nand2tetris.org
// and the book "The Elements of Computing Systems"
// by Nisan and Schocken, MIT Press.
// File name: projects/2/ALU.hdl
/**
 * ALU (Arithmetic Logic Unit):
 * Computes out = one of the following functions:
 *                0, 1, -1,
 *                x, y, !x, !y, -x, -y,
 *                x + 1, y + 1, x - 1, y - 1,
 *                x + y, x - y, y - x,
 *                x & y, x | y
 * on the 16-bit inputs x, y,
 * according to the input bits zx, nx, zy, ny, f, no.
 * In addition, computes the two output bits:
 * if (out == 0) zr = 1, else zr = 0
 * if (out < 0)  ng = 1, else ng = 0
 */
// Implementation: Manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) sets x = 0        // 16-bit constant
// if (nx == 1) sets x = !x       // bitwise not
// if (zy == 1) sets y = 0        // 16-bit constant
// if (ny == 1) sets y = !y       // bitwise not
// if (f == 1)  sets out = x + y  // integer 2's complement addition
// if (f == 0)  sets out = x & y  // bitwise and
// if (no == 1) sets out = !out   // bitwise not

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute (out = x + y) or (out = x & y)?
        no; // negate the out output?
    OUT 
        out[16], // 16-bit output
        zr,      // if (out == 0) equals 1, else 0
        ng;      // if (out < 0)  equals 1, else 0

    PARTS:
    //// Replace this comment with your code.
    
    //x part
    //x zero
    Mux16(a= x, sel= zx, out= xzero);
    //x negative
    Not16(in= x, out= xnot);
    //select by negative
    Mux16(a= xzero, b= xnot, sel= nx, out= fixedx);

    //y part
    //y zero
    Mux16(a= y, sel= zy, out= yzero);
    //y negative
    Not16(in= y, out= ynot);
    //select by negative
    Mux16(a= yzero, b= ynot, sel= ny, out= fixedy);

    //x + y
    Add16(a= fixedx, b= fixedy, out= xAddy);

    //x & y
    And16(a= fixedx, b= fixedy, out= xAndy);

    //select by f
    Mux16(a= xAndy, b= xAddy, sel= f, out= xyf);

    //select by no
    Not16(in= xyf, out= Nxyf);
    Mux16(a= xyf, b= Nxyf, sel= no, out= xyO);

    //zr ng sign
    //output = 0 then zr = 1

}
```
