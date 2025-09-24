# EXPERIMENT 3B: Simulation of All Flip-Flops using Non Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **Non blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **Non blocking assignment (`<=`)** inside the `always` block.  
Non Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Non Blocking)
```verilog
module sr_ff (
    input  S,     
    input  R,     
    input  clk,   
    input  rst, 
    output reg Q,      
);
always @(posedge clk) 
begin
    if (rst==1)
        Q <= 1'b0;   
    else 
begin
        case ({S,R})
            2'b00: Q <= Q;     
            2'b01: Q <= 1'b0;  
            2'b10: Q <= 1'b1;  
            2'b11: Q <= 1'bX;  
        endcase
    end
end

endmodule


```
### SR Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_sr_ff;
   reg S, R, clk, rst;
    wire Q;
    sr_ff uut (S,R,clk,rst,Q);
    always #5 clk = ~clk;
    initial begin
        clk = 0; S = 0; R = 0; rst = 1;

        #10 rst = 0;    
        #10 S=1; R=0;   
        #10 S=0; R=0;   
        #10 S=0; R=1;   
        #10 S=1; R=1;   
        #10 S=0; R=0;   
        #20 $finish;
    end
 initial begin
        $monitor("Time=%0t | clk=%b rst=%b | S=%b R=%b | Q=%b", $time, clk, rst, S, R, Q);
    end
endmodule

```
#### SIMULATION OUTPUT

<img width="1036" height="658" alt="image" src="https://github.com/user-attachments/assets/b05a5e7b-a0dd-458b-9e79-50c0252edef2" />

---

### JK Flip-Flop (Non Blocking)
```verilog
module JK_FF(J,K,clk,rst,Q);
input J,K,clk,rst;
output reg Q;
always @ (posedge clk) 
begin
if (rst == 1)
Q<=0;   
else begin
case ({J, K})
2'b00: Q <= Q;     
2'b01: Q <= 1'b0;  
2'b10: Q <= 1'b1;  
2'b11: Q <= ~Q;    
endcase
end
end
endmodule
```
### JK Flip-Flop Test bench 
```verilog
`timescale 1ns / 1ps
module tb_JK_FF;
reg J,K,clk,rst;
wire Q;
JK_FF uut (J, K, clk, rst, Q);
always #5 clk = ~clk;
initial begin
clk = 0; J = 0; K = 0; rst = 1;
#10 rst = 0;
#10 J = 1; K = 0;   
#10 J = 0; K = 1;   
#10 J = 1; K = 1;   
#10 J = 0; K = 0;   
#10 J = 1; K = 1;   
#20 $finish;
end
endmodule

```
#### SIMULATION OUTPUT
<img width="1041" height="662" alt="image" src="https://github.com/user-attachments/assets/f137d608-9e33-4880-a6b4-9d1fb0b3e366" />

---
### D Flip-Flop (Non Blocking)
```verilog
module D_FF(D,clk,rst,Q);
input D,clk,rst;
output reg Q;

always @ (posedge clk)
begin
if (rst == 1)
    Q <= 0;        
else
    Q <= D;        
end
endmodule
```
### D Flip-Flop Test bench 
```verilog
`timescale 1ns / 1ps
module tb_D_FF;
reg D,clk,rst;
wire Q;
D_FF uut(D,clk,rst,Q);
  always #5 clk = ~clk;
initial 
begin
clk = 0; D = 0; rst = 1;
#10 rst = 0;
#10 D = 1;   
#10 D = 0;   
#10 D = 1;   
#10 D = 1;   
#10 D = 0;   
#10 D = 1;   
#20 $finish;
end
endmodule

```

#### SIMULATION OUTPUT

<img width="1048" height="647" alt="image" src="https://github.com/user-attachments/assets/54aef9a5-5e23-4a02-8d83-1cf4fec64cce" />

---
### T Flip-Flop (Non Blocking)
```verilog
module T_FF(T,clk,rst,Q);
  input T,clk,rst;
  output reg Q;

  always @ (posedge clk) 
  begin
    if (rst == 1)
      Q <= 0;          
    else if (T == 1)
      Q <= ~Q;         
    else
      Q <= Q;          
  end
endmodule
```
### T Flip-Flop Test bench 
```verilog
`timescale 1ns / 1ps
module tb_T_FF;
  reg T, clk, rst;
  wire Q;
T_FF uut (T, clk, rst, Q);
always #5 clk = ~clk;
initial 
begin
    clk = 0; T = 0; rst = 1;
    #10 rst = 0;
    #10 T = 1;   
    #20 T = 0;   
    #20 T = 1;   
    #20 T = 0;  
    #20 $finish;
 end
endmodule

```

#### SIMULATION OUTPUT
<img width="1041" height="657" alt="image" src="https://github.com/user-attachments/assets/70f32ba1-daf0-4647-9bd7-bbf3cec26b66" />


---

### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using Non blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.
