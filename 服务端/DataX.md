### 1.基本介绍

> DataX是一种**异构数据源离线同步工具**，致力于实现包括关系型数据库(MySQL、Oracle等)、HDFS、Hive、ODPS、HBase、FTP等各种异构数据源之间稳定高效的数据同步功能。



![图片 1.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/df84221dc1624975977f15d48f38e062~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

DataX目前支持的数据源：



![图片2.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/344b8209d1984067ab5f6d6fbb3f5fd2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)



**当前使用现状**

​	DataX在阿里巴巴集团内被广泛使用，承担了所有大数据的离线同步业务，并已持续稳定运行了6年之久。目前每天完成同步8w多道作业，每日传输数据量超过300TB。

​	此前已经开源DataX1.0版本，此次介绍为阿里云开源全新版本DataX3.0，有了更多更强大的功能和更好的使用体验。Github主页地址：<https://github.com/alibaba/DataX>

### 2.名词解释

- 异构数据源：指不同数据库管理系统之间的数据。因为业务、使用场景、数据量大小，不同需求等因素影响，导致企业会用多种不同的存储方式的业务数据库，包括采用的数据管理系统也大不相同，从简单的文件数据库到复杂的网络数据库，他们构成了企业的异构数据源。

- 离线：在线、离线通常指某一功能或工作（如检测，控制，给定）是在系统运行之中进行的，还是在系统运行之前（或之后）进行。          

  ​	离线通常是生产过程不和计算机相连，且不受计算机控制，而是靠人进行联系并作出相应操作的方式称为离线或者脱机方式

  ​

### 3.DataX3.0框架设计

  ![img](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/a8ff0f3c4e2e977f6804ba51424dc0f3&showdoc=.jpg)



DataX本身作为离线数据同步框架，采用Framework + plugin架构构建。将数据源读取和写入抽象成为            Reader/Writer插件，纳入到整个同步框架中。

  Reader：Reader为数据采集模块，负责采集数据源的数据，将数据发送给Framework。
  Writer： Writer为数据写入模块，负责不断向Framework取数据，并将数据写入到目的端。
  Framework：Framework用于连接reader和writer，作为两者的数据传输通道，并处理缓冲，流控，并发，数据转换等核心技术问题。

### 4. DataX3.0核心架构

  ![img](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/adbcd65c1ffeecf1cf4da22ad3c7547e&showdoc=.jpg)

  - **核心模块介绍**：

    1. DataX完成单个数据同步的作业，我们称之为Job，DataX接受到一个Job之后，将启动一个进程来完成整个作业同步过程。DataX Job模块是单个作业的中枢管理节点，承担了数据清理、子任务切分(将单一作业计算转化为多个子Task)、TaskGroup管理等功能。

    2. DataXJob启动后，会根据不同的源端切分策略，将Job切分成多个小的Task(子任务)，以便于并发执行。Task便是DataX作业的最小单元，每一个Task都会负责一部分数据的同步工作。

    3. 切分多个Task之后，DataX Job会调用Scheduler模块，根据配置的并发数据量，将拆分成的Task重新组合，组装成TaskGroup(任务组)。每一个TaskGroup负责以一定的并发运行完毕分配好的所有Task，默认单个任务组的并发数量为5。

    4. 每一个Task都由TaskGroup负责启动，Task启动后，会固定启动Reader—>Channel—>Writer的线程来完成任务同步工作。

    5. DataX作业运行起来之后， Job监控并等待多个TaskGroup模块任务完成，等待所有TaskGroup任务完成后Job成功退出。否则，异常退出，进程退出值非0

       ​

- **DataX调度流程**：

    举例来说，用户提交了一个DataX作业，并且配置了20个并发，目的是将一个100张分表的mysql数据同步到odps里面。 DataX的调度决策思路是：

    1. DataXJob根据分库分表切分成了100个Task。
    2. 根据20个并发，DataX计算共需要分配4个TaskGroup。
    3. 4个TaskGroup平分切分好的100个Task，每一个TaskGroup负责以5个并发共计运行25个Task。

  ### 5. DataX 3.0核心优势

- ##### 可靠的数据质量监控

  - 完美解决数据传输个别类型失真问题

    DataX旧版对于部分数据类型(比如时间戳)传输一直存在毫秒阶段等数据失真情况，新版本DataX3.0已经做到支持所有的强数据类型，每一种插件都有自己的数据类型转换策略，让数据可以完整无损的传输到目的端。

  - 提供作业全链路的流量、数据量运行时监控

    DataX3.0运行过程中可以将作业本身状态、数据流量、数据速度、执行进度等信息进行全面的展示，让用户可以实时了解作业状态。并可在作业执行过程中智能判断源端和目的端的速度对比情况，给予用户更多性能排查信息。

  - 提供脏数据探测

    在大量数据的传输过程中，必定会由于各种原因导致很多数据传输报错(比如类型转换错误)，这种数据DataX认为就是脏数据。DataX目前可以实现脏数据精确过滤、识别、采集、展示，为用户提供多种的脏数据处理模式，让用户准确把控数据质量大关！

- ##### 丰富的数据转换功能

  DataX作为一个服务于大数据的ETL工具，除了提供数据快照搬迁功能之外，还提供了丰富数据转换的功能，让数据在传输过程中可以轻松完成数据脱敏，补全，过滤等数据转换功能，另外还提供了自动groovy函数，让用户自定义转换函数。详情请看DataX3的transformer详细介绍。

- ##### 精准的速度控制

  还在为同步过程对在线存储压力影响而担心吗？新版本DataX3.0提供了包括通道(并发)、记录流、字节流三种流控模式，可以随意控制你的作业速度，让你的作业在库可以承受的范围内达到最佳的同步速度。

  ```
  "speed": {
     "channel": 5,
     "byte": 1048576,
     "record": 10000
  }
  ```

- ##### 强劲的同步性能

  DataX3.0每一种读插件都有一种或多种切分策略，都能将作业合理切分成多个Task并行执行，单机多线程执行模型可以让DataX速度随并发成线性增长。在源端和目的端性能都足够的情况下，单个作业一定可以打满网卡。另外，DataX团队对所有的已经接入的插件都做了极致的性能优化，并且做了完整的性能测试。性能测试相关详情可以参照每单个数据源的详细介绍：[DataX数据源指南](https://github.com/alibaba/DataX/wiki/DataX-all-data-channels)

- ##### 健壮的容错机制

  DataX作业是极易受外部因素的干扰，网络闪断、数据源不稳定等因素很容易让同步到一半的作业报错停止。因此稳定性是DataX的基本要求，在DataX 3.0的设计中，重点完善了框架和插件的稳定性。目前DataX3.0可以做到线程级别、进程级别(暂时未开放)、作业级别多层次局部/全局的重试，保证用户的作业稳定运行。

  - 线程内部重试

    DataX的核心插件都经过团队的全盘review，不同的网络交互方式都有不同的重试策略。

  - 线程级别重试

    目前DataX已经可以实现TaskFailover，针对于中间失败的Task，DataX框架可以做到整个Task级别的重新调度。

- ##### 极简的使用体验

### 6.主要特点

- 实现了不同的数据源之间的数据高效同步
- 全内存操作，不读写磁盘，因此效率高
- 框架开放性和可扩展性强，能够让开发者在短时间内开发出新的插件来支持新的数据源
- 采用Framework+plugin架构构建，Framework主要负责缓冲，流控，并发，监控，统计，上下文加载高速高效的数据同步的大部分技术问题，提供了大量接口可以与插件进行交互，插件仅需实现对数据处理系统的访问
- 单进程，多线程的运行模式

### 7.使用场景

- 不同数据源之间需要进行数据同步时，DataX是首选
- DataX传输高效稳定，适用于数据的全量同步场景
- 不同的或相同的数据源数据量的大规模迁移

mongodb缺点

- 不支持事务
- 占用空间大
- 不能进行表关联
- 复杂聚合操作通过mapReduce创建，速度慢
- mongodb在你删除记录后不会在文件系统中回收空间，除非你删除数据库





### datax-web

> datax-web 是在datax之上开发的分布式数据同步工具，提供简易的操作界面，降低用户使用datax的学习成本，缩短任务配置时间，避免配置过程中出错。



#### 1.特征

- 1、通过Web构建datax Json
- 2、datax Json保存在数据库中，方便任务的迁移，管理
- 3、web实时查看抽取日志，类似Jenkins的日志控制台输出功能
- 4、datax运行记录展示，可页面停止datax作业

- 5、支持DataX定时任务，支持动态修改任务状态、启动/停止任务，以及终止运行中任务，即时生效；
- 6、调度采用中心式设计，支持集群部署；
- 7、任务分布式执行，任务"执行器"支持集群部署；
- 8、执行器会周期性自动注册任务, 调度中心将会自动发现注册的任务并触发执行；
- 9、**路由策略**：执行器集群部署时提供丰富的路由策略，包括：第一个、最后一个、轮询、随机、一致性HASH、最不经常使用、最近最久未使用、故障转移、忙碌转移等；
- 10、**阻塞处理策略**：调度过于密集执行器来不及处理时的处理策略，策略包括：单机串行（默认）、丢弃后续调度、覆盖之前调度；
- 11、任务超时控制：支持自定义任务超时时间，任务运行超时将会主动中断任务；
- 12、任务失败重试：支持自定义任务失败重试次数，当任务失败时将会按照预设的失败重试次数主动进行重试；
- 13、任务失败告警；默认提供邮件方式失败告警，同时预留扩展接口，可方便的扩展短信、钉钉等告警方式；
- 14、用户管理：支持在线管理系统用户，存在管理员、普通用户两种角色；
- 15、**任务依赖**：支持配置子任务依赖，当父任务执行结束且执行成功后将会主动触发一次子任务的执行, 多个子任务用逗号分隔；
- 16、运行报表：支持实时查看运行数据，以及调度报表，如调度日期分布图，调度成功分布图等；
- 17、**指定增量字段**，配置定时任务自动获取每次的数据区间，任务失败重试，保证数据安全；
- 18、页面可配置DataX启动JVM参数；
- 19、数据源配置成功后添加手动测试功能；
- 20、可以对常用任务进行配置模板，在构建完JSON之后可选择关联模板创建任务；
- 21、jdbc添加hive数据源支持，可在构建JSON页面选择数据源生成column信息并简化配置；
- 22、优先通过环境变量获取DataX文件目录，集群部署时不用指定JSON及日志目录；
- 23、通过动态参数配置指定hive分区，也可以配合增量实现增量数据动态插入分区；
- 24、任务类型由原来DataX任务扩展到Shell任务、Python任务、PowerShell任务；
- 25、添加HBase数据源支持，JSON构建可通过HBase数据源获取hbaseConfig，column；
- 26、添加MongoDB数据源支持，用户仅需要选择collectionName即可完成json构建；
- 27、添加执行器CPU、内存、负载的监控页面；
- 28、添加24类插件DataX JSON配置样例
- 29、公共字段（创建时间，创建人，修改时间，修改者）插入或更新时自动填充
- 30、对swagger接口进行token验证
- 31、任务增加超时时间，对超时任务kill datax进程，可配合重试策略避免网络问题导致的datax卡死。
- 32、添加项目管理模块，可对任务分类管理；
- 33、对RDBMS数据源增加批量任务创建功能，选择数据源，表即可根据模板批量生成DataX同步任务；
- 34、JSON构建增加ClickHouse数据源支持；
- 35、执行器CPU.内存.负载的监控页面图形化；
- 36、RDBMS数据源增量抽取增加主键自增方式并优化页面参数配置；
- 37、更换MongoDB数据源连接方式,重构HBase数据源JSON构建模块；
- 38、脚本类型任务增加停止功能；
- 39、rdbms json构建增加postSql，并支持构建多个preSql，postSql；
- 40、数据源信息加密算法修改及代码优化；
- 41、日志页面增加DataX执行结果统计数据；



阻塞处理策略：调度过于密集执行器来不及处理时的处理策略；

- 单机串行：调度请求进入单机执行器后，调度请求进入FIFO队列并以串行方式运行；
- 丢弃后续调度：调度请求进入单机执行器后，发现执行器存在运行的调度任务，本次请求将会被丢弃并标记为失败；
- 覆盖之前调度：调度请求进入单机执行器后，发现执行器存在运行的调度任务，将会终止运行中的调度任务并清空队列，然后运行本地调度任务；

### 官方文档

datax:

https://github.com/alibaba/DataX/blob/master/introduction.md



datax-web:

https://gitee.com/WeiYe-Jing/datax-web