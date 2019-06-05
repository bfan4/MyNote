#  Storage Gateway  

## 1.File Gateway  
* File Gateway for flat files, stored directly on S3.

## 2.Volume Gateway  
* **Stored Volumes** - Entire Dataset is stored on site and is asynchronously backed up to S3.

* **卷存储** - 如果需要对整个数据集进行低延迟访问，首先将本地网关配置为将所有数据存储在本地。然后以异步方式将此数据的时间点快照备份到 Amazon S3。异步方式就是在部分数据被更改之后，无需整体重新备份，只需要更新那些作出改变的数据即可。

* **Cached Volumes** - Entire Dataset is stored on S3 and the most frequently accessed data is cached on site.
* **卷缓存** - 整个数据集储存在S3上，最常用的部分的副本会保存在本地，缓存卷不仅有助于节省大量主存储成本，而且最大程度地减小了本地扩展存储的需求。您还可以保留对经常访问的数据的低延迟访问。

##  3.Gateway Virtual Tape Library  
* Used for backup and uses popular backup application like NetBackup, Backup Exec, veeam etc.