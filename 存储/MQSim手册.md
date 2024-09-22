#MQSim：一个用于现代NVMe和SATA SSD的模拟器

在Linux中的用法

运行以下命令： ./MQSim -i <SSD Configuration File> -w <Workload Definition File> 

在Windows 1中的用法:

1.打开MQSim.slnMS Visual Studio 2017或更高版本中的解决方案文件。

2.将解决方案配置设置为Release（默认设置为Debug）。

3.编译解决方案。

4.运行生成的可执行文件（例如。，MQSim.exe)在命令行模式下或单击MS Visual Studio运行按钮。请指定包含1）SSD配置和2）工作负载定义的文件的路径。

命令行执行示例：MQSim.exe -i <SSD Configuration File> -w <Workload Definition File> 

##MQSim执行配置

您可以用XML格式指定首选SSD配置。如果命令行中指定的SSD配置文件不存在，MQSim将在指定路径中创建一个示例XML文件。以下是XML文件中可用配置参数的定义：

###主机

1. PCIe_Lane_Bandwidth:*每个通道的PCIe带宽（GB/s）。范围={所有正的双精度值}。

2. **PCIe_Lane_Count:***PCIe通道数。范围={所有正整数值}。

3. **SATA_Processing_Delay:**定义以纳秒为单位向SSD设备发送/接收SATA消息的聚合硬件和软件处理延迟。范围={所有正整数值}。

4. **Enable_ResponseTime_Logging:**用于启用响应时间日志记录的切换。如果启用，则计算模拟时段上每个运行的I/O流的响应时间，并在每个时段结束时在日志文件中报告。范围={true，false}。

5. **ResponseTime_Logging_Period_Length:**定义响应时间日志记录的纪元长度（以纳秒为单位）。范围={所有正整数值}。


   

###固态硬盘设备

1。**Seed:**用于随机数生成的种子值。范围={所有正整数值}。

2。**Enabled_Preconditioning:**启用预处理的切换。范围={true，false}。

3。**Memory_Type:**用于数据存储的非易失性内存的类型。范围={FLASH}。

4。**HostInterface_Type:**主机接口的类型。范围={NVME，SATA}。

5。**IO_Queue_Depth:**主机端I/O队列的长度。如果主机接口设置为NVME，则**I O_Queue_Depth**定义I/O提交和I/O完成队列的容量。如果主机接口设置为SATA，则**IO_Queue_Depth**定义本机命令队列（NCQ）的容量。范围={所有正整数值}

6。**Queue_Fetch_Size:**QueueFetchSize参数的值，如FAST 2018论文[1]所述。范围={所有正整数值}

7。**Caching_Mechanism:**设备上使用的数据缓存机制。范围={SIMPLE:实现一个简单的数据转储缓冲区，ADVANCED:实现一个高级数据缓存机制，在并发流之间具有不同的共享选项}。

8。**Data_Cache_Sharing_Mode:**使用NVMe主机接口时，并发运行的I/O流之间的DRAM数据缓存（缓冲区）的共享模式。范围={共享，相等分区}。

9。**Data_Cache_Capacity:**DRAM数据缓存的大小（字节）。范围={所有正整数}

10。**Data_Cache_DRAM_Row_Size：**DRAM行的大小（字节）。范围={2所有正幂,例如 2,4,8,16等等}。

11。**Data_Cache_DRAM_Data_Rate:**以MB/s 我觉得是他文档里写错了为单位的DRAM数据传输速率。范围={所有正整数值}。

12。**Data_Cache_DRAM_Data_Burst_Size：**在一个DRAM突发中传输的字节数（取决于DRAM芯片的数量）。范围={所有正整数值}。

13。**Data_Cache_DRAM_tRCD:**用于访问数据缓存中DRAM的计时参数tRCD的值（以纳秒为单位）。范围={所有正整数值}。

14。**Data_Cache_DRAM_tCL**用于访问数据缓存中DRAM的计时参数tCL的值（以纳秒为单位）。范围={所有正整数值}

15。**Data_Cache_DRAM_tRP:**用于访问数据缓存中DRAM的计时参数tRP的值（以纳秒为单位）。范围={所有正整数值}。

19。**CMT_Sharing_Mode:**当使用NVMe主机接口时，决定如何在并发运行的流之间共享整个CMT（缓存映射表）空间的模式。范围={共享，相等分区}。

20。**Plane_Allocation_Scheme：**Tavakkol等人定义的分组分配方案。[3]。范围={CWDP，CWDP，CDWP，CDPW，CPWD，CPDW，WCDP，WCPD，WDCP，WDPC，WPCD，WPDC，DCWP，DCPW，DWCP，DWPC，DPCW，DPWC，PCWD，PCDW，PWCD，PWDC，PDCW

，PDWC}

