---
title: extundelete 数据恢复
layout: post
author:
  name: macint0sh
---
1. 卸载磁盘分区      

		在将数据误删除后，立即需要做的就是卸载这块磁盘分区
    
2. 查询可恢复的数据信息       

		通过extundelete命令可以查询/dev/sdXn分区可恢复的数据信息：

		# extundelete /dev/sdXn --inode 2
3. 恢复单个文件        

		# extundelete /dev/sdXn --restore-file 文件在分区内的路径 [前边不带/]
        
4. 恢复某个时间段的数据        

		# date +%s
		1449027652
		# extundelete --after 1449020452 --restore-all /dev/sdXn
        
#### 恢复成功后文件被保存在当前路径下的 RECOVERED_FILES 目录        




