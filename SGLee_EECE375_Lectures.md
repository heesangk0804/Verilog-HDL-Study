# Verilog HDL Manuals & Tips
## Module 5 : HDL

(1) Digital Circuit Design Flow

: Design Spec > High-Level Design > Testbench Development > Behavioral Simul \>

                  (          Verilog HDL Coding       )
                
Gate-Lv Design > Timing Simul > Board Implem > Testing

     (   Netlist Synthesis   )

(2) Hardware Description Language
* Definition: Programming language for description of digital hardware
  * Hardware inherently PARALLEL, CONCURRENT
* Advantages
  * highly portable: simple text file(*.v) w/ standard language format
  * behavioral description: can describe intended behavior of circuit w/out any structural detail
  * automatic synthesis: HDL code synthesized to “automatically” produce circuits; less human errors
  * portable for testing: test bench also written using HDL, compatibility and portability
* Methods
  * Behavioral: Procedural description of actual operation (pseudocode descript)
  
  -> best for synthesis! 
```
module HADD (
   output s, cout,
   input i0, i1
   );
   assign {cout, s} = i0 + i1;
endmodule
```
  * Data Flow: Modeling with only combinational logic
  * Structural: Specifies device connection; Block/Gate level
```
module HADD (
   output s, cout,
   input i0, i1
   );
   and g1(cout, i0, i1);
   xor g2(s, i0, i1);
endmodule
```

(3) RTL Design & Synthesis
* Definition: Register-Transfer Level Notation = all operations described as trasfer of data between registers
  * All description = (1) Register declaration (2) Register assignment(data transfer)
> regx <- f(reg1, reg2, ... , regn)
```
  reg[N-1:0] r0, r1, r2;
  r0 <- r1 + r2;
  r0 <- r0 << 2;
```

(+) LUT

: small memory device, with input = address / output = data

-> could be internally designd as specific CL mapping input to output

## Module 6 : Verilog HDL Overview, TestBench

(1) Verilog Coding: Basic Description -> Hierarchial Design
* vs Computer Language

(2) Verilog Basics

(a) Data Types
* nets: connections between hardware elements; signal values continuously driven, transmitted
  * declaration keyword: ``wire`` (1bit)
* registers: data storage element(=variable); retain value without driver until another value being overwritten
  * declaration keyword: ``reg`` (1bit)
