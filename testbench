`timescale 1ns/1ps

module testbench();
reg clk, rst_n, w_en, r_en;
reg [15:0] data_in;
reg [3:0] rdptr, wtptr;
wire full,empty;
wire [15:0] data_out;

Synchronous_FIFO S1(clk,rst_n,w_en,r_en,data_in, data_out, full, empty);

always #2 clk = ~clk;

initial 
begin
clk = 1'b0; rst_n = 1'b0;
w_en = 1'b0;
r_en = 1'b0; 

#10 rst_n = 1'b1;
drive(30);
drive(20);
$finish;
end

always@(*)
begin
    rdptr <= S1.r_ptr;
    wtptr <= S1.w_ptr;
end

task push();
    if(!full)
    begin
    w_en = 1;
    data_in = $random;
    #1 w_en = 0;
    end
endtask

task pop();
    if(!empty)
    begin
    r_en =1;
    #1 r_en = 0;
    end
endtask

task drive(input integer delay);
begin
    w_en =0; r_en=0;
    fork
        begin
        repeat(25) begin @(posedge clk) push();end
        w_en = 0;
        end
        begin
        #delay;
        repeat(20) begin @(posedge clk) pop(); end
        r_en = 0;
        end
    join
 end
endtask



endmodule
