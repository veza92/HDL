# HDL Code
# Design Code: The Verilog HDL 8 bit Wide, 64-Bit Long Shift Register with Evenly Spaced Taps
module shift_8x64_taps(clk, shift, sr_in, sr_out, sr_tap_one, sr_tap_two, sr_tap_three);
    input clk, shift;
    input [7:0] sr_in;
  output [7:0] sr_tap_one, sr_tap_two, sr_tap_three, sr_out;
    
    reg [7:0] sr [63:0];
    integer n;
    always @ (posedge clk)
    begin
      if (shift == 1'b1)
        begin
            for (n = 63; n > 0; n = n-1)
            begin
                sr[n] <= sr[n-1];
            end
            sr[0] <= sr_in;
        end
    end
    assign sr_tap_one = sr[15];
    assign sr_tap_two = sr[31];
    assign sr_tap_three = sr[47];
    assign sr_out = sr[63];
endmodule

# TestBench Code
module shift_8x64_taps_tb;
  wire sr_tap_one, sr_tap_two, sr_tap_three, sr_out;
  reg clk, shift, sr_in;
  
  initial
    begin
      clk = 1'b0; shift = 1'b0; sr_in = 1'b0;
      #10 clk = 1'b0; shift = 1'b1; sr_in = 1'b1;
      #20 clk = 1'b1; shift = 1'b0; sr_in = 1'b0;
      #30 clk = 1'b1; shift = 1'b1; sr_in = 1'b1;
    end
  initial
  
    $monitor($time, "clk=%shift shift=%sr_in sr_in = %sr_in, sr_out = %sr_in, sr_tap_one = %sr_in,   sr_tap_two = %sr_in, sr_tap_three = %sr_in", clk, shift, sr_in, sr_out, sr_tap_one, sr_tap_two, sr_tap_three);
  initial
    begin
      $dumpfile("dump.vcd"); $dumpvars(1);
    end
  
endmodule