21。**Transaction_Scheduling_Policy**SSD后端中使用的事务调度策略。范围={OUT_OF_ORDER 如Sprinkler 论文 [2]中所定义，PRIORITY_OUT_OF_ORDER实现OUT_OF_ORDER 和NVM优先级}。

22。**Overprovisioning_Ratio:**保留存储空间与可用闪存存储容量的比率。范围={所有正的双精度值}。

23。**GC_Exect_Threshold:*：**启动垃圾收集（GC）的阈值。当一个分组空闲物理页比率低于此阈值时，垃圾回收执行开始。范围={所有正的双精度值}。

24。**GC_Block_Selection_Policy:：**垃圾回收块选择策略。范围{GREEDY，RGA*（在[4]和[5]中描述），RANDOM*（在[4]中描述），RANDOM_P*（在[4]中描述），RANDOM_PP*（在[4]中描述），FIFO*（在[6]中描述）。

25。**Use_Copyback_for_GC:*”用于“GC_and_WL_Unit_Page_Level”中，以确定块管理器→是否是有效的GC_写入事务

26。**Preemptible_GC_Enabled：**用于启用可抢占垃圾回收的切换（如[7]所述）。范围={true，false}。

27。**GC_Hard_Threshold:**停止可先发制人的垃圾回收执行的阈值（如[7]所述）。范围={所有可能的正双精度值小于GC_Exect_Threshold}。

28。**Dynamic_Wearleveling_Enabled：**用于启用动态磨损均衡的切换（如[9]所述）。范围={true，false}。

29。**Static_Wearleveling_Enabled：**启用静态磨损均衡的开关（如[9]所述）。范围={所有正整数值}。

30。**Static_Wearleveling_Threshold：**启动静态磨损平衡的阈值（如[9]所述）。当存储器单元（例如，闪存中的分组 ）内的最小和最大擦除计数之间的差异降到该阈值以下时，静态磨损水平开始。范围={true，false}。

31。**Preferred_suspend_erase_time_for_read：:*暂停正在进行的闪存擦除操作的合理时间，以支持最近排队的读取操作。范围={所有正整数值}。

32。**Preferred_suspend_erase_time_for_write:*暂停正在进行的闪存擦除操作的合理时间，以支持最近排队的读取操作。范围={所有正整数值}。

33。**Preferred_suspend_write_time_for_read:*暂停正在进行的闪存擦除操作的合理时间，以支持最近排队的程序操作。范围={所有正整数值}。

34。**Flash_Channel_Count:**固态硬盘后端的闪存通道数。范围={所有正整数值}。

35。**Flash_Channel_Width::**每个闪存通道的宽度（字节）。范围={所有正整数值}。

36。**Channel_Transfer_Rate:：**以MT/s为单位的固态硬盘后端中闪存通道的传输速率。范围={所有正整数值}。

37。**Chip_No_Per_Channel:**连接到SSD后端每个通道的闪存芯片数。范围={所有正整数值}。

38。**Flash_Comm_Protocol：**开放式NAND Flash接口（ONFI）协议，用于通过SSD后端的闪存通道传输数据。范围={NVDDR2}。

![手册 | MQSim翻译（原文件GitHub上有）[1]](https://vlambda.com/img?url=https://mmbiz.qpic.cn/mmbiz_gif/0oHHdKjH8xBvIVSkYGBwHc64HW08czbRReHjuQcZRu1kFibjuR9lQXJmLSAosYkHmJTVqYXIYhEDrkROyQdIqdw/640?wx_fmt=gif)

   

###NAND闪存

1。**Flash_Technology:**范围={SLC，MLC，TLC}。

2。**CMD_Suspension_Support::**闪存芯片支持的暂停支持命令类型。范围={NONE, PROGRAM, PROGRAM_ERASE, ERASE}。

3。**Page_Read_Latency_LSB:*读取闪存单元的LSB位的延迟（纳秒）。范围={所有正整数值}。

4。**Page_Read_Latency_CSB:*以纳秒为单位读取闪存单元的CSB位的延迟。范围={所有正整数值}。

5。**Page_Read_Latency_MSB::*以纳秒为单位读取闪存单元的MSB位的延迟。范围={所有正整数值}。

6。**Page_Program_Latency_LSB:*编程闪存单元LSB位的延迟（纳秒）。范围={所有正整数值}。

7。**Page_Program_Latency_CSB:*以纳秒为单位编程闪存单元的CSB位的延迟。范围={所有正整数值}。

8。**Page_Program_Latency_MSB:*以纳秒为单位编程闪存单元的MSB位的延迟。范围={所有正整数值}。

9。**Block_Erase_Latency:*擦除闪存块的延迟（纳秒）。范围={所有正整数值}。

10。**Block_PE_Cycles_Limit：:**每个闪存块的PE限制。范围={所有正整数值}。

11。**Suspend_Erase_Time：**以纳秒为单位暂停正在进行的擦除操作所用的时间。范围={所有正整数值}。

12。**Suspend_Program_Time：**以纳秒为单位暂停正在进行的程序操作所用的时间。范围={所有正整数值}。

13。**Die_No_Per_Chip:*每个闪存芯片中的晶圆数。范围={所有正整数值}。

14。**Plane_No_Per_Die：**每个模具中的分组数。范围={所有正整数值}。

15。**Block_No_Per_Plane：**每个平面中的闪存块数。范围={所有正整数值}。

16。**Page_No_Per_Block：**每个闪存块中的物理页数。范围={所有正整数值}。

17。**Page_Capacity：**每个物理闪存页面的大小（字节）。范围={所有正整数值}。

18。**Page_Metadat_Capacity:**每个物理闪存页的元数据区域大小（字节）。范围={所有正整数值}。  

   

##MQSim工作负载定义

您可以用XML格式定义您首选的工作负载集。如果指定的工作负载定义文件不存在，MQSim将为您创建一个XML格式的示例工作负载定义文件（即工作负载.xml). 以下是对工作负载定义文件的XML属性和标记的说明：

一。整个工作负载定义应该嵌入到<MQSim_IO_Scenarios></MQSim_IO_Scenarios> 标记中。您可以在这些标记中定义不同的*I/O方案集*。MQSim分别模拟每个I/O场景。

2。我们调用一组应该一起执行的工作负载，一个*I/O场景*。I/O场景在<IO_Scenario></IO_Scenario>标记中定义。例如，工作负载定义文件中按以下方式定义了两种不同的I/O方案：```

```

<MQSim_IO_Scenarios>

<IO_Scenario>

.............

</IO_Scenario>

<IO_Scenario>

.............

</IO_Scenario>

</MQSim_IO_Scenarios>

```

对于每个I/O方案，MQSim 1）重建主机和SSD驱动器模型并执行方案直至完成，2）创建输出文件并将仿真结果写入其中。对于上面提到的示例，MQSim两次构建主机和SSD驱动器模型，执行第一个和第二个I/O方案，最后分别将执行结果写入workload_scenario_1.xml和workload_scenario_2.xml文件。

您可以在每个 IO_Scenario标记中定义多达8个不同的工作负载。每个工作负载可以是已在实际系统上收集的磁盘跟踪文件，也可以是由MQSim的请求生成器生成的I/O请求的合成流。

###定义基于采集(trace)的工作负载

您可以使用XML标记<IO_Flow_Parameter_Set_Trace_Based>为MQSim定义基于跟踪的工作负载。目前，MQSim可以执行[8]中定义的ASCII磁盘跟踪，其中跟踪文件的每一行具有以下格式：

以下参数用于定义基于跟踪的工作负载：  

  

1。**Priority_Class:**与此I/O请求关联的I/O队列的优先级。范围={紧急，高，中，低}。  

2。**Device_Level_Data_Caching_Mode:**此流的设备上数据缓存类型。范围={写缓存，读缓存，写读缓存，关闭}。如果将上面提到的缓存机制设置为简单，则只能使用写缓存和关闭模式。 

3。**Channel_IDs：**分配给此工作负载的通道标识的逗号分隔列表。此列表用于资源分区。如果SSD中有C个通道（在SSD配置文件中定义），那么通道ID列表应该包含0到C-1范围内的值。如果不需要资源分区，那么所有工作负载都应该具有通道ID 0到C-1。

4。**Chip_IDs:**分配给此工作负载的以逗号分隔的Chip id列表。此列表用于资源分区。如果每个通道中都有W个芯片（在SSD配置文件中定义），那么芯片ID列表应该包含0到W-1范围内的值。如果不需要资源分区，那么所有工作负载都应该具有芯片ID 0到W-1。

5。**Die_IDs:**分配给此工作负载的芯片id的逗号分隔列表。此列表用于资源分区。如果每个闪存芯片（在SSD配置文件中定义）中都有D个晶圆 ，那么晶圆 ID列表应该包含0到D-1范围内的值。如果不需要资源分区，那么所有工作负载都应该具有晶圆 id 0到D-1。

6。**Plane_IDs:**分配给此工作负载的分组 ID的逗号分隔列表。此列表用于资源分区。如果每个晶圆中都有P个分组（在SSD配置文件中定义），那么分组ID列表应该包含0到P-1范围内的值。如果不需要资源分区，那么所有工作负载都应该具有分组id 0到P-1。

7。**Initial_Occupancy_Percentage：**预处理期间填充的存储空间（即逻辑页）的百分比。范围={1到100}范围内的所有整数值。

8。**File_Path：**输入跟踪文件的相对/绝对路径。

9。**Percentage_To_Be_Executed:**应执行的输入跟踪文件中请求的百分比。范围={1到100}范围内的所有整数值。

10。**Relay_Count:**应重复跟踪执行的次数。范围={所有正整数值}。

11。**Time_Unit：**输入跟踪文件中的到达时间单位。范围={皮秒，纳秒，微秒}