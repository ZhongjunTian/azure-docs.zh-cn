

* Dv2 系列、D 系列、G 系列以及对应的 DS/GS 系列是请求更快速的 CPU、更好的本地磁盘性能，或有更高内存要求之应用程序的最佳选择。  它们为许多企业级应用程序提供强大的组合。

D 系列的 VM 旨在运行需要更高计算能力和临时磁盘性能的应用程序。 D 系列 VM 为临时磁盘提供更快的处理器、更高的内存内核比和固态驱动器 (SSD)。 有关详细信息，请参阅 Azure 博客[新的 D 系列虚拟机大小](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)上的公告。

Dv2 系列是原 D 系列的后续系列，其特点是 CPU 功能更强大。 Dv2 系列 CPU 比 D 系列 CPU 快大约 35%。 该系列基于最新一代的 2.4 GHz Intel Xeon® E5-2673 v3 (Haswell) 处理器，通过 Intel Turbo Boost Technology 2.0 可以达到 3.1 GHz。 Dv2 系列的内存和磁盘配置与 D 系列相同。


## <a name="gs-series"></a>GS 系列*

ACU：180 - 240

| 大小 | CPU 核心数 | 内存：GiB | 本地 SSD：GiB | 最大数据磁盘数 | 缓存和本地磁盘的最大吞吐量：IOPS/MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大网卡数/网络带宽等级 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_GS1 |2 |28 |56 |4 |10,000 / 100 (264) |5,000 / 125 |2/高 |
| Standard_GS2 |4 |56 |112 |8 |20,000 / 200 (528) |10,000 / 250 |2/高 |
| Standard_GS3 |8 |112 |224 |16 |40,000 / 400 (1,056) |20,000 / 500 |4/很高 |
| Standard_GS4 |16 |224 |448 |32 |80,000 / 800 (2,112) |40,000 / 1,000 |8/极高 |
| Standard_GS5** |32 |448 |896 |64 |160,000 / 1,600 (4,224) |80,000 / 2,000 |8/极高 |

MBps = 每秒 10^6 字节，GiB = 1024^3 字节。

*GS 系列 VM 可能的最大磁盘吞吐量（IOPS 或 MBps）可能受限于附加磁盘的数量、大小和条带化。 有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../articles/storage/storage-premium-storage.md)。 

**实例对于专用于单个客户的硬件独立。
<br>

## <a name="g-series"></a>G 系列

ACU：180 - 240

| 大小         | CPU 核心数 | 内存：GiB | 本地 SSD：GiB | 本地磁盘的最大吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大网卡数/网络带宽等级 |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000/93/46                                           | 4/4 x 500                       | 2/高                     |
| Standard_G2  | 4         | 56          | 768            | 12000/187/93                                         | 8/8 x 500                       | 2/高                     |
| Standard_G3  | 8         | 112         | 1,536          | 24000/375/187                                        | 16/16 x 500                     | 4/很高                |
| Standard_G4  | 16        | 224         | 3,072          | 48000/750/375                                        | 32/32 x 500                     | 8/极高           |
| Standard_G5* | 32        | 448         | 6,144          | 96000 / 1500 / 750                                       | 64/64 x 500                     | 8/极高           |

*实例对于专用于单个客户的硬件独立。
<br>


## <a name="dsv2-series"></a>DSv2 系列*

ACU：210-250

| 大小 | CPU 核心数 | 内存：GiB | 本地 SSD：GiB | 最大数据磁盘数 | 缓存磁盘最大吞吐量：IOPS / MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大网卡数/网络带宽等级 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2 |2 |14 |28 |4 |8,000 / 64 (72) |6,400 / 96 |2/高 |
| Standard_DS12_v2 |4 |28 |56 |8 |16,000 / 128 (144) |12,800 / 192 |4/高 |
| Standard_DS13_v2 |8 |56 |112 |16 |32,000 / 256 (288) |25,600 / 384 |8/高 |
| Standard_DS14_v2 |16 |112 |224 |32 |64,000 / 512 (576) |51,200 / 768 |8/极高 |
| Standard_DS15_v2*** |20 |140 |280 |40 |80,000 / 640 (720) |64,000 / 960 |8/极高** |

MBps = 每秒 10^6 字节，GiB = 1024^3 字节。

*DSv2 系列 VM 可能的最大磁盘吞吐量（IOPS 或 MBps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../articles/storage/storage-premium-storage.md)。

**某些区域为 Standard_DS15_v2 大小提供加速网络。 有关使用情况和可用性的详细信息，请参阅 [Accelerated Networking is in Preview](https://azure.microsoft.com/updates/accelerated-networking-in-preview/)（加速网络已推出预览版）和[虚拟机的加速网络](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md)。

***实例对于专用于单个客户的硬件独立。
<br>
MBps = 每秒 10^6 字节，GiB = 1024^3 字节。

*DS 系列 VM 可能的最大磁盘吞吐量（IOPS 或 MBps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../articles/storage/storage-premium-storage.md)。

## <a name="dv2-series"></a>Dv2 系列

ACU：21--250

| 大小              | CPU 核心数 | 内存：GiB | 本地 SSD：GiB | 本地磁盘的最大吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大网卡数/网络带宽等级 |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 4/4x500                         | 2/高                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 8/8x500                         | 4/高                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 16/16x500                       | 8/高                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 32/32x500                       | 8/极高           |
| Standard_D15_v2** | 20        | 140         | 1,000          | 60000/937/468                                        | 40/40x500                       | 8/极高*          |

*某些区域为 Standard_D15_v2 大小提供加速网络。 有关使用情况和可用性的详细信息，请参阅 [Accelerated Networking is in Preview](https://azure.microsoft.com/updates/accelerated-networking-in-preview/)（加速网络已推出预览版）和[虚拟机的加速网络](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md)。

**实例对于专用于单个客户的硬件独立。

<br>

## <a name="ds-series"></a>DS 系列*

ACU：160

| 大小 | CPU 核心数 | 内存：GiB | 本地 SSD：GiB | 最大数据磁盘数 | 缓存磁盘最大吞吐量：IOPS / MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大网卡数/网络带宽等级 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |4 |8,000 / 64 (72) |6,400 / 64 |2/高 |
| Standard_DS12 |4 |28 |56 |8 |16,000 / 128 (144) |12,800 / 128 |4/高 |
| Standard_DS13 |8 |56 |112 |16 |32,000 / 256 (288) |25,600 / 256 |8/高 |
| Standard_DS14 |16 |112 |224 |32 |64,000 / 512 (576) |51,200 / 512 |8/非常高 |


MBps = 每秒 10^6 字节，GiB = 1024^3 字节。

*DS 系列 VM 可能的最大磁盘吞吐量（IOPS 或 MBps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../articles/storage/storage-premium-storage.md)。

## <a name="d-series"></a>D 系列

ACU：160

| 大小         | CPU 核心数 | 内存：GiB | 本地 SSD：GiB | 本地磁盘的最大吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大网卡数/网络带宽等级 |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000/93/46                                           | 4/4x500                         | 2/高                     |
| Standard_D12 | 4         | 28          | 200            | 12000/187/93                                         | 8/8x500                         | 4/高                     |
| Standard_D13 | 8         | 56          | 400            | 24000/375/187                                        | 16/16x500                       | 8/高                     |
| Standard_D14 | 16        | 112         | 800            | 48000/750/375                                        | 32/32x500                       | 8/非常高                |
<br>


<br>
