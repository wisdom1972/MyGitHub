# My share Efinix FPGA company
## 神秘的FPGA公司Efinix

__Emphasis__

在过去你听过很多公司的产品

１．　我的而第一项
２．　我的__第二项目__

偶尔我们需要重视的是： **你必须学习网络的开源分享方式**
那些还采用自闭的方式是： ~~等于自杀~~

..*  it is my 2nd title list

3. it is very important

* 你应该相信

I want to write a open source document based on Mark down format

insert the link 

## Links

There are two ways to creat links

第一种方法，　是在显示的内容上，　嵌入链接
[the Efinix company webiste is ](http://efinixinc.com)

第二种方法，　就是直接将链接写在正文里
maybe we want to use the link directly www.efinixinc.com

## Images

 my document have the following imag

 Here's our logo (hover to see the title text):

Inline-style: 
![alt text](./docx_imag/PB190024.jpg "Logo Title Text 1")

Reference-style: 
![alt text][logo]

## 代码说明

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

##Power Benchmark

We will list the power as follows

| Vendor        | FPGA           | Power  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |


Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

> 请注意，　这个是我们引用其他的文档
> 这些数据具有参考意义

文档参考

## 水平分隔线使用


　
Three or more...

---

Hyphens

***

Asterisks

___

Underscores

## 自然换行的段落

我们描述第一段文字

我们描述第二段文字

我们描述第三段文字


## Youku 视频的引用




[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](https://v.youku.com/v_show/id_XNDQxOTY0NDI3Ng==.html?spm=a2h0k.11417342.soresults.dtitle)

<a href="https://v.youku.com/v_show/id_XNDQxOTY0NDI3Ng==.html?spm=a2h0k.11417342.soresults.dtitle" target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg" 
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>