# My share Efinix FPGA company
## 神秘的FPGA公司Efinix

__Emphasis__

在过去你听过很多公司的产品

１．　我的而第一项
２．　我的ｄｉｅｒｘｉａｎｇ
..*  it is my 2nd title list

3. it is very important

* 你应该相信

I want to write a open source document based on Mark down format

insert the link 

## Links

There are two ways to creat links
[the Efinix company webiste is ](http://efinixinc.com)

maybe we want to use the link directly www.efinixinc.com

# Images

 my document have the following imag

 Here's our logo (hover to see the title text):

Inline-style: 
![alt text](./docx_imag/PB190024.jpg "Logo Title Text 1")

Reference-style: 
![alt text][logo]

```verilog


/////////////////////////////////////////////////////////////////////////////
//
// 
// 1.0 Updated to use the Trion T8 board
// *******************************
/////////////////////////////////////////////////////////////////////////////

module helloworld (led, clk, rstn, reverse);
   parameter DELAY_SIZE = 9;
   parameter WIDTH = 5;
   
   /**
	* DWIDTH is the width of the LED outputs.
	* NUM_PATTERNS is 2xWIDTH.
	* AWIDTH is log2(NUM_PATTERNS).
	* MAX_COUNT is the value where the pattern counter should reset
	**/
   localparam DWIDTH = WIDTH;
   localparam AWIDTH = 4;
   localparam NUM_PATTERNS = 2 * WIDTH;
   localparam MAX_COUNT = NUM_PATTERNS * (2 ** DELAY_SIZE);
   localparam COUNTER_SIZE = AWIDTH + DELAY_SIZE;
         
   output [DWIDTH-1:0] led;
   input 			  clk, rstn, reverse;
		
   wire [AWIDTH-1:0] raddr;
   reg [DWIDTH-1:0]  rdata;

   reg [DWIDTH-1:0]  mem [NUM_PATTERNS-1:0];
   reg [COUNTER_SIZE-1:0] counter = 0;

   integer 			 i;
   reg [DWIDTH-1:0]  init_data;
   initial begin
	  //$monitor("led=%b, raddr=%d, counter=%d", led, raddr, counter);
	  
	  // Initialize the memory with the LED pattern
	  init_data = 0;
      for (i=0;i<NUM_PATTERNS;i=i+1) begin
		 mem[i] = init_data;
		 init_data = {init_data[DWIDTH-2:0], ~init_data[DWIDTH-1]};
	  end
   end 

   // LED ROM
   always @(posedge clk) begin
	  rdata <= mem[raddr];
   end

   // Use the high-order counter bits as the ROM address
   assign raddr = counter[COUNTER_SIZE-1:DELAY_SIZE];

   // Delay counter
   always @(posedge clk) begin
	  if(~rstn)
		counter <= 0;
	  else if(reverse)
		if (counter == 0) counter <= MAX_COUNT-1;
		else counter <= counter - 1;
	  else
		if (counter == MAX_COUNT-1) counter <= 0;
		else counter <= counter + 1;
   end

   // Assign LED values, invert since low enables LED
   assign led = ~rdata;

endmodule // helloworld


```
    