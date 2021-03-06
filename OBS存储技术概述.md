##  对象存储（Object-based Storage） 概述  
什么是对象存储？多次在不同场合被问起这个问题，于是就想写篇小综述文章。网上查找资料时，找到几篇不错的资料，简单整理一下，供自己和大家参考。

**什么是对象存储（OSD）？**  
存储局域网(SAN)和网络附加存储(NAS)是目前两种主流网络存储架构，而对象存储（Object-based Storage）是一种新的网络存储架构，基于对象存储技术的设备就是对象存储设备（Object-based Storage Device）简称OSD。1999年成立的全球网络存储工业协会(SNIA)的对象存储设备(Object Storage Device)工作组发布了ANSI的X3T10标准。总体上来讲，对象存储（Object-Based Storage, OBS)综合了NAS和SAN的优点，同时具有SAN的高速直接访问和NAS的分布式数据共享等优势，提供了具有高性能、高可靠性、跨平台以及安全的数据共享的存储体系结构。

**SAN存储架构**  
采用SCSI 块I/O的命令集，通过在磁盘或FC（Fiber Channel）级的数据访问提供高性能的随机I/O和数据吞吐率，它具有高带宽、低延迟的优势，在高性能计算中占有一席之地，如SGI的CXFS文件系统就是基于SAN实现高性能文件存储的，但是由于SAN系统的价格较高，且可扩展性较差，已不能满足成千上万个CPU规模的系统。 
 
**NAS存储架构**  
它采用NFS或CIFS命令集访问数据，以文件为传输协议，通过TCP/IP实现网络化存储，可扩展性好、价格便宜、用户易管理，如目前在集群计算中应用较多的NFS文件系统，但由于NAS的协议开销高、带宽低、延迟大，不利于在高性能集群中应用。
 
**对象存储架构**  
核心是将数据通路（数据读或写）和控制通路（元数据）分离，并且基于对象存储设备（Object-based Storage Device，OSD）构建存储系统，每个对象存储设备具有一定的智能，能够自动管理其上的数据分布。对象存储结构由对象、对象存储设备、元数据服务器、对象存储系统的客户端四部分组成。
![avatar](https://img-my.csdn.net/uploads/201401/07/1389108549_1299.png)


 **[DAN, NAS和SAN存储技术介绍和优缺点分析](https://zhuanlan.zhihu.com/p/36106323)**


**1、对象**  
 
对象是系统中数据存储的基本单位，每个Object是数据和数据属性集的综合体，数据属性可以根据应用的需求进行设置，包括数据分布、服务质量等。在传统的存储系统中用文件或块作为基本的存储单位，块设备要记录每个存储数据块在设备上的位置。Object维护自己的属性，从而简化了存储系统的管理任务，增加了灵活性。Object的大小可以不同，可以包含整个数据结构，如文件、数据库表项等。在存储设备中，所有对象都有一个对象标识，通过对象标识OSD命令访问该对象。通常有多种类型的对象，存储设备上的根对象标识存储设备和该设备的各种属性，组对象是存储设备上共享资源管理策略的对象集合等。


![avatar](https://img-my.csdn.net/uploads/201401/07/1389108549_8043.jpg)
传统块存储与对象存储

![avatar](https://img-my.csdn.net/uploads/201401/07/1389108549_2975.jpg)



 
**2、对象存储设备**  
 
每个OSD都是一个智能设备，具有自己的存储介质、处理器、内存以及网络系统等，负责管理本地的Object，是对象存储系统的核心。OSD同块设备的不同不在于存储介质，而在于两者提供的访问接口。OSD的主要功能包括数据存储和安全访问。目前国际上通常采用刀片式结构实现对象存储设备。OSD提供三个主要功能：
（1） 数据存储。OSD管理对象数据，并将它们放置在标准的磁盘系统上，OSD不提供块接口访问方式，Client请求数据时用对象ID、偏移进行数据读写。
（2） 智能分布。OSD用其自身的CPU和内存优化数据分布，并支持数据的预取。由于OSD可以智能地支持对象的预取，从而可以优化磁盘的性能。
（3） 每个对象元数据的管理。OSD管理存储在其上对象的元数据，该元数据与传统的inode元数据相似，通常包括对象的数据块和对象的长度。而在传统的NAS系统中，这些元数据是由文件服务器维护的，对象存储架构将系统中主要的元数据管理工作由OSD来完成，降低了Client的开销。
![avatar](https://img-my.csdn.net/uploads/201401/07/1389109760_5754.png)



 
**3、元数据服务器（Metadata Server，MDS）**  
 
MDS控制Client与OSD对象的交互，为客户端提供元数据，主要是文件的逻辑视图，包括文件与目录的组织关系、每个文件所对应的OSD等。主要提供以下几个功能：
（1） 对象存储访问。MDS构造、管理描述每个文件分布的视图，允许Client直接访问对象。MDS为Client提供访问该文件所含对象的能力，OSD在接收到每个请求时将先验证该能力，然后才可以访问。
（2） 文件和目录访问管理。MDS在存储系统上构建一个文件结构，包括限额控制、目录和文件的创建和删除、访问控制等。
（3） Client Cache一致性。为了提高Client性能，在对象存储系统设计时通常支持Client方的Cache。由于引入Client方的Cache，带来了Cache一致性问题，MDS支持基于Client的文件Cache，当Cache的文件发生改变时，将通知Client刷新Cache，从而防止Cache不一致引发的问题。
 
**4、对象存储系统的客户端Client**  
 
为了有效支持Client支持访问OSD上的对象，需要在计算节点实现对象存储系统的Client。现有的应用对数据的访问大部分都是通过POSIX文件方式进行的，对象存储系统提供给用户的也是标准的POSIX文件访问接口。接口具有和通用文件系统相同的访问方式，同时为了提高性能，也具有对数据的Cache功能和文件的条带功能。同时，文件系统必须维护不同客户端上Cache的一致性，保证文件系统的数据一致。文件系统读访问流程：
1)客户端应用发出读请求; 
2)文件系统向元数据服务器发送请求，获取要读取的数据所在的OSD; 
3)然后直接向每个OSD发送数据读取请求； 
4)OSD得到请求以后，判断要读取的Object，并根据此Object要求的认证方式，对客户端进行认证，如果此客户端得到授权，则将Object的数据返回给客户端；
5)文件系统收到OSD返回的数据以后，读操作完成。

