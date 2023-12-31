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
// Testbench
module test;

  reg clk;
  reg shift;
  reg [7:0] sr_in;;
  wire [7:0] sr_tap_one;
  wire [7:0] sr_tap_two;
  wire [7:0]sr_tap_three;
  wire [7:0] sr_out;
  
  // Instantiate design under test
  shift_8x64_taps test(clk, shift, sr_in, 
                  sr_out, sr_tap_one, sr_tap_two, sr_tap_three); 
          
  initial begin
    // Dump waves
    $dumpfile("dump.vcd");
    $dumpvars(1);
    
    $display("shift.");
    clk = 0;
    shift = 1;
    sr_in = 1'bx;
    display;
    
    $display("Release shift.");
    sr_in = 1;
    shift = 0;
    display;

    $display("Toggle clk.");
    clk = 1;
    display;
    
  end
  
  task display;
    #1 $display("shift:%0h, sr_in:%0h, sr_out:%0h, sr_tap_one:%0h,   sr_tap_two:%0h, sr_tap_three:%0h",
      shift, sr_in, sr_out, sr_tap_one, sr_tap_two, sr_tap_three);
  endtask

endmodule

