# Verilog HDL Manuals & Tips
## Lecture Chapters
**Module 5 : HDL**

(1) Digital Circuit Design Flow

: Design Spec > High-Level Design > Testbench Development > Behavioral Simul \>

                  (          Verilog HDL Coding       )
                
Gate-Lv Design > Timing Simul > Board Implem > Testing

     (   Netlist Synthesis   )

(2) Hardware Description Language
* Definition: Programming language for description of digital hardware
  * Hardware inherently PARALLEL 
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

**Module 6 : Verilog HDL Overview, TestBench**

(1) Verilog Coding: Basic Description -> Hierarchial Design
* vs Computer Language

(2) Modules & Ports
* module : design block with i/o ports performing specific function
  * hides internal structure: hierarchial design (fixing, unaffecting lower level designs) available
  * syntax for module definition (declaration keyword: ``module``)
```
module mod_name (inputs, outputs, ...);
  (datatype def, assign/always statements)
endmodule 
```
  * syntax for module instantiation = instance creation
```
mod_name M0(q[0], reg1, clk, out, ...);
mod_name M2(.in0(q[0]), .in1(reg1), .clk(clk), .out0(out), ...);
```
* ports: signal interface by which a module communicates externally
  * only shown explicitly outside a module 
  * declaration keyword:
  * ``input`` = conn to reg + net/ ``output`` = conn to net/ ``inout`` = conn to net

(3) Data Types
* nets: connections between hardware elements; signal values continuously driven, transmitted
  * declaration keyword: ``wire`` (1bit)
* registers: data storage element(=variable); retain value without driver until another value being overwritten
  * declaration keyword: ``reg`` (1bit)
* vectors: multiple bit nets or regs
  * declaration keyword: ``wire/reg [high#:low#]``
* 4 logic values:
  * ``0`` = false / ``1`` = true / ``x`` = unknown / ``z`` = high-imp, float
* number

(2) Test Bench
* Objectives, Advantages
* Components
* Module Hierarchy Config
* Practical Considerations

**Module 7 : Verilog Modeling Combinational Logic**

(1) Logic System : 4-Valued (0 / 1 / z / x)

(2) Code Examples
* mux
* Full Adder
* Test Bench
* Priority Encoder

Module 8 : Verilog Modeling State Machine

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
