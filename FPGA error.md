**[Synth 8-2543]** port connections cannot be mixed ordered and named ["D:/e203_FPGA/e203_7045/e203_fpga_proj.srcs/sources_1/new/clk_div_32k.v":46]

'''
BUFG clkout1_buf(
    .O  (clk32k     ),
    .I  (clk_32k    ),        //这里多加了一个逗号
);
'''