**对象存储文件系统的关键技术**  
 
1、分布元数据 传统的存储结构元数据服务器通常提供两个主要功能。
（1）为计算结点提供一个存储数据的逻辑视图（Virtual File System，VFS层），文件名列表及目录结构。
（2）组织物理存储介质的数据分布（inode层）。对象存储结构将存储数据的逻辑视图与物理视图分开，并将负载分布，避免元数据服务器引起的瓶颈（如NAS系统）。元数据的VFS部分通常是元数据服务器的10％的负载，剩下的90％工作（inode部分）是在存储介质块的数据物理分布上完成的。在对象存储结构，inode工作分布到每个智能化的OSD，每个OSD负责管理数据分布和检索，这样90%的元数据管理工作分布到智能的存储设备，从而提高了系统元数据管理的性能。另外，分布的元数据管理，在增加更多的OSD到系统中时，可以同时增加元数据的性能和系统存储容量。
 
2、并发数据访问 对象存储体系结构定义了一个新的、更加智能化的磁盘接口OSD。OSD是与网络连接的设备，它自身包含存储介质，如磁盘或磁带，并具有足够的智能可以管理本地存储的数据。计算结点直接与OSD通信，访问它存储的数据，由于OSD具有智能，因此不需要文件服务器的介入。如果将文件系统的数据分布在多个OSD上，则聚合I/O速率和数据吞吐率将线性增长，对绝大多数Linux集群应用来说，持续的I/O聚合带宽和吞吐率对较多数目的计算结点是非常重要的。对象存储结构提供的性能是目前其它存储结构难以达到的，如ActiveScale对象存储文件系统的带宽可以达到10GB/s。

![avatar](https://pic4.zhimg.com/80/v2-62385fc5eb1a94cacafbf1a32d771593_hd.jpg)
