# xcd语法

1. **管脚位置约束：** set_property PAKAGE_PIN “管脚编号” [get_ports “端口名称”]

2. **管脚电平约束：** set_property IOSTANDARD “电压” [get_ports “端口名称”]

3. **FPGAIO命名**：IO_LXXY#/IO_XX：其中：

   （1）  IO代表用户IO；

   （2） L代表差分，XX代表在当前BANK下的唯一标识号，Y=[P|N]表示LVDS信号的P或者N；

   （3）   #表示Bank号。

4. **GCLK**：global clock 全局时钟，FPGA的全局时钟应该是从晶振分出来的，最原始的频率。其他需要的各种频率都是在这个基础上利用PLL或者其他分频手段得到的    https://blog.csdn.net/Times_poem/article/details/51757227?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-51757227.pc_agg_new_rank&utm_term=cclk%E6%98%AF%E5%86%85%E9%83%A8%E7%9A%84%E5%90%97+fpga&spm=1000.2123.3001.4430

5. **RTC_CLK**：Real_Time Clock 实时时钟

6. **CCLK**：FPGA同步配置时钟。如果配置模式为主模式，则该时钟由FPGA器件生成，并输出；如果配置模式为从模式，则该时钟由外部提供

7. **EMC_CLK**：以太网介质访问控制模块时钟

8. **SRCC和MRCC**：SRCC可用于本时钟区域，MRCC用于本时钟区域和相邻时钟区域

9. **LVDS信号**：差分信号，P是正信号，N是负极性信号

10. **oscillator**：有源晶振

11. DCM、PLL、pMCD、MMCM

12. **PMU**：power Management Unit 电源管理单元

13. **UART**：通用异步收发传输器（Universal Asynchronous Receiver/Transmitter)UART是串口，但串口不一定是UART，它包含了UART

14. **openOCD**：

15. **create_clock**: create_clock -name clk16M -period 62.500 [get_pins ip/port] 或者是get_ports

16. **set_false_path**: set_false_path -from [get_clocks clk16M] -to [get_clocks clk32K]
17. **分频时钟输出缓冲**: BUFG clkout1_buf(
    .O  (clk32k     ),
    .I  (clk_32k    ),
);
18. 
