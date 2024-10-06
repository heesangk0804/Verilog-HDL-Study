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
