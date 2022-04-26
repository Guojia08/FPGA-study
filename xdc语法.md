# xcd语法

1. **管脚位置约束: ** set_property PAKAGE_PIN “管脚编号” [get_ports “端口名称”]

2. **管脚电平约束: ** set_property IOSTANDARD “电压” [get_ports “端口名称”]

3. **FPGAIO命名**: IO_LXXY#/IO_XX: 其中: 

   （1）  IO代表用户IO；

   （2） L代表差分, XX代表在当前BANK下的唯一标识号, Y=[P|N]表示LVDS信号的P或者N；

   （3）   #表示Bank号。

4. **GCLK**: global clock 全局时钟, FPGA的全局时钟应该是从晶振分出来的, 最原始的频率。其他需要的各种频率都是在这个基础上利用PLL或者其他分频手段得到的    https://blog.csdn.net/Times_poem/article/details/51757227?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-51757227.pc_agg_new_rank&utm_term=cclk%E6%98%AF%E5%86%85%E9%83%A8%E7%9A%84%E5%90%97+fpga&spm=1000.2123.3001.4430

5. **RTC_CLK**: Real_Time Clock 实时时钟

6. **CCLK**: FPGA同步配置时钟。如果配置模式为主模式, 则该时钟由FPGA器件生成, 并输出；如果配置模式为从模式, 则该时钟由外部提供

7. **EMC_CLK**: 以太网介质访问控制模块时钟

8. **SRCC和MRCC**: SRCC可用于本时钟区域, MRCC用于本时钟区域和相邻时钟区域

9. **LVDS信号**: 差分信号, P是正信号, N是负极性信号

10. **oscillator**: 有源晶振

11. **DCM、PLL、pMCD、MMCM**: 

12. **PMU**: power Management Unit 电源管理单元

13. **UART**: 通用异步收发传输器（Universal Asynchronous Receiver/Transmitter)UART是串口, 但串口不一定是UART, 它包含了UART

14. **openOCD**: 

15. **create_clock**: create_clock -name clk16M -period 62.500 [get_pins ip/port] 或者是get_ports

16. **set_false_path**: set_false_path -from [get_clocks clk16M] -to [get_clocks clk32K]
17. **分频时钟输出缓冲**: 
```
BUFG clkout1_buf(
    .O  (clk32k     ),
    .I  (clk_32k    ),
);
```
18. **MIG ip 中的各个时钟**: 理解MIG ip时钟, 将设计分为三个部分, 会更好理解一点:我们自己设计的电路, MIG ip, DDR硬件;
    1. **clock period**: MIG ip 和 DDR3 的接口时钟频率, 或许可以理解为DDR的内部工作时钟; 
    2. **PHY to Controler CLock Ratio**: 4:1 or 2:1, 应该是指ui_clk, 这个是MIG输出时钟, 应该是给用户用的,有MIG☞user design,使用UI驱动可以避免跨时钟域的问题？
    3. **input clock period**: 输入时钟频率, 这是外界(user)输入给 MIG 核的时钟, IP核内部会自己调用pll和MMCM 来产生自己的工作时钟,应该是产生1中**clock period**,
    4. **system clock**: 是设置步骤3的输入时钟(**input clock period**)的属性, 可以是外部晶振（差分、单端）或者是PLL输出的时钟
    5. **Reference clock**: 是设置MIG的参考时钟, 这个时钟频率是固定的, 如果工作频率>666MHz, 参考时钟固定为300MHz/400MHz, 其他工作频率固定为200MHz, 这里设置时钟的属性, 可以是外部晶振(差分, 单端)或者是PLL输出的时钟; (在 FPGA options中), 注: 如果system clock的频率在199-201MHz之间, 这里会出现一个use system clock 的选项, 意思就是用系统时钟作为参考时钟

19. **MIG ip**: 带有AXI接口的, 使用收获: 
    1. 本次因为蜂鸟E203的系统总线上带有AXI接口, 为了使E203能够访问DDR, 将DDR挂载在系统总线上, 因此使用了带有AXI接口的MIG ip, 当MIG ip生成后, 例化的端口中的**Memory interface ports**是MIG ip与DDR硬件之间的连接方式, 对于**Memory interface ports**中的端口仅需将其引出至top层即可, 相关的约束文件已经在ip生成过程中设置完成, **Application interface ports**中的app_sr_req, app_ref_req, app_zq_req我直接设为1’b0, aresetn为全局复位信号;
    2. MIG ip为控制接口，在进行仿真是，需要在模块中加入**模拟出来的DDR模块**, 以便进行读写, 模拟的ddr模块位于**open ip example design**, 单击ip核获得, 进入IP核自带的仿真历程后, 找到文件中的**ddr3_model.sv**和**ddr3_model_parameters.vh**, 将以上的两个文件导入自己工程中即可, 上述的两个文件是**DDR的仿真模型**

20. **github的使用**: 

21. **跨时钟域的处理**: 

22. **AXI 学习**
    1. **AWQoS/ARQoS**: Quality of Service, 位宽为4位，手册中并没有固定该信号的确切用途，但是建议将该信号用于优先级声明信号，值越高代表优先级越高。

23. **AXI Interconnect RTL ip**: 分清AXI接口的maste信号接口与slave信号接口，slave接口接收来自master端的信号, **注意: 并不是AXI接口上的master, 而是相对于AXI接口这个整体而言的master**, 在AXI接口这个整体的内部, 数据是从slave流向master

24. **vivado block design**: 暂时还没用
