**[Synth 8-2543]** port connections cannot be mixed ordered and named ["D:/e203_FPGA/e203_7045/e203_fpga_proj.srcs/sources_1/new/clk_div_32k.v":46]
**解决办法**
'''
BUFG clkout1_buf(
    .O  (clk32k     ),
    .I  (clk_32k    ),        //这里多加了一个逗号
);
'''
**[Synth 8-1766]** cannot open include file e203_defines.v ["D:/e203_FPGA/e203_7045/e203_fpga_proj.srcs/sources_1/imports/e203_hbirdv2-master/rtl/e203/subsys/e203_subsys_clint.v":28]
**解决办法** 在Synthesis OpTIons对话框下，找到MoreOpTIon选项，手动输入"-include_dirs"选项 后面加路径，我添加的绝对路径，是可以使用的，相对路径还没试过

