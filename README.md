Code for design:
timescale 1ns / 1ps
module sequencedetector(
input x,clk,reset,
output reg z );
parameter S0 = 0 , S1 = 1 , S2 = 2 , S3 = 3 ;
reg [1:0] PS,NS ;
    always@(posedge clk or posedge reset)
        begin
            if(reset)
                PS <= S0;   
            else    
                PS <= NS ;
        end             
    always@(PS or x)
        begin 
            
            case(PS)
                S0 : begin 
                            z = 0 ;
                            NS = x ? S1 : S0 ;
                            $display(PS);
                        end
                S1 : begin 
                            z = 0 ;
                            NS = x ? S1 : S2 ;
                            $display(PS);
                        end
                S2 : begin 
                            z = 0 ;
                            NS = x ? S3 : S0 ;
                            $display(PS);
                        end 
                S3 : begin 
                            z = x ? 1 : 0 ; 
                            NS = x ? S1 : S2 ;
                            $display(PS);
                        end
            endcase
        end
endmodule




code for testbench:
timescale 1ns / 1ps
module mealyfsm;
    // Inputs
    reg x;
    reg clk;
    reg reset;
    // Outputs
    wire z;
    // Instantiate the Unit Under Test (UUT)
    sequencedetector uut (
        .x(x), 
        .clk(clk), 
        .reset(reset), 
        .z(z)  ); 
initial
    begin
        clk = 1'b0;
        reset = 1'b1;
        #15 reset = 1'b0;
    end
always #5 clk = ~ clk;  
initial begin
        #12 x = 0;#10 x = 0 ; #10 x = 1 ; #10 x = 0 ;
        #12 x = 1;#10 x = 1 ; #10 x = 0 ; #10 x = 1 ;
        #12 x = 1;#10 x = 0 ; #10 x = 0 ; #10 x = 1 ;
        #12 x = 0;#10 x = 1 ; #10 x = 1 ; #10 x = 0 ;
        #10 $finish;
    end     
endmodule
