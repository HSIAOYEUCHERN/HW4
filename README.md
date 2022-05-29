# HW4

BCD design :
```script=
module BCD(clk, rst_asyn, Q_out);
input clk; 
input rst_asyn;
output [3:0] Q_out;
reg [3:0] Q_out=0;
  
always@ (posedge clk )
begin
  	if (rst_asyn)
		Q_out <= 0;
    else if (Q_out == 9)
		Q_out <= 0;
	else
		Q_out <= Q_out + 1;
end

endmodule
```
BCD testbench :
```
`timescale 1ns/1ps
module BCD_tb;
  reg clk;
  reg rst_asyn;
  wire [3:0] Q_out;
  parameter PERIOD = 20;
  parameter real DUTY_CYCLE = 0.5;
  parameter OFFSET = 0; 
  
  BCD BCD_tb (
      .clk(clk),
      .rst_asyn(rst_asyn),
      .Q_out(Q_out)
  );

  initial begin
  	#OFFSET;
  	forever
  	begin
    	clk = 1'b0;
    	#(PERIOD-(PERIOD*DUTY_CYCLE)) clk = 1'b1;
    	#(PERIOD*DUTY_CYCLE);
  	end
  end
   
  initial begin
    rst_asyn = 0;
    #50 rst_asyn=1'b1;
    #5	rst_asyn=0'b0;
    #250 $finish;
  end
  
  initial begin
    $dumpfile("BCD.vcd");
    $dumpvars(0, BCD_tb);
  end
endmodule 
```
![image](https://user-images.githubusercontent.com/101077336/170857254-7a89e81c-7643-4a67-8ecb-5e2580b9d5ee.png)
