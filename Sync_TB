`timescale 1ns/1ps

module testbench();
  reg clk_i, rst_n_i, write_en_i, read_en_i;
  reg [15:0] data_in_i;
  reg [3:0] read_ptr, write_ptr;
  wire fifo_full_o, fifo_empty_o;
  wire [15:0] data_out_o;

  // Instantiate the FIFO module
  Sync_FIFO S1(clk_i, rst_n_i, write_en_i, read_en_i, data_in_i, data_out_o, fifo_full_o, fifo_empty_o);

  // Clock generation
  always #2 clk_i = ~clk_i;

  initial 
  begin
    clk_i = 1'b0; 
    rst_n_i = 1'b0;
    write_en_i = 1'b0;
    read_en_i = 1'b0; 

    #10 rst_n_i = 1'b1;
    drive(30);
    drive(20);
    $finish;
  end

  always@(*)
  begin
    read_ptr <= S1.read_ptr;
    write_ptr <= S1.write_ptr;
  end

  task push();
    if(!fifo_full_o)
    begin
      write_en_i = 1;
      data_in_i = $random;
      #1 write_en_i = 0;
    end
  endtask

  task pop();
    if(!fifo_empty_o)
    begin
      read_en_i = 1;
      #1 read_en_i = 0;
    end
  endtask

  task drive(input integer delay);
  begin
    write_en_i = 0; 
    read_en_i = 0;
    fork
      begin
        repeat(25) begin @(posedge clk_i) push(); end
        write_en_i = 0;
      end
      begin
        #delay;
        repeat(20) begin @(posedge clk_i) pop(); end
        read_en_i = 0;
      end
    join
  end
  endtask

endmodule
