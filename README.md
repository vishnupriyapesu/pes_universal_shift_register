# pes_universal_shift_register

> top level module
module univshift(in,cnt,clk,rst,q);
  input clk,rst;
  input [3:0]in;
  input [1:0]cnt;
  output reg [3:0]q;
  
  always @(posedge clk,posedge rst)
    begin
      if(rst)
        q<=0;
      else
        begin
          case(cnt)
            2'b00: q<=q;
              
            2'b01: q<={1'b0,q[3:1]};
              
            2'b10: q<={q[2:0],1'b0};
              
            2'b11: q<= in;
              
              default: q<=0;
          endcase
        end
    end
endmodule
> test bench

module univshifttb();
  reg clk,rst;
  reg [3:0] in;
  reg [1:0] cnt;
  wire [3:0] q;
  
  univshift dut(in,cnt,clk,rst,q);
  initial 
    begin
      clk = 1;
      forever #5 clk = ~clk;
    end
  
  initial 
    begin
      $dumpfile("univshifttb.vcd");
      $dumpvars(0,univshifttb);
      #200 $finish;
    end
  
  initial
    begin
      rst=1;
      #10 rst=0; cnt=2'b11; in=4'b1100;
      #10 cnt=2'b00;
      #10 cnt=2'b01;
      #30 cnt=2'b11; in=4'b1100;
      #10 cnt=2'b10;
      #30 rst=1;
    end
endmodule