* vectors: multiple bit nets or regs
  * declaration keyword: ``wire/reg [high#:low#]``
* paramters: named resettable constant representing a value; defined at module description or instantiation
  * declaration keyword: ``parameter``
  * modification keyword: ``#(-val-, -val-)``
* integer, real, time
  * ``integer`` (32b): register datatype of storing integers; exploited for designing counters 
  * ``real`` : constant datatype representing real numbers 
  * ``time`` (64b) : register datatype of storing simulation time 

(b) Algebraic Structure
* 4 logic values:
  * ``0`` = false / ``1`` = true / ``x`` = unknown / ``z`` = high-imp, float
* Numbers
  * Syntax: (sign) (# of bits) '(radix) (number)_(number)  <br/>
  ``2'b11`` ``12'hfff`` ``-16'd128``

(c) Operators <br/> (Example: ``A = 4'b1010 == 4'd10, B = 4'b0011 = 4'd3``)
  * Arithmetic: <br/> ``A + B == 4'b1101``, ``A - B == 4'b0111``, ``A * B == 4'b1110``, ``A / B == 4'b0011``, ``A % B == 4'b0001``
  * Logical: Binary Operator yielding 1bit; operand == 1'b1 if nonzero, == 1'b0 if zero <br/> ``A && B == 1'b1``, ``A || B == 1'b1``, ``!A == 1'b0``
  * Relational: <br/> ``A > B == 1'b1``, ``A <(=) B == 1'b0``,
  * Equality: <br/> ``A == B == 1'b0(x if x, z)``, ``A != B == 1'b1(x if x, z)``, ``A === B == 1'b0(for x, z)``, ``A !== B == 1'b1(for x, z)``
  * Bitwise: calculate btwn bits of same position of each operands, yielding to same bit position at output <br/> ``A & B == 4'b0010``, ``A | B == 4'b1011``, ``~A == 4'b0101``, ``A ^ B == 4'b1001``, ``A ^~ B == 4'b0110``
  * Reduction: Unary operator, calculate btwn bits among each position, yielding 1 bit <br/> ``&A == 1'b0``, ``|A == 1'b1``, ``^A == 1'b0``
  * Shift: shift left operand bit position by right operand number, fill in 0s <br/> ``A >> 1 == 4'b0101``, ``A << 2 == 4'b1000``
  * Concatenate: append multiple operands into 1 vector, from left = MSB to right = LSB  <br/>  ``{A, B} = 8'b10100011``
  * Replication: repetitive concatenation of operand by number outside bracket <br/> ``2{A} = 8'b10101010``
  * Conditional: Tenary operator, evaluate logic of 1st operand -> 1: 2nd operand evaluated / 0: 3rd operand evaluated / x: 2, 3 evaluated and compared <br/>  ``(A==1): A ? B == 4'b0011``

(d) System Tasks <br/> : routine operation including displaying on screen, monitoring net values, stop & finish execution (Syntax: ``$ <keyword>``)
  * Displaying
    * ``$display("%b, %d, %f", p1, p2, p3, ...)`` : display strings or data in format on screen
  * Monitoring
    * ``$display("%b, %d, %f", p1, p2, p3, ...)`` : display strings or data on screen every time the signal data(p1, p2, p3, ...) changes
  * Stopping and Finishing
    *  ``$stop`` : stop, pause the simulation
    *  ``$finish`` : terminate the simulation
  * Simulation Time Report
    *  ``$time`` : display current simulation time in 64b
    *  ``$stime`` : display current simulation time in 32b
    *  ``$realtime`` : display current simulation time in realnum
    *  ``$random`` :
  * File Operation
    *  ``$fopen`` :
    *  ``$fdisplay`` ``$fmonitor`` ``$fwrite``
    *  ``$fgets`` ``$fscanf`` ``$fread``
    *  ``$fclose``

(e) Compiler Directives <br/> : pre-processing tasks done before compiling, including macro define, alternate file inclusion (Syntax: `` `<keyword>``) 
  * Macro Define
    * `` `define <TEXT> <value>`` : define text macros to be substituted to value when being compiled
  * Source File Inclusion
    * `` `include <filename>`` : include other source files to this file while compiling
  * `` `ifdef``
  * `` `timescale``

(3) Module, Block Design

(a) Modules & Ports
* module : design block with i/o ports performing specific function
  * hides internal structure: hierarchial design (fixing, unaffecting lower level designs) available
  * syntax for module description (keyword: ``module`` (encapsulated) ``endmodule``)
```
module mod_name (inputs, outputs, ...);
  (datatype def, assign/always statements)
endmodule 
```
*
  *  syntax for module instantiation = instance creation
```
mod_name M0(q[0], reg1, clk, out, ...);
mod_name M2(.in0(q[0]), .in1(reg1), .clk(clk), .out0(out), ...);
```
* ports: signal interface by which a module communicates externally; only shown explicitly outside a module
  * Declaration keyword: <br/>``input`` / ``output`` / ``inout``
  * Connection Rules: <br/>``input``: internally as net, externally to reg or net <br/>``output``: internally as reg or net, externally to net <br/>``inout``:internally as net, externally to net
  * Connection Styles
    * by Order: ``ADD A0(A1, B1, C1, C2, S1)``
    * by Name: ``ADD A0(.A(A1), .B(B1), .Cin(C1), .Cout(C2), .S(S1))``

(b) Design Block Modeling
* **Structural Modeling (Gate-Level)** <br/> : Design a gate-level digital system, then convert it to code by **instantizing primitive gates**
  * Primitive Gates: pre-defined modules operating boolean logic operations <br/> Syntax: ``<gate_name> <instant_name> (<output>, <input0>, <input1>);``
  * and/or gates: ``and`` ``or`` ``nand`` ``nor`` ``xor`` ``xnor``
  * buf/not gates: ``not`` ``buf`` ``bufif1`` ``bufif0`` ``notif1`` ``notif0``
  * Gate Delay: described by delay operator # <br/> ``<gate_name> #(<risetime>,<falltime>,<turnofftime>) <instant_name> (...)``, <br/>``<gate_name> #(<mindelay>:<typdelay>:<maxdelay>) <instant_name> (...)``
  * Example Code
```
module full_adder (
  output s, cout,
  input a, b, cin
  );
  wire tc0, tc1, ts0;

  and g0(tc0, a, b);
  xor g1(ts0, a, b);
  
  and g2(tc1, ts0, cin);
  xor g3(cout, tc0, tc1);
  xor g4(s, ts0, cin);
endmodule
```

* **Dataflow Modeling (Continuous Assignment)** <br/> : Design in more abstract approach with **continuous assignment** of expressions using keyword ``assign``
  * Continuous Assignment: concurrent assignment between nets (NOT reg); change in RHS net instantly assigned to LHS net <br/> Syntax: ``assign out = i0 & i1;``
  * Delay: <br/> Regular: ``assign #(<td>) <LHS> = <RHS>;`` <br/> Implicit: ``wire #(<td>) <LHS> = <RHS>;`` <br/> Net Declaration: ``wire #(<td>) <LHS>; assign <LHS>  = <RHS>;``
  * Example Code
```
module full_adder (
   output s, cout,  //net
   input a, b, cin  //net
   );
  ///logic expression
  assign s = a ^ b ^ cin;
  assign cout = (a & b) | (b & c) | (c & a);
  ///
  assign {cout, s} = a + b + c; //arithmetic expression
endmodule
```


* **Behavioral Modeling (Procedural Assignment)** <br/> : Design in most abstract = sequential approach with **procedural assignment** using keyword ``intial``or ``always``
  * Initial Statement: executed at t=0, only once, independently from other blocks <br/> Applications: initialization of registers(especially test vectors, clk, reset), system tasks
  * Always Statement: executed repetitively every time any change of condition events or signals occur 
  * Procedural Assignment: assignment on register or storage var; LHS var keeps data until new data overwritten <br/> * Non Blocking: sequential assignment of code lines ``b = a; c = b; a = c; \\ a == a0``  <br/> * Blocking: concurrent assignment of code lines ``b = a; c = b; a = c; \\ a == c0``
  * Timing Control <br/> Delay-Based: ``always begin #(<td>) ... `` <br/> Event-Based: ``always @(posedge clk) ``(regular) ``always @(reset or enable)``(event) <br/> Level-Sensitive: ``wait(enable)``
  * Conditional Statements: <br/> ``if(<condition>) <true_statement>; else if(<condition>) <true_statement2>; else <false_statement>;``  <br/> ``case(<condition>) <N'bXX> : <state1>; <N'bXX> : <state2>; default: <state3> endcase;`` <br/> ``casez``: ``z``-> ``dc``, ``casex``: ``x, z``-> ``dc``
  * Loop Statements: <br/> ``while(<condition>)`` <br/>``for(cnt=1; cnt<MAX; cnt=cnt+1)`` <br/> ``repeat(<N>)``<br/>``forever``
  * Tasks <br/>: called in module, used for procedure containing delays/timing/event constructs, no return but multiple output ports, no internal nets
```
module <>;
...
<task_name>;
...
endmodule

task <task_name>;
output [WO:0] o0, o1, ...;
input [WI:0] i0, i1, ...;
begin
  #<td0> o0 = <expression0>;
  #<td1> o1 = <expression1>;
end
endtask
```
*  * Functions <br/>: called in module, used for procedures with purely CL(no procedural), executing in zero time, 1 or more input, single return no output port, no internal nets
```
module <>;
...
reg0 = <func_name>;
...
endmodule

function <func_name>;
input [WI:0] i0, i1, ...;
begin
  <func_name> = <expression>;
end
endfunction
```

  * Example Code
```
initial begin
end

always begin
end

always @(<condition>) begin
end
```

(c) Test Bench(Stimulus Block) Modeling <br/>: stimulus block; (1)generate test vector input signals and (3)check DUT operation, output by (2)flowing signals into DUT
* Objectives
  * Test functionality of DUT
  * Test manufacturing defects
* Form: written in same manner as Verilog module
  * Advantages: Portable, Reproducible, Compatible 
* Components
  * Test Vector Generator: define test vector signal storage, generate vectors using behavioral code(``initial``, ``always`` statement)
  * Device Under Test(DUT): Instantize, connect input&output ports with test vector signals
  * Test Result Checker(optional): functions, tasks for checking output, involving system tasks(``display``, ``monitor``)
* Module Hierarchy Config
* Practical Considerations


## Module 7 : Verilog Modeling Combinational Logic

(1) 4-Valued Logic System
(2) Code Examples
* 2:1 MUX (structural design)
```
`timescale 100ps/10ps   // 1 time unit = 100ps, precision = 10ps
module tb_mux2to1();
  wire out0; reg in0, in1, sel; // IO declare for test vector signals conn. to DUT
  
  mux2to1 M0 (out0, in0, in1, sel); // IO port connection at DUT
  
  initial begin // input test vector waveform generation
    in0 = 1’b0;  in1 = 1’b0;  sel = 1’b0;  // (0,0,0)
    #40;  in0 = 1’b1;                      // (1,0,0)
    #40;  sel = 1’b1;                      // (1,0,1)
    #40;  in1 = 1’b1; #40;                 // (1,1,1)
  end
endmodule

module mux2to1(
  output o0;
  input i0, i1, sel;
  );
  //o0 = or(and((sel)',io), and(sel, i1)) = nand(nand((sel)',io), nand(sel, i1))

  wire nsel, mt0, mt0;

  not g1(nsel, sel);
  nand g2(mt0, i0, nsel);
  nand g3(mt1, i1, sel);
  nand g4(o0, mt0, mt1);
endmodule
```
* 1-bit Full Adder (dataflow modeling)
```
`timescale 100ps/10ps
module tb_full_adder();
  wire sum, cout; reg in0, in1, cin; // IO declare for test vector signals conn. to DUT
  
  full_adder M0 (.s(sum), .co(cout), .a(in0), .b(in1), .ci(cin)); // IO port connection at DUT
  
  initial begin // input test vector waveform generation
    in0 = 1’b0;  in1 = 1’b0;  cin = 1’b0;        // (0,0,0)
    #10;  cin = 1’b1;                            // (0,0,1)
    #10;  in1 = 1’b1;                            // (0,1,1)
    #10;  in1 = 1’b1;  in1 = 1’b0;  cin = 1’b0;  // (1,0,0)
    #10;
    $finish;    //simulation termination
  end
endmodule

module full_adder (
  output s, co,
  input a, b, ci
  );
  assign {co, s} = a + b + ci; //arithmetic expression
endmodule
```

* 4:2 Priority Encoder (behavioral modeling)
```
`timescale 100ps/10ps
module tb_priority_encoder_8to3();
  wire [1:0] test_encode; reg [3:0] test_line; // IO declare for test vector signals conn. to DUT

  priority_encoder_8to3 M0 (test_encode, test_line); // IO port connection at DUT
  
  initial begin // input test vector waveform generation
    test_line = 4’b0000;
    #10; test_line = 4’b0100;
    #10; test_line = 4’b0110;
    #10; test_line = 4’b0010;
    #10; test_line = 4’b1011;
    #10;
    $finish;    //simulation termination
  end
endmodule

//4:2 Priority Encoder: outputs encoded index for active input line with low-to-high num priority
module priority_encoder_4to2 (
  output reg [1:0] encode, input [3:0] line
  );
  always @ (line) begin
    casex (line)
      4'b1xxx : encoded = 2'b00;
      4'b01xx : encoded = 2'b01;
      4'b001x : encoded = 2'b10;
      default : encoded = 2'b11;
    endcase
  end
endmodule
```

## Module 8 : Verilog Modeling State Machine

(1) Verilog Sequential Logic = always not assign

(2) Moore FSM
* example
* testbench
* Blocking Assignment

(3) Mealy FSM


**2. Insert Mode**

Access: pressing **i** or **a** at command mode

**i**: start at (front of) where cursor belongs / **a**: start at 1 char after the cursor

Activity: able to write text freely by keyboard

**shift+spacebar** : ENG/KOR translate

**3. Colon Mode**

Access: press **:** key at command mode

Activity: able to save the text file or quit editor

![vi_editor_structure](https://github.com/heesangk0804/Linux/assets/149554144/6db62260-f117-4147-aa02-8f6a803e071b)

## vi Commands

**0. Quit, Save**
> :q = exit
>
