承接我的上一篇博客：

[https://blog.csdn.net/weixin_62528784/article/details/146125180?sharetype=blogdetail&sharerId=146125180&sharerefer=PC&sharesource=weixin_62528784&spm=1011.2480.3001.8118](https://blog.csdn.net/weixin_62528784/article/details/146125180?sharetype=blogdetail&sharerId=146125180&sharerefer=PC&sharesource=weixin_62528784&spm=1011.2480.3001.8118)

以下内容参考校HPC超算平台的官方指南文档：  
[https://docs.hpc.sjtu.edu.cn/index.html](https://docs.hpc.sjtu.edu.cn/index.html)

# 目录如下：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741448836768-febd715d-94a8-49f3-bba4-eca6998a754e.png)
# 一，快速上手：
[https://docs.hpc.sjtu.edu.cn/quickstart/index.html](https://docs.hpc.sjtu.edu.cn/quickstart/index.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741448923642-93a0bd35-e82c-40c5-a690-b0d955f9dc3a.png)

以上内容有所更新，参考我之前的2篇 sjtu HPC相关的博客

里面有一个提交 Hello world 单节点作业的流程演示



# 二，平台硬件资源：
[https://docs.hpc.sjtu.edu.cn/system/index.html](https://docs.hpc.sjtu.edu.cn/system/index.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503160040-0a026d77-4662-4401-97da-f760e5bc208a.png)

## 1，文件系统
<font style="color:rgba(0, 0, 0, 0.87);">HPC+AI 平台集群（除思源一号外）采用 Lustre 作为后端存储系统。Lustre是一种分布式的、可扩展的、高性能的并行文件系统，能够支持数万客户端、PB级存储容量、数百GB的聚合I/O吞吐，非常适合众多客户端并发进行大文件读写的场合。 Lustre最常用于高性能计算HPC，世界超级计算机TOP 10中的70%、TOP 30中的50%、TOP 100中的40%均部署了Lustre。</font>

<font style="color:rgba(0, 0, 0, 0.87);">HPC+AI 平台集群（除思源一号外）已上线多套 Lustre 文件系统，挂载在计算节点的不同目录：/lustre、/scratch。</font>

<font style="color:rgba(0, 0, 0, 0.87);">数据传输节点（data.hpc.sjtu.edu.cn）还多挂载了一个 /archive。</font>

<font style="color:rgba(0, 0, 0, 0.87);">思源一号为独立集群，使用Gpfs文件系统，共10P。系统包含4 台 DSS-G Server 节点，每台配置 2 块 300G HDD， 用于安装操作系统，安装配置 GPFS 集群及创建文件系统。文件系统 metadata 采用 3 副本冗余，文件系统 data 采用 8+2p 冗余。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">主文件系统</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id2)
<font style="color:rgba(0, 0, 0, 0.87);">/lustre 目录挂载的为 HPC+AI平台集群（除思源一号外）中的主文件系统，共 13.1P，用户的个人目录即位于该目录。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">主文件系统特性</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id3)
<font style="color:rgba(0, 0, 0, 0.87);">主文件系统主要使用 HDD 盘搭建，旨在提供大容量、高可用、较高性能的存储供用户使用。搭建过程中，使用 RAID 保障硬盘级别的数据安全，使用 HA（High Availability） 保障服务器级别的高可用。</font>

<font style="color:rgba(0, 0, 0, 0.87);">用户的主要工作、重要数据都应该发生和存储在主文件系统。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">如何使用主文件系统</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id4)
<font style="color:rgba(0, 0, 0, 0.87);">用户通过个人账户登录计算节点（包括登录节点）之后，默认进入主文件系统，即 HOME 目录。可以在以下路径找到 /lustre 提供给用户的空间：</font>

`/lustre/home/acct-xxxx/yyyy`

<font style="color:rgba(0, 0, 0, 0.87);">其中acct-xxxx代表计费帐号（课题组帐号），yyyy代表个人帐号。</font>

<font style="color:rgba(0, 0, 0, 0.87);">通过</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`cd`<font style="color:rgba(0, 0, 0, 0.87);"> </font>`cd $HOME`<font style="color:rgba(0, 0, 0, 0.87);"> </font>`cd ~`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">等方式都可进入主目录。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">全闪存文件系统</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id5)
<font style="color:rgba(0, 0, 0, 0.87);">/scratch 目录挂载的为 HPC+AI平台集群（除思源一号外）的全闪存并行文件系统，共 108T 容量，可用作用户的临时工作目录。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">全闪存文件系统特性</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id6)
<font style="color:rgba(0, 0, 0, 0.87);">全闪存文件系统使用全套的 SSD（NVMe协议） 硬盘搭建，旨在提供高性能的存储供用户使用，可更好地支持 IO 密集型作业。对系统来说，单客户端最大读带宽达 5.7GB/s，最大写带宽达 10GB/s；4k 小文件读 IOPS 达 170k，写 IOPS 达 126k。但同时，由于成本问题，系统提供的容量较小；在搭建时也未设置高可用和数据备份，存在数据存储安全性不高等问题。</font>

<font style="color:rgba(0, 0, 0, 0.87);">基于该套系统的特性，推荐用户将其作为临时工作目录，可用于</font>

1. <font style="color:rgba(0, 0, 0, 0.87);">存储计算过程中产生的临时文件</font>
2. <font style="color:rgba(0, 0, 0, 0.87);">保存读写频率高的文件副本</font>

**<font style="color:rgba(0, 0, 0, 0.87);">注意：为了保持全闪存文件系统的稳定可用，/scratch 目录每 3 个月会进行一次清理。因此，请务必及时将重要数据保存回 /lustre 目录。</font>**

### <font style="color:rgba(0, 0, 0, 0.87);">如何使用全闪存文件系统</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id7)
<font style="color:rgba(0, 0, 0, 0.87);">用户可以在以下路径找到 /scratch 提供的暂存空间：</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`/scratch/home/acct-xxxx/yyyy`

<font style="color:rgba(0, 0, 0, 0.87);">其中acct-xxxx代表计费帐号（课题组帐号），yyyy代表个人帐号。</font>

<font style="color:rgba(0, 0, 0, 0.87);">为了快捷访问，我们已经为用户设置好环境变量，</font>`cd $SCRATCH`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">即可进入该临时工作目录。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">作业使用示例</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id8)
<font style="color:rgba(0, 0, 0, 0.87);">我们使用生信 WES 分析流程为例，该流程从测序文件开始，经 bwa 比对、samtools处理，然后用 GATK 检测变异（部分步骤）。 原代码如下：</font>

```plain
#!/bin/bash

#SBATCH --job-name=WES
#SBATCH --partition=cpu
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH -N 1
#SBATCH --ntasks-per-node=40
#SBATCH --exclusive

echo "##### 加载相关软件 #####"
module load bwa samtools
module load miniconda3 && source activate 10_24  # gatk-4.1.9.0

echo "##### 设置变量 #####"
REFDIR=$HOME/med/annotation/gatk/hg19  # 参考基因组和注释文件目录
SAMPLEDIR=$HOME/med/testnor  # 样本目录

WORKDIR=$HOME/WES_TEST  # 工作目录
TMPDIR=$WORKDIR/tmpdir  # 临时缓存目录
mkdir -p $WORKDIR
mkdir -p $TMPDIR

SampleID=test  # 样本名

cd $WORKDIR

echo "##### bwa 比对 #####"
bwa mem -M -t 40 \
${REFDIR}/ucsc.hg19.fasta \
${SAMPLEDIR}/${SampleID}_1.fastq.gz \
${SAMPLEDIR}/${SampleID}_2.fastq.gz \
| gzip -3 > ${SampleID}_mem.sam

echo "##### samtools 生成bam #####"
samtools view -@ 40 -bS ${SampleID}_mem.sam \
| samtools sort -@ 40 > ${SampleID}_mem.sorted.bam

samtools index ${SampleID}_mem.sorted.bam

echo "##### gatk 检测变异 #####"
gatk ReorderSam \
-I ${SampleID}_mem.sorted.bam \
-O ${SampleID}_mem.sorted.reorder.bam \
-R ${REFDIR}/ucsc.hg19.fasta \
--TMP_DIR ${TMPDIR} \
--VALIDATION_STRINGENCY LENIENT \
--SEQUENCE_DICTIONARY ${REFDIR}/ucsc.hg19.dict \
--CREATE_INDEX true

gatk MarkDuplicates \
-I ${SampleID}_mem.sorted.reorder.bam \
-O ${SampleID}_mem.sorted.reorder.rmdup.bam \
--TMP_DIR ${TMPDIR} \
--REMOVE_DUPLICATES false \
--ASSUME_SORTED true \
--METRICS_FILE ${SampleID}_mem.sorted.reorder.markduplicates_metrics.txt \
--OPTICAL_DUPLICATE_PIXEL_DISTANCE 2500 \
--VALIDATION_STRINGENCY LENIENT \
--CREATE_INDEX true
```

<font style="color:rgba(0, 0, 0, 0.87);">过程中，会产生许多中间文件和临时文件。因此，可利用 $SCRATCH 作为临时目录，加快分析过程。只需要把脚本中的</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`WORKDIR=$HOME/WES_TEST`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">修改为</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`WORKDIR=$SCRATCH/WES_TEST`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">即可。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">归档文件系统</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id9)
<font style="color:rgba(0, 0, 0, 0.87);">在</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">data （data.hpc.sjtu.edu.cn）</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">节点的目录</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">/archive</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">下挂载了挂挡存储，共 3P 容量，用来存储用户的不常用数据。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">归档文件系统特性</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id10)
<font style="color:rgba(0, 0, 0, 0.87);">归档文件系统主要使用机械硬盘搭建，可提供大容量、高可用的存储供用户使用。搭建过程中，使用 RAID 保障硬盘级别的数据安全，使用 HA（High Availability） 保障服务器级别的高可用。归档文件系统作为主文件系统的一个补充，主要提供给用户存储不常用的数据（冷数据），从而释放主文件系统的存储空间、缓解主文件系统的存储压力。</font>

<font style="color:rgba(0, 0, 0, 0.87);">** 注意：和主文件系统以及全闪存文件系统不同，归档文件系统只能在</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">data</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">节点访问，无法在计算节点和登录节点访问，也就是说保存在该文件系统的数据不能在计算节点读取并参与计算，因此只推荐保存不常使用的数据。**</font>

### <font style="color:rgba(0, 0, 0, 0.87);">如何使用归档文件系统</font>[¶](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id11)
1. <font style="color:rgba(0, 0, 0, 0.87);">登录</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">data</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">节点</font>

```plain
# ssh $USER@data.hpc.sjtu.edu.cn
```

1. <font style="color:rgba(0, 0, 0, 0.87);">进入归档文件系统</font>

<font style="color:rgba(0, 0, 0, 0.87);">用户可以在以下路径找到 /archive 提供的个人存储空间:</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`/archive/home/acct-xxxx/yyyy`

<font style="color:rgba(0, 0, 0, 0.87);">其中 acct-xxxx 代表计费帐号（课题组帐号），yyyy 代表个人帐号。</font>

<font style="color:rgba(0, 0, 0, 0.87);">为了快捷访问，我们已经为用户设置好环境变量，</font>`cd $ARCHIVE`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">即可进入。</font>

1. <font style="color:rgba(0, 0, 0, 0.87);">将不常用文件移动到</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">$ARCHIVE</font>

```plain
# rsync -avh -P --append-verify $DATA $ARCHIVE
```

<font style="color:rgba(0, 0, 0, 0.87);">推荐使用 rsync 移动数据，详细参数含义可使用 </font>`man rsync`<font style="color:rgba(0, 0, 0, 0.87);"> 命令查看。</font>





## 2，计算系统：
[https://docs.hpc.sjtu.edu.cn/system/computesystem.html](https://docs.hpc.sjtu.edu.cn/system/computesystem.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503455192-863c562b-a056-45d6-95eb-286090d08a8d.png)



# 三，可视化平台
[https://docs.hpc.sjtu.edu.cn/studio/index.html](https://docs.hpc.sjtu.edu.cn/studio/index.html)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503571597-9980efc7-4b6c-49da-8c00-355e28498444.png)

## <font style="color:rgba(0, 0, 0, 0.87);">登录</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/basic.html#id1)
<font style="color:rgba(0, 0, 0, 0.87);">在浏览器中，打开：</font>[https://studio.hpc.sjtu.edu.cn](https://studio.hpc.sjtu.edu.cn/)

**<font style="color:rgba(0, 0, 0, 0.87);">小技巧</font>**

<font style="color:rgba(0, 0, 0, 0.87);">浏览器需为Chrome，Firefox或Edge。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">文件管理</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/basic.html#id2)
<font style="color:rgba(0, 0, 0, 0.87);">点击图标Files下拉菜单Home Directory即可进入文件管理界面</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669365-2f6af08a-6bef-4b55-a11e-559a4ebd7e9c.png)

<font style="color:rgba(0, 0, 0, 0.87);">文件管理包含以下功能：</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">功能</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">详细功能</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">按钮</font>** |
| :--- | :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">查看</font> | <font style="color:rgba(0, 0, 0, 0.87);">点击文 件可查看文件内容，点击文件夹可显示文件属性</font> | <font style="color:rgba(0, 0, 0, 0.87);">1</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">编辑</font> | <font style="color:rgba(0, 0, 0, 0.87);">仅可编辑文本文件，编辑文件有多种编 辑器可选，均为系统嵌入，无需链接本地编辑器</font> | <font style="color:rgba(0, 0, 0, 0.87);">2</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">重命名/移动</font> | <font style="color:rgba(0, 0, 0, 0.87);">重命名或移动文件</font> | <font style="color:rgba(0, 0, 0, 0.87);">3</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">下载</font> | <font style="color:rgba(0, 0, 0, 0.87);">下载文件，若 下载的为文件夹则会自动压缩成压缩包进行下载</font> | <font style="color:rgba(0, 0, 0, 0.87);">4</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">拷贝/粘贴</font> | <font style="color:rgba(0, 0, 0, 0.87);">复制文件</font> | <font style="color:rgba(0, 0, 0, 0.87);">5 6</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">全选/取消全选</font> | <font style="color:rgba(0, 0, 0, 0.87);">选择文件功能</font> | <font style="color:rgba(0, 0, 0, 0.87);">7</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">进入···</font> | <font style="color:rgba(0, 0, 0, 0.87);">进入到其他路径</font> | <font style="color:rgba(0, 0, 0, 0.87);">8</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">打开终端</font> | <font style="color:rgba(0, 0, 0, 0.87);">打开webshell</font> | <font style="color:rgba(0, 0, 0, 0.87);">9</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">新建文件</font> | <font style="color:rgba(0, 0, 0, 0.87);">新建文件</font> | <font style="color:rgba(0, 0, 0, 0.87);">10</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">新建目录</font> | <font style="color:rgba(0, 0, 0, 0.87);">新建目录</font> | <font style="color:rgba(0, 0, 0, 0.87);">11</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">上传</font> | <font style="color:rgba(0, 0, 0, 0.87);">上传文件</font> | <font style="color:rgba(0, 0, 0, 0.87);">12</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">显示隐藏文件</font> | <font style="color:rgba(0, 0, 0, 0.87);">点击可显示隐藏文件/文件夹</font> | <font style="color:rgba(0, 0, 0, 0.87);">13</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">显示属主信息</font> | <font style="color:rgba(0, 0, 0, 0.87);">显示文件/文件夹属主/大小/日期等信息</font> | <font style="color:rgba(0, 0, 0, 0.87);">14</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">删除</font> | <font style="color:rgba(0, 0, 0, 0.87);">删除文件/文件夹</font> | <font style="color:rgba(0, 0, 0, 0.87);">15</font> |


![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669353-012d3230-7747-4e90-924d-3f6355e1d412.png)

## <font style="color:rgba(0, 0, 0, 0.87);">作业</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/basic.html#id3)
### <font style="color:rgba(0, 0, 0, 0.87);">查看作业</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/basic.html#id4)
<font style="color:rgba(0, 0, 0, 0.87);">点击</font>`Jobs->Active Jobs`<font style="color:rgba(0, 0, 0, 0.87);">，查看队列中的作业。在右上角的</font>`Your Jobs/All Jobs`<font style="color:rgba(0, 0, 0, 0.87);">中选择要查看的任务类型。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669369-6e1db33a-1b09-4426-bf0f-fe8b61a49fe1.png)<font style="color:rgba(0, 0, 0, 0.87);"> </font>![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669373-5009ec96-d14e-4c79-87ab-2942536be873.png)

<font style="color:rgba(0, 0, 0, 0.87);">点击作业项前的箭头查看作业详情。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669439-2e2c7a49-b132-44eb-bf93-1bb5704325e3.png)

### <font style="color:rgba(0, 0, 0, 0.87);">提交作业</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/basic.html#id5)
<font style="color:rgba(0, 0, 0, 0.87);">点击</font>`Jobs->Job Composer`<font style="color:rgba(0, 0, 0, 0.87);">，打开新建作业选项卡。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669736-aa0a517a-c966-40be-92c1-940834b9cbf9.png)

<font style="color:rgba(0, 0, 0, 0.87);">点击左上角</font>`New Job`<font style="color:rgba(0, 0, 0, 0.87);">（1）按钮，选择模板，点击</font>`submit`<font style="color:rgba(0, 0, 0, 0.87);">（2）按钮提交作业。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669783-05600310-682d-4558-af12-81c1fbabf562.png)

<font style="color:rgba(0, 0, 0, 0.87);">同时HPC Studio提供了在线文本编辑功能，在右侧底部的</font>`Submit Script`<font style="color:rgba(0, 0, 0, 0.87);">选项卡中，点击</font>`Open Editor`<font style="color:rgba(0, 0, 0, 0.87);">按钮，即可打开文本编辑器。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669817-2b554b64-155e-4a8c-98ad-c6b85e49e311.png)<font style="color:rgba(0, 0, 0, 0.87);"> </font>![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669870-ce89c37a-aa28-4a4c-a304-a2561de7c14c.png)

## <font style="color:rgba(0, 0, 0, 0.87);">在线Shell终端</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/basic.html#shell)
<font style="color:rgba(0, 0, 0, 0.87);">点击</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`Clusters->sjtu Shell Access`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">,打开在线终端。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503669941-17110572-6380-4f87-b1c3-5b1dc1795c3b.png)

## <font style="color:rgba(0, 0, 0, 0.87);">远程桌面</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/basic.html#id6)
<font style="color:rgba(0, 0, 0, 0.87);">远程桌面打开方式需先提交一个空的作业取得计算节点的控制权（此操作会计入机时）。</font>

<font style="color:rgba(0, 0, 0, 0.87);">点击</font>`Interactive Apps->Desktop`<font style="color:rgba(0, 0, 0, 0.87);">按钮，进入作业提交页面。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503670178-40668429-ec53-46cb-870d-1f8fe0d0aaf9.png)

<font style="color:rgba(0, 0, 0, 0.87);">Number of hours 默认是 1，然后点击 launch 即可进入桌面选项卡。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503670171-43cd19da-3e7d-40cc-a5c2-0723f4e557c1.png)<font style="color:rgba(0, 0, 0, 0.87);"> </font>![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503670238-1f34bab1-6cfd-4322-8365-ac9e42feb4a2.png)

<font style="color:rgba(0, 0, 0, 0.87);">待选项卡显示作业在running的状态时,点击</font>`launch`<font style="color:rgba(0, 0, 0, 0.87);">即可进入远程桌面。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503670300-521a1c15-947c-429f-9157-9a7373506572.png)<font style="color:rgba(0, 0, 0, 0.87);"> </font>![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503670321-405b8a3c-6ab7-43dc-b681-da1ca768fad9.png)

## <font style="color:rgba(0, 0, 0, 0.87);">参考教学视频</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/basic.html#id7)
[2022 春季用户培训之HPC Studio](https://vshare.sjtu.edu.cn/play/f8e22f383e28d22acd6d556f913886b7)

[https://vshare.sjtu.edu.cn/play/f8e22f383e28d22acd6d556f913886b7](https://vshare.sjtu.edu.cn/play/f8e22f383e28d22acd6d556f913886b7)



另外一个可视化平台就是Jupyter了，Julia+Python+R，可以参考我之前的博客，我经常使用。

<font style="color:rgba(0, 0, 0, 0.87);">Jupyter是一个非营利组织，旨在“为数十种编程语言的交互式计算开发开源软件，开放标准和服务”。2014年由Fernando Pérez从IPython中衍生出来，Jupyter支持几十种语言的执行环境。</font>

<font style="color:rgba(0, 0, 0, 0.87);">Jupyter Project的名称是对Jupyter支持的三种核心编程语言的引用，这三种语言是Julia、Python和R，也是对伽利略记录发现木星的卫星的笔记本的致敬。Jupyter项目开发并支持交互式计算产品Jupyter Notebook、JupyterHub和JupyterLab，这是Jupyter Notebook的下一代版本。</font>

<font style="color:rgba(0, 0, 0, 0.87);">登录HPC Studio平台后，可以在内置应用中选择</font>`Jupyter`<font style="color:rgba(0, 0, 0, 0.87);">或</font>`Jupyer (GPU)`<font style="color:rgba(0, 0, 0, 0.87);">，均支持</font>`Jupyter Notebook`<font style="color:rgba(0, 0, 0, 0.87);">和</font>`JupyterLab`<font style="color:rgba(0, 0, 0, 0.87);">。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">在 Jupyter 中使用预置环境</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/jupyter.html#id1)
<font style="color:rgba(0, 0, 0, 0.87);">已有三个预置环境，可供用户使用：</font>

### <font style="color:rgba(0, 0, 0, 0.87);">预置 PyTorch 环境</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/jupyter.html#pytorch)
| **<font style="color:rgba(0, 0, 0, 0.87);">环境</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">版本</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">python</font> | <font style="color:rgba(0, 0, 0, 0.87);">3.8.3</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">cudatoolkit</font> | <font style="color:rgba(0, 0, 0, 0.87);">10.1.243</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">pytorch</font> | <font style="color:rgba(0, 0, 0, 0.87);">1.5.0</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">torchvision</font> | <font style="color:rgba(0, 0, 0, 0.87);">0.6.0</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">numpy</font> | <font style="color:rgba(0, 0, 0, 0.87);">1.18.1</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">pandas</font> | <font style="color:rgba(0, 0, 0, 0.87);">1.0.4</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">pillow</font> | <font style="color:rgba(0, 0, 0, 0.87);">7.1.2</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">scipy</font> | <font style="color:rgba(0, 0, 0, 0.87);">1.4.1</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">matplotlib</font> | <font style="color:rgba(0, 0, 0, 0.87);">3.2.1</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">seaborn</font> | <font style="color:rgba(0, 0, 0, 0.87);">0.10.1</font> |


### <font style="color:rgba(0, 0, 0, 0.87);">预置 TensorFlow 环境</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/jupyter.html#tensorflow)
| **<font style="color:rgba(0, 0, 0, 0.87);">环境</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">版本</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">python</font> | <font style="color:rgba(0, 0, 0, 0.87);">3.8.3</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">cudatoolkit</font> | <font style="color:rgba(0, 0, 0, 0.87);">10.1.243</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">cudnn</font> | <font style="color:rgba(0, 0, 0, 0.87);">7.6.5</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">tensorflow</font> | <font style="color:rgba(0, 0, 0, 0.87);">2.2.0</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">tensorboard</font> | <font style="color:rgba(0, 0, 0, 0.87);">2.2.2</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">numpy</font> | <font style="color:rgba(0, 0, 0, 0.87);">1.18.5</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">pandas</font> | <font style="color:rgba(0, 0, 0, 0.87);">1.0.4</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">pillow</font> | <font style="color:rgba(0, 0, 0, 0.87);">7.1.2</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">scipy</font> | <font style="color:rgba(0, 0, 0, 0.87);">1.4.1</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">matplotlib</font> | <font style="color:rgba(0, 0, 0, 0.87);">3.2.1</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">seaborn</font> | <font style="color:rgba(0, 0, 0, 0.87);">0.10.1</font> |


### <font style="color:rgba(0, 0, 0, 0.87);">预置 R 环境</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/jupyter.html#r)
| **<font style="color:rgba(0, 0, 0, 0.87);">环境</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">版本</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">R</font> | <font style="color:rgba(0, 0, 0, 0.87);">3.6.1</font> |


## <font style="color:rgba(0, 0, 0, 0.87);">在 Jupyter 中使用自定义的环境</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/jupyter.html#id2)
<font style="color:rgba(0, 0, 0, 0.87);">新建环境（或使用已有环境）:</font>

```plain
$ module load miniconda3
$ conda create -n test-env
$ source activate test-env
```

<font style="color:rgba(0, 0, 0, 0.87);">安装并注册为</font>`jupter kernel`<font style="color:rgba(0, 0, 0, 0.87);">：</font>

```plain
(test-env) $ conda install ipykernel
(test-env) $ python -m ipykernel install --user --name test-env --display-name "Test Environment"
```

<font style="color:rgba(0, 0, 0, 0.87);">然后可以在</font>`Jupyter`<font style="color:rgba(0, 0, 0, 0.87);">中选择名为</font>`Test Environment`<font style="color:rgba(0, 0, 0, 0.87);">的Kernel进行计算。</font>

<font style="color:rgba(0, 0, 0, 0.87);">如果环境需要依赖</font>`NVIDIA CUDA Toolkit`<font style="color:rgba(0, 0, 0, 0.87);">或</font>`NVIDIA cuDNN`<font style="color:rgba(0, 0, 0, 0.87);">，可以使用</font>`conda`<font style="color:rgba(0, 0, 0, 0.87);">进行安装：</font>

```plain
(test-env) $ conda install cudatoolkit=10.1 cudnn
```

## <font style="color:rgba(0, 0, 0, 0.87);">在 Jupyter 中使用自定义 R 环境</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/jupyter.html#jupyter-r)
<font style="color:rgba(0, 0, 0, 0.87);">新建环境（或使用已有环境）:</font>

```plain
$ module load miniconda3
$ conda create -n r-test-env
$ source activate r-test-env
$ (r-test-env) $ conda install -c r r-essentials
```

<font style="color:rgba(0, 0, 0, 0.87);">安装并注册为</font>`jupter kernel`<font style="color:rgba(0, 0, 0, 0.87);">：</font>

```plain
(test-env) $ R
> install.packages('IRkernel')
> IRkernel::installspec(name = 'r-test-env', displayname = 'R 3.6.1')
```

<font style="color:rgba(0, 0, 0, 0.87);">然后可以在</font>`Jupyter`<font style="color:rgba(0, 0, 0, 0.87);">中选择名为</font>`R 3.6.1`<font style="color:rgba(0, 0, 0, 0.87);">的Kernel进行计算。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">参考资料</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/jupyter.html#id3)
+ [Jupyter Wikepedia](https://zh.wikipedia.org/wiki/Jupyter)
+ [Jupyter Home](https://jupyter.org/)



除了最常用的Jupyter notebook，我本科时候第2常用的就是Rstudio server了。

# RStudio[¶](https://docs.hpc.sjtu.edu.cn/studio/rstudio.html#rstudio)
## <font style="color:rgba(0, 0, 0, 0.87);">简介</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/rstudio.html#id2)
<font style="color:rgba(0, 0, 0, 0.87);">RStudio是一个集成开发环境，主要支持R编程语言，专用于统计计算和图形。它包括一个控制台，支持代码执行的语法编辑器，以及用于绘制，调试和管理工作区的工具。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">可用的版本</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/rstudio.html#id3)
| **<font style="color:rgba(0, 0, 0, 0.87);">R版本</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">平台</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">RStudio版本</font>** |
| :--- | :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">4.2.2</font> | ![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503972401-2d2d4a2f-8566-48ea-8752-b902bae932b4.png) | <font style="color:rgba(0, 0, 0, 0.87);">2022.12.0</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">4.1.3</font> | ![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503972387-7bfe8768-4c38-4aae-9cf6-ac74a2418e6d.png) | <font style="color:rgba(0, 0, 0, 0.87);">2022.02.1</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">4.0.2</font> | ![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503972386-333dafd9-5552-4922-a1bf-0bd9aff7df69.png) | <font style="color:rgba(0, 0, 0, 0.87);">1.2.5042</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">3.6.3</font> | ![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503972379-5f7a8410-40e0-4a5a-aaa7-385258594d90.png) | <font style="color:rgba(0, 0, 0, 0.87);">1.2.5042</font> |


## <font style="color:rgba(0, 0, 0, 0.87);">如何使用</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/rstudio.html#id4)
<font style="color:rgba(0, 0, 0, 0.87);">使用超算的账号及密码登录</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>[HPC Studio](https://studio.hpc.sjtu.edu.cn/)<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">，在导航栏</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`Interactive Apps`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">中选择</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`RStudio Server`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">，如下图：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503972418-d154f3eb-4bd4-476f-ae52-b28ab1f40826.png)

<font style="color:rgba(0, 0, 0, 0.87);">点击后会出现相关的启动界面，可以设置作业时间，资源情况，软件版本。设置完成后</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`Launch`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">即可运行：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503972856-6cd831be-d53f-4f14-8f74-62c1bdc6c67c.png)

**<font style="color:rgba(0, 0, 0, 0.87);">小技巧</font>**

<font style="color:rgba(0, 0, 0, 0.87);">π2.0 集群和思源一号的数据不互通，注意区分。</font>

<font style="color:rgba(0, 0, 0, 0.87);">待界面从排队变成</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`Running`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">后，可通过</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`Connect to RStudio Server`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">连接到</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`Rstudio Server`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503973019-17c3d789-1b2d-4104-adb5-8f1d5b3844c9.png)

## <font style="color:rgba(0, 0, 0, 0.87);">运行示例</font>[¶](https://docs.hpc.sjtu.edu.cn/studio/rstudio.html#id5)
<font style="color:rgba(0, 0, 0, 0.87);">所需的</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`R`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">依赖包需要自行安装：</font>

```plain
# Upload library
library(circlize)
circos.par("track.height" = 0.4)

# Create data
data = data.frame(
  factor = sample(letters[1:8], 1000, replace = TRUE),
  x = rnorm(1000),
  y = runif(1000)
)

# Step1: Initialise the chart giving factor and x-axis.
circos.initialize( factors=data$factor, x=data$x )

# Step 2: Build the regions.
circos.trackPlotRegion(factors = data$factor, y = data$y, panel.fun = function(x, y) {
  circos.axis()
})

# Step 3: Add points
circos.trackPoints(data$factor, data$x, data$y, col="#69b3a2")
```

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741503973025-454c1cc3-6695-4299-871c-69ea51e34178.png)



——》该部分的常见问题：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741504070369-6c7a309f-329d-443f-a26e-7adf9d98690f.png)



# 四，登入交我算集群平台
[https://docs.hpc.sjtu.edu.cn/login/index.html](https://docs.hpc.sjtu.edu.cn/login/index.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741504106310-b8273938-3c2e-4569-99a2-089ffb37ae33.png)

ssh登入的方式就不用说了，我之前的HPC系列都是用这种方式登入的，以vscode为主。

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741504209378-36c4baeb-38a4-4e13-b168-05abd14f9a6e.png)

然后重点是免密登入部分，我在上一篇新手指南博客中就已经提到了，我们常规的小型服务器的公私钥的免密登入方式是不work的。

## <font style="color:rgba(0, 0, 0, 0.87);">免密SSH登录</font>[¶](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#label-no-password-login)
<font style="color:rgba(0, 0, 0, 0.87);">经过安全升级后，超算平台不再支持传统的公私钥免密，将公钥写入个人家目录的</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`~/.ssh/authorized_keys`<font style="color:rgba(0, 0, 0, 0.87);"> </font>**<font style="color:rgba(0, 0, 0, 0.87);">不会</font>**<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">再赋予您免密登录的权限。</font>

<font style="color:rgba(0, 0, 0, 0.87);">如果您希望继续进行SSH免密登录，需要从</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>[超算账号管理平台](https://my.hpc.sjtu.edu.cn/)<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">申请免密证书。以RSA密钥为例，公私钥免密和证书免密的区别如下表所示。</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">传统公钥免密</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">证书免密</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">生成公钥id_rsa.pub和私钥id_rsa文件</font> | <font style="color:rgba(0, 0, 0, 0.87);">生成公钥id_rsa.pub和私钥id_rsa文件</font> |
| | <font style="color:rgba(0, 0, 0, 0.87);">用户提交公钥id_rsa.pub申请证书id_rsa-cert.pub</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">将公钥内容写入服务器的~/.ssh/authorized_keys中</font> | |
| <font style="color:rgba(0, 0, 0, 0.87);">本地使用私钥id_rsa进行免密登录</font> | <font style="color:rgba(0, 0, 0, 0.87);">本地使用私钥id_rsa和证书id_rsa-cert.pub进行免密登录</font> |


<font style="color:rgba(0, 0, 0, 0.87);">申请免密证书的方法请参阅</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>[账号安全信息管理](https://docs.hpc.sjtu.edu.cn/accounts/security.html)<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">的相关章节。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">以Mobaxterm为代表的命令行客户端进行免密登录</font>[¶](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#id9)
<font style="color:rgba(0, 0, 0, 0.87);">此章节同样适用于原生linux环境进行免密登录。</font>

<font style="color:rgba(0, 0, 0, 0.87);">将下载得到的证书文件存放到</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`~/.ssh/`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">目录，同时您原有的或者下载得到的私钥文件也应该存在于此目录，并与证书文件名保持上表所示的匹配关系。如果您不确定Mobaxterm将个人家目录映射到了哪里，请打开</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`settings - configurations`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">检查下图所示的路径配置。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741504320691-ca3dac2c-f2ec-4960-8ff4-589198bf72af.png)

_<font style="color:rgba(0, 0, 0, 0.87);">Mobaxterm查看个人目录映射位置</font>_[_¶_](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#id13)

<font style="color:rgba(0, 0, 0, 0.87);">之后在免密有效期内发起ssh连接即可实现免密登录。</font>

<font style="color:rgba(0, 0, 0, 0.87);"></font>

## <font style="color:rgba(0, 0, 0, 0.87);">免密SSH登录</font>[¶](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#label-no-password-login)
<font style="color:rgba(0, 0, 0, 0.87);">经过安全升级后，超算平台不再支持传统的公私钥免密，将公钥写入个人家目录的</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`~/.ssh/authorized_keys`<font style="color:rgba(0, 0, 0, 0.87);"> </font>**<font style="color:rgba(0, 0, 0, 0.87);">不会</font>**<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">再赋予您免密登录的权限。</font>

<font style="color:rgba(0, 0, 0, 0.87);">如果您希望继续进行SSH免密登录，需要从</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>[超算账号管理平台](https://my.hpc.sjtu.edu.cn/)<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">申请免密证书。以RSA密钥为例，公私钥免密和证书免密的区别如下表所示。</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">传统公钥免密</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">证书免密</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">生成公钥id_rsa.pub和私钥id_rsa文件</font> | <font style="color:rgba(0, 0, 0, 0.87);">生成公钥id_rsa.pub和私钥id_rsa文件</font> |
| | <font style="color:rgba(0, 0, 0, 0.87);">用户提交公钥id_rsa.pub申请证书id_rsa-cert.pub</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">将公钥内容写入服务器的~/.ssh/authorized_keys中</font> | |
| <font style="color:rgba(0, 0, 0, 0.87);">本地使用私钥id_rsa进行免密登录</font> | <font style="color:rgba(0, 0, 0, 0.87);">本地使用私钥id_rsa和证书id_rsa-cert.pub进行免密登录</font> |


<font style="color:rgba(0, 0, 0, 0.87);">申请免密证书的方法请参阅</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>[账号安全信息管理](https://docs.hpc.sjtu.edu.cn/accounts/security.html)<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">的相关章节。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">以Mobaxterm为代表的命令行客户端进行免密登录</font>[¶](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#id9)
<font style="color:rgba(0, 0, 0, 0.87);">此章节同样适用于原生linux环境进行免密登录。</font>

<font style="color:rgba(0, 0, 0, 0.87);">将下载得到的证书文件存放到</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`~/.ssh/`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">目录，同时您原有的或者下载得到的私钥文件也应该存在于此目录，并与证书文件名保持上表所示的匹配关系。如果您不确定Mobaxterm将个人家目录映射到了哪里，请打开</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`settings - configurations`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">检查下图所示的路径配置。</font>

![]()

_<font style="color:rgba(0, 0, 0, 0.87);">Mobaxterm查看个人目录映射位置</font>_[_¶_](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#id13)

<font style="color:rgba(0, 0, 0, 0.87);">之后在免密有效期内发起ssh连接即可实现免密登录。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">如何生成自己的公私钥对</font>[¶](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#id10)
```plain
（在集群上）$ rm -f ~/.ssh/authorized_keys             # 清除服务器上原有的 authorized_keys
（在自己电脑上）$ rm  ~/.ssh/id*                           # 清除本地 .ssh 文件夹中的密钥对
（在自己电脑上）$ ssh-keygen -t rsa                        # 在本地重新生成密钥对。第二个问题，设置密码短语 (passphrase)，并记住密码短语
（在自己电脑上）$ ssh-keygen -R sylogin.hpc.sjtu.edu.cn    # 清理本地 known_hosts 里关于集群的条目
（在自己电脑上）$ ssh-copy-id YOUR_USERNAME@TARGET_IP      # 将本地新的公钥发给服务器，存在服务器的 authorized_keys 文件里
```

## <font style="color:rgba(0, 0, 0, 0.87);">SSH 重置 known_hosts</font>[¶](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#ssh-known-hosts)
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741504441532-d6e3553d-b127-44cc-89f9-d4ca4dfdc036.png)

<font style="color:rgba(0, 0, 0, 0.87);">若遇到上方图片中的问题，请重置 known_hosts，命令如下：</font>

```python
（在自己电脑上）$ ssh-keygen -R sylogin.hpc.sjtu.edu.cn
```

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741504471021-9f36c787-830e-4885-a0da-d61963d4aa8d.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741504495919-3d3b7751-dafa-4f2a-95f4-b4054b7a5073.png)

## <font style="color:rgba(0, 0, 0, 0.87);">登录常掉线的问题</font>[¶](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#id12)
<font style="color:rgba(0, 0, 0, 0.87);">如果 SSH 客户端长时间静默后，SSH 服务器端会自动断开相关会话。要解决这个，需要调整 SSH 的 keepalive 值，设置一个较长的静默时长阈值。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">Mac/Linux用户</font>[¶](https://docs.hpc.sjtu.edu.cn/login/sshlogin.html#mac-linux)
<font style="color:rgba(0, 0, 0, 0.87);">对于 Mac/Linux 用户，并且使用操作系统原生的终端 (terminal)，需要修改</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`$HOME/.ssh/config`<font style="color:rgba(0, 0, 0, 0.87);">。具体的，在文件中添加如下内容：</font>

```plain
Host pi-sjtu-login:
    HostName sylogin.hpc.sjtu.edu.cn
    ServerAliveInterval 240
```

<font style="color:rgba(0, 0, 0, 0.87);">其中 ServerAliveInterval 后的值即为阈值，单位为秒，用户可根据需要自行调整。</font>

<font style="color:rgba(0, 0, 0, 0.87);">或者为了对所有的服务器设置长静默阈值：</font>

```plain
Host *
    ServerAliveInterval 240
```

<font style="color:rgba(0, 0, 0, 0.87);">之后保持</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`config`<font style="color:rgba(0, 0, 0, 0.87);">文件为只可读：</font>

```python
chmod 600 ~/.ssh/config
```

<font style="color:rgba(0, 0, 0, 0.87);">演示如下：  
</font><font style="color:rgba(0, 0, 0, 0.87);">我们依据习惯，设置该值为30mins，也就是30*60=1800s</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741504964286-12351fae-391b-4051-bd34-bb432c875949.png)

```python
vim $HOME/.ssh/config

# 修改为：

Host *
    ServerAliveInterval 1800

# 然后
chmod 600 ~/.ssh/config
```

<font style="color:rgba(0, 0, 0, 0.87);">当然，上面只是简单配置，具体细节部分参考：</font>[https://linux.die.net/man/5/ssh_config](https://linux.die.net/man/5/ssh_config)



## 1，tmux
此外需要注意的一点就是tmux：

可以参考我之前的博客，对于如何使用tmux，我也专门学过一期

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741506864475-7264b7c2-70b3-43e4-b391-1042a3563147.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741506887766-d1a270d7-686d-4150-bfb9-7095f4cce169.png)

### <font style="color:rgba(0, 0, 0, 0.87);">安装</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id4)
<font style="color:rgba(0, 0, 0, 0.87);">集群中已经默认安装了Tmux，无须操作。如果您需要在自己的服务器上安装Tmux，请参考以下指令：</font>

```plain
# Ubuntu 或 Debian
$ sudo apt-get install tmux

# CentOS 或 Fedora
$ sudo yum install tmux

# Mac
$ brew install tmux
```

#### <font style="color:rgba(0, 0, 0, 0.87);">启动与退出</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id5)
<font style="color:rgba(0, 0, 0, 0.87);">直接在终端中键入</font>`tmux`<font style="color:rgba(0, 0, 0, 0.87);">指令，即可进入Tmux窗口。</font>

```plain
$ tmux
```

<font style="color:rgba(0, 0, 0, 0.87);">上面命令会启动 Tmux 窗口，底部有一个状态栏。状态栏的左侧是窗口信息（编号和名称），右侧是系统信息。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741506931434-c25dddb6-c415-47c8-9066-a3255d4d8f65.png)

<font style="color:rgba(0, 0, 0, 0.87);">按下</font>`Ctrl+d`<font style="color:rgba(0, 0, 0, 0.87);">或者显式输入</font>`exit`<font style="color:rgba(0, 0, 0, 0.87);">命令，就可以退出 Tmux 窗口。</font>

```plain
$ exit
```

### <font style="color:rgba(0, 0, 0, 0.87);">快捷键</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id6)
<font style="color:rgba(0, 0, 0, 0.87);">Tmux有大量的快捷键。所有的快捷键都要使用</font>`Ctrl+b`<font style="color:rgba(0, 0, 0, 0.87);">作为前缀唤醒。我们将会在后续章节中讲解快捷键的具体使用。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">会话管理</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id7)
### <font style="color:rgba(0, 0, 0, 0.87);">新建会话</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id8)
<font style="color:rgba(0, 0, 0, 0.87);">第一个启动的会话名为</font>`0`<font style="color:rgba(0, 0, 0, 0.87);">，之后是</font>`1`<font style="color:rgba(0, 0, 0, 0.87);">、</font>`2`<font style="color:rgba(0, 0, 0, 0.87);">一次类推。</font>

<font style="color:rgba(0, 0, 0, 0.87);">但是有时候我们希望为会话起名以方便区分。</font>

```plain
$ tmux new -s SESSION_NAME
```

<font style="color:rgba(0, 0, 0, 0.87);">以上指令启动了一个名为</font>`SESSION_NAME`<font style="color:rgba(0, 0, 0, 0.87);">的会话。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">分离会话</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id9)
<font style="color:rgba(0, 0, 0, 0.87);">如果我们想离开会话，但又不想关闭会话，有两种方式。按下</font>`Ctrl+b d`<font style="color:rgba(0, 0, 0, 0.87);">或者</font>`tmux detach`<font style="color:rgba(0, 0, 0, 0.87);">指令，将会分离会话与窗口</font>

```plain
$ tmux detach
```

<font style="color:rgba(0, 0, 0, 0.87);">后面一种方法要求当前会话无正在运行的进程，即保证终端可操作。我们更推荐使用前者。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">查看会话</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id10)
<font style="color:rgba(0, 0, 0, 0.87);">要查看当前已有会话，使用</font>`tmux ls`<font style="color:rgba(0, 0, 0, 0.87);">指令。</font>

```plain
$ tmux ls
```

### <font style="color:rgba(0, 0, 0, 0.87);">接入会话</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id11)
`tmux attach`<font style="color:rgba(0, 0, 0, 0.87);">命令用于重新接入某个已存在的会话。</font>

```plain
# 使用会话编号
$ tmux attach -t 0

# 使用会话名称
$ tmux attach -t SESSION_NAME
```

### <font style="color:rgba(0, 0, 0, 0.87);">杀死会话</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id12)
`tmux kill-session`<font style="color:rgba(0, 0, 0, 0.87);">命令用于杀死某个会话。</font>

```plain
# 使用会话编号
$ tmux kill-session -t 0

# 使用会话名称
$ tmux kill-session -t SESSION_NAME
```

### <font style="color:rgba(0, 0, 0, 0.87);">切换会话</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id13)
`tmux switch`<font style="color:rgba(0, 0, 0, 0.87);">命令用于切换会话。</font>

```plain
# 使用会话编号
$ tmux switch -t 0

# 使用会话名称
$ tmux switch -t SESSION_NAME
```

`Ctrl+b s`<font style="color:rgba(0, 0, 0, 0.87);">可以快捷地查看并切换会话</font>

### <font style="color:rgba(0, 0, 0, 0.87);">重命名会话</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id14)
`tmux rename-session`<font style="color:rgba(0, 0, 0, 0.87);">命令用于重命名会话。</font>

```plain
# 将0号会话重命名为SESSION_NAME
$ tmux rename-session -t 0 SESSION_NAME
```

<font style="color:rgba(0, 0, 0, 0.87);">对应快捷键为</font>`Ctrl+b $`<font style="color:rgba(0, 0, 0, 0.87);">。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">窗格（window）操作</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#window)
<font style="color:rgba(0, 0, 0, 0.87);">Tmux可以将窗口分成多个窗格（window），每个窗格运行不同的命令。以下命令都是在Tmux窗口中执行。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">划分窗格</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id15)
`tmux split-window`<font style="color:rgba(0, 0, 0, 0.87);">命令用来划分窗格。</font>

```plain
# 划分上下两个窗格
$ tmux split-window

# 划分左右两个窗格
$ tmux split-window -h
```

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741506931442-0425327c-c1f4-4bd6-86e6-c37e1dcc3719.png)

<font style="color:rgba(0, 0, 0, 0.87);">对应快捷键为</font>`Ctrl+b "`<font style="color:rgba(0, 0, 0, 0.87);">和</font>`Ctrl+b %`

### <font style="color:rgba(0, 0, 0, 0.87);">移动光标</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id16)
`tmux select-pane`<font style="color:rgba(0, 0, 0, 0.87);">命令用来移动光标位置。</font>

```plain
# 光标切换到上方窗格
$ tmux select-pane -U

# 光标切换到下方窗格
$ tmux select-pane -D

# 光标切换到左边窗格
$ tmux select-pane -L

# 光标切换到右边窗格
$ tmux select-pane -R
```

<font style="color:rgba(0, 0, 0, 0.87);">对应快捷键为</font>`Ctrl+b ↑`<font style="color:rgba(0, 0, 0, 0.87);">、</font>`Ctrl+b ↓`<font style="color:rgba(0, 0, 0, 0.87);">、</font>`Ctrl+b ←`<font style="color:rgba(0, 0, 0, 0.87);">、</font>`Ctrl+b →`<font style="color:rgba(0, 0, 0, 0.87);">。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">窗格快捷键</font>[¶](https://docs.hpc.sjtu.edu.cn/login/tmux.html#id17)
```plain
$ Ctrl+b %：划分左右两个窗格。
$ Ctrl+b "：划分上下两个窗格。
$ Ctrl+b <arrow key>：光标切换到其他窗格。<arrow key>是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键↓。
$ Ctrl+b ;：光标切换到上一个窗格。
$ Ctrl+b o：光标切换到下一个窗格。
$ Ctrl+b {：当前窗格左移。
$ Ctrl+b }：当前窗格右移。
$ Ctrl+b Ctrl+o：当前窗格上移。
$ Ctrl+b Alt+o：当前窗格下移。
$ Ctrl+b x：关闭当前窗格。
$ Ctrl+b !：将当前窗格拆分为一个独立窗口。
$ Ctrl+b z：当前窗格全屏显示，再使用一次会变回原来大小。
$ Ctrl+b Ctrl+<arrow key>：按箭头方向调整窗格大小。
$ Ctrl+b q：显示窗格编号。
```



## 2，tabby：
  
另外超算团队还开发了一个定制版本的ssh客户端，但是暂时没有android版本

至于原生版本的tabby，可以参考官网：[https://github.com/Eugeny/tabby/blob/master/README.zh-CN.md](https://github.com/Eugeny/tabby/blob/master/README.zh-CN.md)

[https://tabby.sh/](https://tabby.sh/)

原版的也没有对应的android版本，

所以**我本人还是更推荐**termius，支持多操作系统

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741507044121-97792bbc-8995-4256-a3cc-13f394615f91.png)



## 3，vscode：  

这个是我们重点关心的，	

[https://docs.hpc.sjtu.edu.cn/login/vscode.html](https://docs.hpc.sjtu.edu.cn/login/vscode.html)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741507533897-606bbed8-3dec-4dca-ae1a-c77d15933199.png)

重点的部分在于登入密码输入的部分：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741507609979-106830ca-17aa-4c2b-a622-0d68e09e5d5e.png)

就是我们不是在正常的top正上方的部分输入登入密码，而是需要在起始链接部分的detail中输入输入密码；

至于免密登入，参考我的上一篇博客，需要申请免密证书，普通的公私钥的反式已经是不work了。



## 4，使用X Server显示图形界面


[https://docs.hpc.sjtu.edu.cn/login/xserver.html](https://docs.hpc.sjtu.edu.cn/login/xserver.html)

还有一种我个人不是很常用的方式，但是可以尝试图形化界面：  
这个其实就和desktop很像了，

比如说结构生物学方面的relion软件，如果我想操作的话，我得以图形化界面来进行操作

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508463901-70d30870-af33-49fb-88c1-d8bf409fc727.png)

```python
module load rdp                             # 加载远程桌面及VNC启动脚本
sbatch -p cpu -n 4 -J rdp --wrap="rdp"  	# 提交计算节点执行
squeue                                      # 查看分配到的节点
ssh -X 该分配的节点                          # 通过登录节点登录到计算节点，需要-X参数
module load relion                          # 运行GUI程序
relion
```

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508612461-5c39047b-0a4f-4d2d-9c5c-113c1ffc2790.png)

然后我就能进入到图形化界面中了

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508624484-4cef8fe7-6e4c-480c-9db8-0cd967b3b6e3.png)

然后就可以轻松愉快地使用该软件了，以linux-desktop的形式

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508645349-9db466ce-d860-4f2f-871e-5a2820534bd1.png)

同样的，我们再以fastqc为例子，来进行尝试：

参考[https://docs.hpc.sjtu.edu.cn/app/bioinformatics/fastqc.html](https://docs.hpc.sjtu.edu.cn/app/bioinformatics/fastqc.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508847293-d4bd7768-ce1d-480a-9825-704932811429.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508860837-de87cf98-b6fb-4824-82e8-c5b2c907cbbf.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508871667-b1a128b5-e9b4-464c-a404-32bd34d73b5d.png)  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508876167-03e466d5-325e-4391-b033-7adc3d1f0178.png)

然后我们就可以正常的操作执行任务了：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508893558-159b6314-aa05-4868-b032-479b843299bb.png)

# 五，文件系统与数据访问
[https://docs.hpc.sjtu.edu.cn/transport/index.html](https://docs.hpc.sjtu.edu.cn/transport/index.html)

目录如下：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741508999111-986fd45c-39eb-4c51-b158-2cb1a00dada5.png)

## <font style="color:rgba(0, 0, 0, 0.87);">文件系统简介</font>[¶](https://docs.hpc.sjtu.edu.cn/transport/index.html#id2)
<font style="color:rgba(0, 0, 0, 0.87);">下图为 Pi 2.0、思源一号集群目前的存储架构，展示了各节点可以访问到的存储系统。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741509147225-68049cc7-4996-4ef0-bc60-ac3f54355f65.png)

### <font style="color:rgba(0, 0, 0, 0.87);">总结</font>[¶](https://docs.hpc.sjtu.edu.cn/transport/index.html#id3)
<font style="color:rgba(0, 0, 0, 0.87);">文件系统可以管理数据和访问数据，不同的文件系统具有不同的特点。下面的表格介绍了超算平台使用的文件系统的特征。</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">文件系统</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">挂载点</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">裸盘容量</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">存储介质</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">在哪些节点访问</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">是否收费</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">是否有快照功能</font>** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">Lustre</font> | <font style="color:rgba(0, 0, 0, 0.87);">/lustre</font> | <font style="color:rgba(0, 0, 0, 0.87);">25 PB</font> | <font style="color:rgba(0, 0, 0, 0.87);">HDD</font> | <font style="color:rgba(0, 0, 0, 0.87);">Pi 2.0集群的登录节点、计算节点；data传输节点</font> | <font style="color:rgba(0, 0, 0, 0.87);">3 TB以下免费，3 TB 以上收费，和思源存储合并计费</font> | <font style="color:rgba(0, 0, 0, 0.87);">无</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">GPFS</font> | <font style="color:rgba(0, 0, 0, 0.87);">/dssg</font> | <font style="color:rgba(0, 0, 0, 0.87);">10 PB</font> | <font style="color:rgba(0, 0, 0, 0.87);">HDD</font> | <font style="color:rgba(0, 0, 0, 0.87);">思源一号集群的登录节点、计算节点；data、sydata传输节点</font> | <font style="color:rgba(0, 0, 0, 0.87);">3 TB以下免费，3 TB 以上收费，和 Pi2.0 存储合并计费</font> | <font style="color:rgba(0, 0, 0, 0.87);">无</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">全闪存文件系统</font> | <font style="color:rgba(0, 0, 0, 0.87);">/scratch</font> | <font style="color:rgba(0, 0, 0, 0.87);">108 TB</font> | <font style="color:rgba(0, 0, 0, 0.87);">NVMe SSD</font> | <font style="color:rgba(0, 0, 0, 0.87);">Pi 2.0集群的登录节点、计算节点；data传输节点</font> | <font style="color:rgba(0, 0, 0, 0.87);">不收费</font> | <font style="color:rgba(0, 0, 0, 0.87);">无</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">冷存储系统</font> | <font style="color:rgba(0, 0, 0, 0.87);">/archive</font> | <font style="color:rgba(0, 0, 0, 0.87);">23.5 PB</font> | <font style="color:rgba(0, 0, 0, 0.87);">HDD</font> | <font style="color:rgba(0, 0, 0, 0.87);">data、sydata传输节点</font> | <font style="color:rgba(0, 0, 0, 0.87);">收费</font> | <font style="color:rgba(0, 0, 0, 0.87);">有</font> |


+ <font style="color:rgba(0, 0, 0, 0.87);">Lustre/GPFS 文件系统：提供了大容量、高可用、较高性能的存储，是 Pi2.0、思源一号集群的主要存储系统，是用户通过登录节点后默认进入的文件系统。用户的主要工作和常用数据都位于该文件系统。用户通过</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`cd $HOME`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">可以跳转到 Lustre/GPFS 下自己的目录。</font>
+ <font style="color:rgba(0, 0, 0, 0.87);">全闪存文件系统：使用全套 NVMe SSD 搭建的高性能存储可以更好地支持 I/O 密集作业。该系统容量较小，同时未设置高可用，会定期清理数据，推荐将其作为临时工作目录使用。用户通过</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`cd $SCRATCH`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">可以跳转到全闪存文件系统下自己的目录。</font>
+ <font style="color:rgba(0, 0, 0, 0.87);">冷存储系统：作为 Lustre/GPFS 文件系统的补充，提供大容量、高可用的存储，只能在传输节点访问，主要给用户存放不常用的数据。冷存储系统具有快照功能，快照可以恢复用户误删除的数据。用户通过 </font>`cd $ARCHIVE`<font style="color:rgba(0, 0, 0, 0.87);"> 可以跳转到冷存储下自己的目录。</font>

此处演示如下：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741509170095-fec70be2-62fb-4036-8811-1351eb08f71a.png)

每个文件系统的应用场景如下：

### <font style="color:rgba(0, 0, 0, 0.87);">文件系统使用场景</font>[¶](https://docs.hpc.sjtu.edu.cn/transport/index.html#id4)
<font style="color:rgba(0, 0, 0, 0.87);">因为存储介质、搭建方法不同，文件系统有各自适用的使用场景。</font>

**<font style="color:rgba(0, 0, 0, 0.87);">场景一：将数据从用户本地传输到 Pi 2.0 集群，再提交作业处理数据</font>**

1. <font style="color:rgba(0, 0, 0, 0.87);">用户从本地终端，使用 scp/rsync 等命令向 data 传输节点发起数据传输</font>
2. <font style="color:rgba(0, 0, 0, 0.87);">用户登录 Pi 2.0 登录节点，此时可以访问到之前传输的数据，然后用户提交 Slurm 作业</font>

<font style="color:rgba(0, 0, 0, 0.87);">可以参考：</font>

+ [数据传输方法](https://docs.hpc.sjtu.edu.cn/transport/transportmethod.html#transportmethod)
+ [通过远程挂载读写超算平台的数据](https://docs.hpc.sjtu.edu.cn/transport/remoteaccessdata.html#remoteaccessdata)

——》这个是最常用的！！！

**<font style="color:rgba(0, 0, 0, 0.87);">场景二：使用全闪存文件系统作为临时目录</font>**

1. <font style="color:rgba(0, 0, 0, 0.87);">在 Slurm 脚本中，自定义一个目录</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`$SCRATCH/tmp_dir`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">作为临时目录存放中间文件，加快计算过程</font>
2. <font style="color:rgba(0, 0, 0, 0.87);">计算完成后，将计算结果及时从 Scratch 转移到 Lustre/GPFS 系统</font>

<font style="color:rgba(0, 0, 0, 0.87);">可以参考：</font>

+ [如何使用全闪存文件系统](https://docs.hpc.sjtu.edu.cn/system/filesystem.html#id7)

**<font style="color:rgba(0, 0, 0, 0.87);">场景三：将 Lustre 文件系统中不常用的数据迁移到冷存储</font>**

1. <font style="color:rgba(0, 0, 0, 0.87);">用户登录到 data 传输节点，通过</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`rsync`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">等命令向冷存储迁移数据</font>
2. <font style="color:rgba(0, 0, 0, 0.87);">通过数据校验，确认数据完整迁移到了冷存储</font>
3. <font style="color:rgba(0, 0, 0, 0.87);">清理原文件，释放存储空间</font>

<font style="color:rgba(0, 0, 0, 0.87);">可以参考：</font>

+ [科学数据平台（冷存储系统）数据使用指南](https://docs.hpc.sjtu.edu.cn/transport/archiveusage.html#archiveusage)

**<font style="color:rgba(0, 0, 0, 0.87);">场景四：将数据集发布在科学数据平台</font>**

[科学数据平台](https://scidata.sjtu.edu.cn/)<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">致力打造科学数据全生命周期管理平台，具有发布数据、共享数据、下载公共数据功能的数据平台，您可以将公开数据集发布在平台上。</font>

<font style="color:rgba(0, 0, 0, 0.87);">数据平台简明使用指南可以参考：</font>

+ [科学数据平台使用指南](https://docs.hpc.sjtu.edu.cn/transport/scidatausage.html#scidatausage)



另外我们需要关注的是对整个文件系统的选择：

[https://docs.hpc.sjtu.edu.cn/transport/transportsolution.html](https://docs.hpc.sjtu.edu.cn/transport/transportsolution.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741509613127-0625dd79-c8cf-43d0-8b0c-16641e1c2fec.png)

少量的数据直接使用具有SFTP功能的ssh客户端进行传输，或者使用linux数据传输工具，建议在数据传输节点进行操作，不要在登入节点操作！！！![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741509712238-bbc7d73b-0bbf-46f9-ad11-b66f3ca29a7b.png)





# 六，提交作业
[https://docs.hpc.sjtu.edu.cn/job/index.html](https://docs.hpc.sjtu.edu.cn/job/index.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741509966691-8dba5072-52db-446b-81ad-569dcb0ac660.png)	

## 1，交我算集群队列介绍[¶](https://docs.hpc.sjtu.edu.cn/job/partition.html#id1)
## <font style="color:rgba(0, 0, 0, 0.87);">思源一号集群</font>[¶](https://docs.hpc.sjtu.edu.cn/job/partition.html#id2)
<font style="color:rgba(0, 0, 0, 0.87);">思源一号集群设置以下队列，使用限制与说明如下</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">队列名</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">说明</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">64c512g</font> | <font style="color:rgba(0, 0, 0, 0.87);">允许单作业CPU核数为1~60000，每核配比8G内存；单节点配置为64核，512G内存</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">a100</font> | <font style="color:rgba(0, 0, 0, 0.87);">允许单作业GPU卡数为1~92，推荐每卡配比CPU为16，每CPU配比8G内存；单节点配置为64核，512G内存，4块40G显存的A100卡</font> |


<font style="color:rgba(0, 0, 0, 0.87);">该集群另外设置了调试队列 debug64c512g 和 debuga100 ，仅用于短时间测试，请勿批量投递作业进行完整计算。 debug64c512g 作业最多申请2节点，运行60分钟。 debuga100 作业最多申请1节点，运行20分钟。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">π 2.0 和 AI 集群</font>[¶](https://docs.hpc.sjtu.edu.cn/job/partition.html#ai)
<font style="color:rgba(0, 0, 0, 0.87);">π 2.0 和 AI 集群设置以下队列，使用限制与说明如下</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">队列名</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">说明</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">cpu</font> | <font style="color:rgba(0, 0, 0, 0.87);">允许单作业CPU核数为1~24000，每核配比4G内存，节点可共享使用；单节点配置为40核，192G内存</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">huge</font> | <font style="color:rgba(0, 0, 0, 0.87);">允许单作业CPU核数为6~80，每核配比35G内存，节点可共享使用；单节点配置为80核，3T内存</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">192c6t</font> | <font style="color:rgba(0, 0, 0, 0.87);">允许单作业CPU核数为48~192，每核配比31G内存，节点可共享使用；单节点配置为192核，6T内存</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">dgx2</font> | <font style="color:rgba(0, 0, 0, 0.87);">允许单作业GPU卡数为1~128，推荐每卡配比CPU为6，每CPU配比15G内存；单节点配置为96核，1.45T内存，16块32G显存的V100卡</font> |


<font style="color:rgba(0, 0, 0, 0.87);">π 2.0集群也设置了 debug 队列用于短时间测试，作业最多申请2节点，最长运行时间为20分钟。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">ARM 集群</font>[¶](https://docs.hpc.sjtu.edu.cn/job/partition.html#arm)
<font style="color:rgba(0, 0, 0, 0.87);">ARM 集群设置以下队列，使用限制与说明如下</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">队列名</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">说明</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">arm128c256g</font> | <font style="color:rgba(0, 0, 0, 0.87);">允许单作业CPU核数为1~12800，每核配比2G内存；单节点配置为128核，256G内存</font> |


<font style="color:rgba(0, 0, 0, 0.87);">以上信息在各集群登录界面均有展示。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">各队列默认运行时长</font>[¶](https://docs.hpc.sjtu.edu.cn/job/partition.html#id3)
<font style="color:rgba(0, 0, 0, 0.87);">huge 和 192c6t 队列默认的作业运行最长时间为 2 天，其余队列默认的作业运行最长时间为 7 天。</font>

<font style="color:rgba(0, 0, 0, 0.87);">若预计超出 7 天，需提前 2 天发邮件告知用户名和 jobID 以便延长时限。延长后的作业最长运行时间不超过 14 天。</font>

<font style="color:rgba(0, 0, 0, 0.87);"></font>

## 2，Slurm 作业调度系统
——》可以参考我之前HPC系列的指南博客

[SLURM](http://slurm.schedmd.com/)<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">（Simple Linux Utility for Resource Management）是一种可扩展的工作负载管理器，已被全世界的国家超级计算机中心广泛采用。 它是免费且开源的，根据</font>[GPL通用公共许可证](http://www.gnu.org/licenses/gpl.html)<font style="color:rgba(0, 0, 0, 0.87);">发行。</font>

<font style="color:rgba(0, 0, 0, 0.87);">本文档将协助您通过 Slurm 管理作业。 在这里可以找到更多的工作样本。</font>

<font style="color:rgba(0, 0, 0, 0.87);">如果我们可以提供任何帮助，请随时联系</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>[HPC 邮箱](mailto:hpc%40sjtu.edu.cn)<font style="color:rgba(0, 0, 0, 0.87);">。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741510063096-28aea211-9a65-44f0-acfa-f19d1d213c2e.png)

**<font style="color:rgba(0, 0, 0, 0.87);">小技巧</font>**

<font style="color:rgba(0, 0, 0, 0.87);">由于跨系统文本编码的问题，我们强烈建议您只用英文字符和数字命名文件夹和目录，并且不要使用特殊字符，以确保作业能顺利运行。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">Slurm 概览</font>[¶](https://docs.hpc.sjtu.edu.cn/job/slurm.html#id2)
| **<font style="color:rgba(0, 0, 0, 0.87);">Slurm</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">功能</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">sinfo</font> | <font style="color:rgba(0, 0, 0, 0.87);">集群状态</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">squeue</font> | <font style="color:rgba(0, 0, 0, 0.87);">排队作业状态</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">sbatch</font> | <font style="color:rgba(0, 0, 0, 0.87);">作业提交</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">scontrol</font> | <font style="color:rgba(0, 0, 0, 0.87);">查看和修改作业参数</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">sacct</font> | <font style="color:rgba(0, 0, 0, 0.87);">已完成作业报告</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">scancel</font> | <font style="color:rgba(0, 0, 0, 0.87);">删除作业</font> |


## `sinfo`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">查看集群状态</font>[¶](https://docs.hpc.sjtu.edu.cn/job/slurm.html#sinfo)
| **<font style="color:rgba(0, 0, 0, 0.87);">Slurm</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">功能</font>** |
| :--- | :--- |
| `sinfo -N` | <font style="color:rgba(0, 0, 0, 0.87);">查看节点级信息</font> |
| `sinfo -N --states=idle` | <font style="color:rgba(0, 0, 0, 0.87);">查看可用节点信息</font> |
| `sinfo --partition=cpu` | <font style="color:rgba(0, 0, 0, 0.87);">查看队列信息</font> |
| `sinfo --help` | <font style="color:rgba(0, 0, 0, 0.87);">查看所有选项</font> |


<font style="color:rgba(0, 0, 0, 0.87);">节点状态包括：</font>

`drain`<font style="color:rgba(0, 0, 0, 0.87);">(节点故障)，</font>`alloc`<font style="color:rgba(0, 0, 0, 0.87);">(节点在用)，</font>`idle`<font style="color:rgba(0, 0, 0, 0.87);">(节点可用)，</font>`down`<font style="color:rgba(0, 0, 0, 0.87);">(节点下线)，</font>`mix`<font style="color:rgba(0, 0, 0, 0.87);">(节点部分占用，但仍有剩余资源）。</font>

<font style="color:rgba(0, 0, 0, 0.87);">查看总体资源信息：</font>

```plain
$ sinfo
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
cpu         up  7-00:00:0    656   idle cas[001-656]
dgx2        up  7-00:00:0      8   idle vol[01-08]
```

## `squeue`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">查看作业信息</font>[¶](https://docs.hpc.sjtu.edu.cn/job/slurm.html#squeue)
| **<font style="color:rgba(0, 0, 0, 0.87);">Slurm</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">功能</font>** |
| :--- | :--- |
| `squeue -j jobid` | <font style="color:rgba(0, 0, 0, 0.87);">查看作业信息</font> |
| `squeue -l` | <font style="color:rgba(0, 0, 0, 0.87);">查看细节信息</font> |
| `squeue -n HOST` | <font style="color:rgba(0, 0, 0, 0.87);">查看特定节点作业信息</font> |
| `squeue` | <font style="color:rgba(0, 0, 0, 0.87);">查看USER_LIST的作业</font> |
| `squeue --state=R` | <font style="color:rgba(0, 0, 0, 0.87);">查看特定状态的作业</font> |
| `squeue --help` | <font style="color:rgba(0, 0, 0, 0.87);">查看所有的选项</font> |


<font style="color:rgba(0, 0, 0, 0.87);">作业状态包括</font>`R`<font style="color:rgba(0, 0, 0, 0.87);">(正在运行)，</font>`PD`<font style="color:rgba(0, 0, 0, 0.87);">(正在排队)，</font>`CG`<font style="color:rgba(0, 0, 0, 0.87);">(即将完成)，</font>`CD`<font style="color:rgba(0, 0, 0, 0.87);">(已完成)。</font>

<font style="color:rgba(0, 0, 0, 0.87);">默认情况下，</font>`squeue`<font style="color:rgba(0, 0, 0, 0.87);">只会展示在排队或在运行的作业。</font>

```plain
$ squeue
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
18046      dgx2   ZXLing     eenl  R    1:35:53      1 vol04
17796      dgx2   python    eexdl  R 3-00:22:04      1 vol02
```

<font style="color:rgba(0, 0, 0, 0.87);">显示您自己账户下的作业：</font>

```plain
squeue
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
17923      dgx2     bash    hpcwj  R 1-12:59:05      1 vol05
```

`-l`<font style="color:rgba(0, 0, 0, 0.87);">选项可以显示更细节的信息。</font>

```plain
squeue
JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
17923      dgx2     bash    hpcwj  RUNNING 1-13:00:53 30-00:00:00    1 vol05
```

## `SBATCH`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">作业提交</font>[¶](https://docs.hpc.sjtu.edu.cn/job/slurm.html#sbatch)
<font style="color:rgba(0, 0, 0, 0.87);">准备作业脚本然后通过</font>`sbatch`<font style="color:rgba(0, 0, 0, 0.87);">提交是 Slurm 的最常见用法。 为了将作业脚本提交给作业系统，Slurm 使用</font>

```plain
$ sbatch jobscript.slurm
```

<font style="color:rgba(0, 0, 0, 0.87);">Slurm 具有丰富的参数集。 以下最常用的。</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">Slurm</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">含义</font>** |
| :--- | :--- |
| `-n [count]` | <font style="color:rgba(0, 0, 0, 0.87);">总进程数</font> |
| `--ntasks-per-node=[count]` | <font style="color:rgba(0, 0, 0, 0.87);">每台节点上的进程数</font> |
| `-p [partition]` | <font style="color:rgba(0, 0, 0, 0.87);">作业队列</font> |
| `--job-name=[name]` | <font style="color:rgba(0, 0, 0, 0.87);">作业名</font> |
| `--output=[file_name]` | <font style="color:rgba(0, 0, 0, 0.87);">标准输出文件</font> |
| `--error=[file_name]` | <font style="color:rgba(0, 0, 0, 0.87);">标准错误文件</font> |
| `--time=[dd-hh:mm:ss]` | <font style="color:rgba(0, 0, 0, 0.87);">作业最大运行时长</font> |
| `--exclusive` | <font style="color:rgba(0, 0, 0, 0.87);">独占节点</font> |
| `--mail-type=[type]` | <font style="color:rgba(0, 0, 0, 0.87);">通知类型，可选 all, fail, end，分别对应全通知、故障通知、结束通知</font> |
| `--mail-user=[mail_address]` | <font style="color:rgba(0, 0, 0, 0.87);">通知邮箱</font> |
| `--nodelist=[nodes]` | <font style="color:rgba(0, 0, 0, 0.87);">偏好的作业节点</font> |
| `--exclude=[nodes]` | <font style="color:rgba(0, 0, 0, 0.87);">避免的作业节点</font> |
| `--depend=[state:job_id]` | <font style="color:rgba(0, 0, 0, 0.87);">作业依赖</font> |
| `--array=[array_spec]` | <font style="color:rgba(0, 0, 0, 0.87);">序列作业</font> |


<font style="color:rgba(0, 0, 0, 0.87);">这是一个名为</font>`cpu.slurm`<font style="color:rgba(0, 0, 0, 0.87);">的作业脚本，该脚本向cpu队列申请1个节点40核，并在作业完成时通知。在此作业中执行的命令是</font>`/bin/hostname`<font style="color:rgba(0, 0, 0, 0.87);">。</font>

```plain
#!/bin/bash

#SBATCH --job-name=hostname
#SBATCH --partition=cpu
#SBATCH -N 1
#SBATCH --mail-type=end
#SBATCH --mail-user=YOU@EMAIL.COM
#SBATCH --output=%j.out
#SBATCH --error=%j.err

/bin/hostname
```

<font style="color:rgba(0, 0, 0, 0.87);">用以下方式提交作业：</font>

```plain
sbatch cpu.slurm
```

`squeue`<font style="color:rgba(0, 0, 0, 0.87);">可用于检查作业状态。用户可以在作业执行期间通过SSH登录到计算节点。输出将实时更新到文件[jobid] .out和[jobid] .err。</font>

<font style="color:rgba(0, 0, 0, 0.87);">这里展示一个更复杂的作业要求，其中将启动80个进程，每台主机40个进程。</font>

```plain
#!/bin/bash

#SBATCH --job-name=LINPACK
#SBATCH --partition=cpu
#SBATCH -n 80
#SBATCH --ntasks-per-node=40
#SBATCH --mail-type=end
#SBATCH --mail-user=YOU@EMAIL.COM
#SBATCH --output=%j.out
#SBATCH --error=%j.err
```

<font style="color:rgba(0, 0, 0, 0.87);">以下作业请求4张GPU卡，其中1个CPU进程管理1张GPU卡。</font>

```plain
#!/bin/bash

#SBATCH --job-name=GPU_HPL
#SBATCH --partition=dgx2
#SBATCH -n 4
#SBATCH --ntasks-per-node=4
#SBATCH --gres=gpu:4
#SBATCH --mail-type=end
#SBATCH --mail-user=YOU@MAIL.COM
#SBATCH --output=%j.out
#SBATCH --error=%j.err
```

<font style="color:rgba(0, 0, 0, 0.87);">以下作业启动一个3任务序列（从0到2），每个任务需要1个CPU内核。关于集群上的Python，您可以查阅我们的</font>[Python文档](https://docs.hpc.sjtu.edu.cn/app/compilers_and_languages/python.html)<font style="color:rgba(0, 0, 0, 0.87);">。</font>

```plain
#!/bin/bash

#SBATCH --job-name=python_array
#SBATCH --mail-user=YOU@MAIL.COM
#SBATCH --mail-type=ALL
#SBATCH --ntasks=1
#SBATCH --time=00:30:00
#SBATCH --array=0-2
#SBATCH --output=python_array_%A_%a.out
#SBATCH --output=python_array_%A_%a.err

module load miniconda2/4.6.14-gcc-4.8.5

source activate YOUR_ENV_NAME

echo "SLURM_JOBID: " $SLURM_JOBID
echo "SLURM_ARRAY_TASK_ID: " $SLURM_ARRAY_TASK_ID
echo "SLURM_ARRAY_JOB_ID: " $SLURM_ARRAY_JOB_ID

python < vec_${SLURM_ARRAY_TASK_ID}.py
```

## `srun`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">和</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`salloc`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">交互式作业</font>[¶](https://docs.hpc.sjtu.edu.cn/job/slurm.html#srun-salloc)
`srun`<font style="color:rgba(0, 0, 0, 0.87);">可以启动交互式作业。该操作将阻塞，直到完成或终止。例如，在计算主机上运行</font>`hostname`<font style="color:rgba(0, 0, 0, 0.87);">。</font>

```plain
$ srun -N 1 -n 4 -p cpu hostname
cas006
```

<font style="color:rgba(0, 0, 0, 0.87);">启动远程主机bash终端：</font>

```plain
srun -p cpu -n 4 --pty /bin/bash
```

<font style="color:rgba(0, 0, 0, 0.87);">或者，可以通过</font>`salloc`<font style="color:rgba(0, 0, 0, 0.87);">请求资源，然后在获取节点后登录到计算节点：</font>

```plain
salloc -N 1 -n 4 -p cpu
ssh casxxx
```

`scontrol`<font style="color:rgba(0, 0, 0, 0.87);">: 查看和修改作业参数</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">Slurm</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">功能</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">scontrol show job JOB_ID</font> | <font style="color:rgba(0, 0, 0, 0.87);">查看排队或正在运行的作业的信息</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">scontrol hold JOB_ID</font> | <font style="color:rgba(0, 0, 0, 0.87);">暂停JOB_ID</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">scontrol release JOB_ID</font> | <font style="color:rgba(0, 0, 0, 0.87);">恢复JOB_ID</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">scontrol update dependency=JOB_ID</font> | <font style="color:rgba(0, 0, 0, 0.87);">添加作业依赖性 ，以便仅在JOB_ID完成后才开始作业</font> |


<font style="color:rgba(0, 0, 0, 0.87);">scontrol hold 命令可使排队中尚未运行的作业暂停被分配运行，被挂起的作业将不被执行。scontrol release 命令可取消挂起。</font>

`sacct`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">查看作业记录</font>

| **<font style="color:rgba(0, 0, 0, 0.87);">Slurm</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">功能</font>** |
| :--- | :--- |
| `sacct -l` | <font style="color:rgba(0, 0, 0, 0.87);">查看详细的帐户作业信息</font> |
| `sacct --states=R` | <font style="color:rgba(0, 0, 0, 0.87);">查看具有特定状态的作业的帐号作业信息</font> |
| `sacct -S YYYY-MM-DD` | <font style="color:rgba(0, 0, 0, 0.87);">在指定时间后选择处于任意状态的作业</font> |
| `sacct --format=“LAYOUT”` | <font style="color:rgba(0, 0, 0, 0.87);">使用给定的LAYOUT自定义sacct输出</font> |
| `sacct --help` | <font style="color:rgba(0, 0, 0, 0.87);">查看所有选项</font> |


<font style="color:rgba(0, 0, 0, 0.87);">默认情况下，sacct显示过去</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>**<font style="color:rgba(0, 0, 0, 0.87);">24小时</font>**<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">的帐号作业信息。</font>

```plain
$ sacct
```

<font style="color:rgba(0, 0, 0, 0.87);">查看更多的信息：</font>

```plain
$ sacct --format=jobid,jobname,account,partition,ntasks,alloccpus,elapsed,state,exitcode -j 3224
```

<font style="color:rgba(0, 0, 0, 0.87);">查看平均作业内存消耗和最大内存消耗：</font>

```plain
$ sacct --format="JobId,AveRSS,MaxRSS" -P -j xxx
```

## <font style="color:rgba(0, 0, 0, 0.87);">Slurm环境变量</font>[¶](https://docs.hpc.sjtu.edu.cn/job/slurm.html#id3)
| **<font style="color:rgba(0, 0, 0, 0.87);">Slurm</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">功能</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_JOB_ID</font> | <font style="color:rgba(0, 0, 0, 0.87);">作业ID</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_JOB_NAME</font> | <font style="color:rgba(0, 0, 0, 0.87);">作业名</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_JOB_PARTITION</font> | <font style="color:rgba(0, 0, 0, 0.87);">队列的名称</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_NTASKS</font> | <font style="color:rgba(0, 0, 0, 0.87);">进程总数</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_NTASKS_PER_NODE</font> | <font style="color:rgba(0, 0, 0, 0.87);">每个节点请求的任务数</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_JOB_NUM_NODES</font> | <font style="color:rgba(0, 0, 0, 0.87);">节点数</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_JOB_NODELIST</font> | <font style="color:rgba(0, 0, 0, 0.87);">节点列表</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_LOCALID</font> | <font style="color:rgba(0, 0, 0, 0.87);">作业中流程的节点本地任务ID</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_ARRAY_TASK_ID</font> | <font style="color:rgba(0, 0, 0, 0.87);">作业序列中的任务ID</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_SUBMIT_DIR</font> | <font style="color:rgba(0, 0, 0, 0.87);">工作目录</font> |
| <font style="color:rgba(0, 0, 0, 0.87);">$SLURM_SUBMIT_HOST</font> | <font style="color:rgba(0, 0, 0, 0.87);">提交作业的主机名</font> |


## <font style="color:rgba(0, 0, 0, 0.87);">参考教学视频</font>[¶](https://docs.hpc.sjtu.edu.cn/job/slurm.html#id4)
[2022 春季用户培训之slurm调度系统](https://vshare.sjtu.edu.cn/play/1dd022b54619d64368c57de58f81b0d9)

## <font style="color:rgba(0, 0, 0, 0.87);">参考资料</font>[¶](https://docs.hpc.sjtu.edu.cn/job/slurm.html#id5)
+ [SLURM Workload Manager](http://slurm.schedmd.com/)
+ [ACCRE’s SLURM Documentation](http://www.accre.vanderbilt.edu/?page_id=2154)
+ [Introduction to SLURM (NCCS lunchtime series)](http://www.nccs.nasa.gov/images/intro-to-slurm-20131218.pdf)



## 3，作业示例（基本）[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#id1)
<font style="color:rgba(0, 0, 0, 0.87);">根据集群的不同队列、不同应用软件，示例 slurm 作业脚本。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">作业提交流程</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#id2)
1. <font style="color:rgba(0, 0, 0, 0.87);">编写作业脚本</font>

```plain
vi test.slurm  # 根据需求，选择计算资源：CPU 或 GPU、所需核数、是否需要大内存
```

1. <font style="color:rgba(0, 0, 0, 0.87);">提交作业</font>

```plain
sbatch test.slurm
```

1. <font style="color:rgba(0, 0, 0, 0.87);">查看作业和资源</font>

```plain
squeue       # 查看正在排队或运行的作业
```

<font style="color:rgba(0, 0, 0, 0.87);">或</font>

```plain
sacct        # 查看过去 24 小时内已完成的作业
```

<font style="color:rgba(0, 0, 0, 0.87);">集群资源实时状态查询</font>

```plain
sinfo        # 若有 idle 或 mix 状态的节点，排队会比较快
```

## <font style="color:rgba(0, 0, 0, 0.87);">各队列作业示例</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#id3)
### <font style="color:rgba(0, 0, 0, 0.87);">cpu</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#cpu)
<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例：单节点不满核（例如20核），共享使用节点</font>

```plain
#!/bin/bash

#SBATCH --job-name=test        # 作业名
#SBATCH --partition=cpu        # cpu 队列
#SBATCH -n 20                 # 总核数 20
#SBATCH --ntasks-per-node=20   # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err
```

<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例：独占单节点不满核（20核），比如为了独占整个节点的大内存</font>

```plain
#!/bin/bash

#SBATCH --job-name=test        # 作业名
#SBATCH --partition=cpu        # cpu 队列
#SBATCH -n 20                 # 总核数 20
#SBATCH --ntasks-per-node=20   # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --exclusive            # 独占节点（独占整个节点的大内存，按照满核计费）
```

<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例：单节点满核（40 核）</font>

```plain
#!/bin/bash

#SBATCH --job-name=test        # 作业名
#SBATCH --partition=cpu        # cpu 队列
#SBATCH -n 40                 # 总核数 40
#SBATCH --ntasks-per-node=40   # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err
```

<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例：多节点（160 核）</font>

```plain
#!/bin/bash

#SBATCH --job-name=test        # 作业名
#SBATCH --partition=cpu        # cpu 队列
#SBATCH -n 160                # 总核数 160
#SBATCH --ntasks-per-node=40   # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err
```

### <font style="color:rgba(0, 0, 0, 0.87);">huge</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#huge)
<font style="color:rgba(0, 0, 0, 0.87);">huge 队列 slurm 脚本示例：单节点（20 核，最高可用 80 核）</font>

```plain
#!/bin/bash

#SBATCH --job-name=test         # 作业名
#SBATCH --partition=huge        # huge 队列
#SBATCH -n 20 # 总核数 20
#SBATCH --ntasks-per-node=20    # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err
```

### <font style="color:rgba(0, 0, 0, 0.87);">192c6t</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#c6t)
<font style="color:rgba(0, 0, 0, 0.87);">192c6t 队列 slurm 脚本示例：单节点（96 核，最高可用 192 核）</font>

```plain
#!/bin/bash

#SBATCH --job-name=test        # 作业名
#SBATCH --partition=192c6      # 192c6t 队列
#SBATCH -n 96                 # 总核数 96
#SBATCH --ntasks-per-node=96   # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err
```

### <font style="color:rgba(0, 0, 0, 0.87);">dgx2</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#dgx2)
<font style="color:rgba(0, 0, 0, 0.87);">dgx2 队列 slurm 脚本示例：单节点，分配 2 块 GPU，GPU:CPU 配比 1:6</font>

```plain
#!/bin/bash

#SBATCH --job-name=test        # 作业名
#SBATCH --partition=dgx2       # dgx2 队列
#SBATCH -N 1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=12     # 1:6 的 GPU:CPU 配比
#SBATCH --gres=gpu:2           # 2 块 GPU
#SBATCH --output=%j.out
#SBATCH --error=%j.err
```

### <font style="color:rgba(0, 0, 0, 0.87);">arm128c256g</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#arm128c256g)
<font style="color:rgba(0, 0, 0, 0.87);">arm128c256g 队列 slurm 脚本示例：单节点60核</font>

```plain
#!/bin/bash

#SBATCH --job-name=test
#SBATCH --partition=arm128c256g
#SBATCH -N 1
#SBATCH --ntasks-per-node=60
#SBATCH --output=%j.out
#SBATCH --error=%j.err

source /lustre/share/singularity/commercial-app/vasp/activate arm

mpirun -n $SLURM_NTASKS vasp_std
```

## <font style="color:rgba(0, 0, 0, 0.87);">常用软件作业示例</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#id4)
<font style="color:rgba(0, 0, 0, 0.87);">下面根据不同应用软件，示例 slurm 作业脚本</font>

### <font style="color:rgba(0, 0, 0, 0.87);">LAMMPS 作业示例</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#lammps)
<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例 LAMMPS</font>

```plain
#!/bin/bash

#SBATCH --job-name=test         # 作业名
#SBATCH --partition=cpu         # cpu 队列
#SBATCH -n 80                  # 总核数 80
#SBATCH --ntasks-per-node=40    # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err

module load lammps

srun --mpi=pmi2 lmp -i YOUR_INPUT_FILE
```

### <font style="color:rgba(0, 0, 0, 0.87);">GROMACS 作业示例</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#gromacs)
<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例 GROMACS</font>

```plain
#!/bin/bash

#SBATCH --job-name=test         # 作业名
#SBATCH --partition=cpu         # cpu 队列
#SBATCH -n 80                  # 总核数 80
#SBATCH --ntasks-per-node=40    # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err

module load gromacs/2020-cpu

srun --mpi=pmi2 gmx_mpi mdrun -deffnm -s test.tpr -ntomp 1
```

### <font style="color:rgba(0, 0, 0, 0.87);">Quantum ESPRESSO</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#quantum-espresso)
<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例 Quantum ESPRESSO</font>

```plain
#!/bin/bash

#SBATCH --job-name=test         # 作业名
#SBATCH --partition=cpu         # cpu 队列
#SBATCH -n 80                  # 总核数 80
#SBATCH --ntasks-per-node=40    # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err

module load quantum-espresso

srun --mpi=pmi2 pw.x -i test.in
```

### <font style="color:rgba(0, 0, 0, 0.87);">OpenFOAM</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#openfoam)
<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例 OpenFoam</font>

```plain
#!/bin/bash

#SBATCH --job-name=test         # 作业名
#SBATCH --partition=cpu         # cpu 队列
#SBATCH -n 80                  # 总核数 80
#SBATCH --ntasks-per-node=40    # 每节点核数
#SBATCH --output=%j.out
#SBATCH --error=%j.err

module load openfoam

srun --mpi=pmi2 icoFoam -parallel
```

### <font style="color:rgba(0, 0, 0, 0.87);">TensorFlow</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#tensorflow)
<font style="color:rgba(0, 0, 0, 0.87);">gpu 队列 slurm 脚本示例 TensorFlow</font>

```plain
#!/bin/bash

#SBATCH -J test
#SBATCH -p dgx2
#SBATCH -o %j.out
#SBATCH -e %j.err
#SBATCH -N 1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=12
#SBATCH --gres=gpu:2

module load miniconda3
source activate tf-env

python -c ’import tensorflow as tf; \
       print(tf.__version__); \
       print(tf.test.is_gpu_available());’
```

## <font style="color:rgba(0, 0, 0, 0.87);">其它示例</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#id5)
### <font style="color:rgba(0, 0, 0, 0.87);">Job Array 阵列作业</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#job-array)
<font style="color:rgba(0, 0, 0, 0.87);">一批作业，若所需资源和内容相似，可借助 Job Array 批量提交。Job Array 中的每一个作业在调度时视为独立的作业。</font>

<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例 array</font>

```plain
#!/bin/bash

#SBATCH --job-name=test           # 作业名
#SBATCH --partition=cpu           # cpu 队列
#SBATCH -n 1                      # 总核数 1
#SBATCH --ntasks-per-node=1       # 每节点核数
#SBATCH --output=array_%A_%a.out
#SBATCH --error=array_%A_%a.err
#SBATCH --array=1-20%10           # 总共 20 个子任务，每次最多同时运行 10 个

echo $SLURM_ARRAY_TASK_ID
```

### <font style="color:rgba(0, 0, 0, 0.87);">作业状态邮件提醒</font>[¶](https://docs.hpc.sjtu.edu.cn/job/jobsample1.html#id6)
<font style="color:rgba(0, 0, 0, 0.87);">--mail-type= 指定状态发生时，发送邮件通知: ALL, BEGIN, END, FAIL</font>

<font style="color:rgba(0, 0, 0, 0.87);">cpu 队列 slurm 脚本示例：邮件提醒</font>

```plain
#!/bin/bash

#SBATCH --job-name=test
#SBATCH --partition=cpu
#SBATCH -n 20
#SBATCH --ntasks-per-node=20
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --mail-type=end           # 作业结束时，邮件提醒
#SBATCH --mail-user=XX@sjtu.edu.cn
```



## 4，查看作业资源信息
<font style="color:rgba(0, 0, 0, 0.87);">当作业对CPU和内存的要求较高时，了解运行作业的CPU和内存的使用信息，能够保证作业顺利运行。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">内存</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#id2)
### <font style="color:rgba(0, 0, 0, 0.87);">内存分配策略</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#id3)
| **<font style="color:rgba(0, 0, 0, 0.87);">集群</font>** | **<font style="color:rgba(0, 0, 0, 0.87);">存储分配策略</font>** |
| :--- | :--- |
| <font style="color:rgba(0, 0, 0, 0.87);">π2.0</font> | <font style="color:rgba(0, 0, 0, 0.87);">单节点配置为40核，180G内存；每核配比4G内存</font> |


<font style="color:rgba(0, 0, 0, 0.87);">可使用</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`seff jobid`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">命令查看单核所能使用的内存空间</font>

```plain
[hpc@login2 data]$ seff 9709905
Job ID: 9709905
Cluster: sjtupi
User/Group: hpchgc/hpchgc
State: RUNNING
Nodes: 1
Cores per node: 40
CPU Utilized: 00:00:00
CPU Efficiency: 0.00% of 02:22:40 core-walltime
Job Wall-clock time: 00:03:34
Memory Utilized: 0.00 MB (estimated maximum)
Memory Efficiency: 0.00% of 160.00 GB (4.00 GB/core)              //(4.00 GB/core)
WARNING: Efficiency statistics may be misleading for RUNNING jobs.
```

### <font style="color:rgba(0, 0, 0, 0.87);">作业运行中的内存占用</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#id4)
<font style="color:rgba(0, 0, 0, 0.87);">当提交作业后，使用</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`squeue`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">命令查看作业使用的节点</font>

```plain
[hpc@login2 test]$ squeue
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           9709875       cpu  40cores      hpc  R       0:02      1 cas478
```

<font style="color:rgba(0, 0, 0, 0.87);">然后进入相关节点</font>

```plain
ssh cas478
```

<font style="color:rgba(0, 0, 0, 0.87);">可根据用户名查看作业占用的存储空间</font>

```plain
ps -u$USER -o %cpu,rss,args
```

<font style="color:rgba(0, 0, 0, 0.87);">示例如下：</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`ps -uhpc -o %cpu,rss,args`

```plain
%CPU    RSS COMMAND
98.5 633512 pw.x -i ausurf.in
98.5 652828 pw.x -i ausurf.in
98.6 654312 pw.x -i ausurf.in
98.6 652196 pw.x -i ausurf.in
```

`RSS`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">表示单核所占用的存储空间，单位为KB，上述分析可得单核上运行作业占用的存储空间大约为650MB,40核的内存利用率大约为：</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`(0.65G*40)/160G: 16%`<font style="color:rgba(0, 0, 0, 0.87);">。</font>

<font style="color:rgba(0, 0, 0, 0.87);">如果需要动态监测存储资源的使用，可进入计算节点后，输入top命令</font>

```plain
PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
428410 hpc       20   0 5989.9m 662.5m 168.6m R 100.0  0.3   0:22.67 pw.x
428419 hpc       20   0 5987.0m 658.9m 163.7m R 100.0  0.3   0:22.61 pw.x
428421 hpc       20   0 5984.6m 677.8m 180.1m R 100.0  0.4   0:22.66 pw.x
428433 hpc       20   0 6002.8m 661.7m 165.3m R 100.0  0.3   0:22.68 pw.x
428436 hpc       20   0 5986.0m 659.0m 165.4m R 100.0  0.3   0:22.66 pw.x
```

<font style="color:rgba(0, 0, 0, 0.87);">上述数据中的RES列数据表示运行作业所占用的存储资源，单核大约占用650m。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">作业运行结束后内存利用分析情况</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#id5)
<font style="color:rgba(0, 0, 0, 0.87);">使用</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`seff jobid`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">命令</font>

```plain
[hpc@login2 data]$ seff 9709905
Job ID: 9709905
Cluster: sjtupi
User/Group: hpchgc/hpchgc
State: COMPLETED (exit code 0)
Nodes: 1
Cores per node: 40
CPU Utilized: 06:27:20
CPU Efficiency: 99.15% of 06:30:40 core-walltime
Job Wall-clock time: 00:09:46
Memory Utilized: 23.33 GB
Memory Efficiency: 14.58% of 160.00 GB
```

## <font style="color:rgba(0, 0, 0, 0.87);">GPU INFO</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#gpu-info)
<font style="color:rgba(0, 0, 0, 0.87);">下面介绍如何通过JOB ID获取GPU信息以及如何获取CUDA_VISIBLE_DEVICES变量并在程序中利用该变量</font>

<font style="color:rgba(0, 0, 0, 0.87);">NVIDIA显卡的UUID（Universally Unique Identifier，通用唯一标识符）和BUS ID（总线标识符）是两个重要的标识符。</font>

<font style="color:rgba(0, 0, 0, 0.87);">UUID（Universally Unique Identifier）：UUID是一个128位的唯一标识符，用于在系统中识别每个独立的NVIDIA显卡设备。每个显卡设备都有一个唯一的UUID，可以通过调用相关命令或API来获取它。UUID在不同系统和环境中是持久的，即使重新启动系统或重新插拔显卡，UUID也不会改变。</font>

<font style="color:rgba(0, 0, 0, 0.87);">BUS ID（总线标识符）：BUS ID是用于标识系统中不同物理或逻辑总线上的NVIDIA显卡设备的标识符。BUS ID提供了关于显卡设备如何连接到系统总线的信息，如PCI总线等。BUS ID通常采用“domain:bus:device.function”的形式表示，其中domain表示域，bus表示总线编号，device表示设备编号，function表示设备的功能编号。BUS ID主要用于管理和配置显卡设备，以及确定显卡在系统中的位置。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">通过JOB ID获取GPU信息</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#job-idgpu)
<font style="color:rgba(0, 0, 0, 0.87);">下面介绍如何通过jobid查询UUID及BUSID信息：</font>

### <font style="color:rgba(0, 0, 0, 0.87);">脚本文件</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#id6)
<font style="color:rgba(0, 0, 0, 0.87);">将下面脚本保存至</font>`getGPUInfo`<font style="color:rgba(0, 0, 0, 0.87);">的文本中</font>

```plain
#!/bin/bash
JOBID=$1
GPUNUMS=`scontrol show jobs ${JOBID}|grep "gres:gpu" |awk -F ':' '{print $3}'`
GPUHOST=`scontrol show jobs ${JOBID}|grep "NodeList="|sed -n '2p'|awk -F '=' '{print $2}'`
echo JobID: ${JOBID}
echo NodeHost: ${GPUHOST}
echo GPUNums: ${GPUNUMS}
echo Information of allocate GPUs:
ssh ${GPUHOST} "echo UUID: && nvidia-smi -L"
ssh ${GPUHOST} "echo BUS_ID: && nvidia-smi -q|grep 'Bus Id'|sed -e 's/^.*: //' -e 's/ $//'"
```

### <font style="color:rgba(0, 0, 0, 0.87);">执行脚本加JOB ID</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#job-id)
<font style="color:rgba(0, 0, 0, 0.87);">针对正在运行的作业，执行脚本加作业ID可获取正在运行作业的节点中的GPU信息</font>

```plain
$ chmod +x getGPUInfo
$ ./getGPUInfo 27180318
JobID: 27180318
NodeHost: gpu09
GPUNums: 4
Information of allocate GPUs:
UUID:
GPU 0: NVIDIA A100-SXM4-40GB (UUID: GPU-5cd88acf-5391-8562-cd34-b543319224b4)
GPU 1: NVIDIA A100-SXM4-40GB (UUID: GPU-7bc1435d-37b5-d4b8-6ac1-df72927a54e0)
GPU 2: NVIDIA A100-SXM4-40GB (UUID: GPU-4dedf87e-d147-83cc-c5bd-ec16324afa15)
GPU 3: NVIDIA A100-SXM4-40GB (UUID: GPU-2bf8c199-1e4a-31cd-470b-4ba6329d9a60)
BUS_ID:
00000000:31:00.0
00000000:4B:00.0
00000000:CA:00.0
00000000:E3:00.0
```

## <font style="color:rgba(0, 0, 0, 0.87);">获取CUDA_VISIBLE_DEVICES</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#cuda-visible-devices)
<font style="color:rgba(0, 0, 0, 0.87);">CUDA_VISIBLE_DEVICES是一个环境变量，用于在使用CUDA编程时指定可见的GPU设备。它可以用来控制程序所使用的GPU设备的数量和顺序。</font>

<font style="color:rgba(0, 0, 0, 0.87);">当用户申请有GPU卡的任务时，slurm系统会根据用户申请的GPU数量来设置CUDA_VISIBLE_DEVICES环境变量，只有相应编号的GPU设备会对程序可见，其他GPU设备则不可使用。</font>

### <font style="color:rgba(0, 0, 0, 0.87);">srun交互式作业</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#srun)
<font style="color:rgba(0, 0, 0, 0.87);">在srun申请交互式作业后，可在shell中直接输出</font>`$CUDA_VISIBLE_DEVICES`<font style="color:rgba(0, 0, 0, 0.87);">变量</font>

```plain
$  srun -n 8 -p dgx2 --gres=gpu:2 --pty /bin/bash
srun: job 27182411 queued and waiting for resources
srun: job 27182411 has been allocated resources
$ echo $CUDA_VISIBLE_DEVICES
0,1
```

### <font style="color:rgba(0, 0, 0, 0.87);">作业脚本</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#id7)
<font style="color:rgba(0, 0, 0, 0.87);">也可以在作业脚本最前面输出</font>`$CUDA_VISIBLE_DEVICES`<font style="color:rgba(0, 0, 0, 0.87);">变量</font>

```plain
#!/bin/bash
#SBATCH -J test
#SBATCH -n 8
#SBATCH --gres=gpu:2
#SBATCH -p dgx2

echo $CUDA_VISIBLE_DEVICES
···
```

### <font style="color:rgba(0, 0, 0, 0.87);">案例测试</font>[¶](https://docs.hpc.sjtu.edu.cn/job/resource.html#id8)
<font style="color:rgba(0, 0, 0, 0.87);">以下是一个简单的torch程序，展示了根据</font>`$CUDA_VISIBLE_DEVICES`<font style="color:rgba(0, 0, 0, 0.87);">变量，设置程序使用的GPU</font>

```plain
$ cat pytorch_test.py
import torch
from torch import nn
from torch.optim import Adam
from torch.nn.parallel import DataParallel
import os
class DEMO_model(nn.Module):
        def __init__(self, in_size, out_size):
                super().__init__()
                self.fc = nn.Linear(in_size, out_size)
        def forward(self, inp):
                outp = self.fc(inp)
                print(inp.shape, outp.device)
                return outp
model = DEMO_model(10, 5).to('cuda')

os.system("echo CUDA_VISIBLE_DEVICES: $CUDA_VISIBLE_DEVICES")
device_ids = os.environ.get('CUDA_VISIBLE_DEVICES')
device_ids = device_ids.split(',')
device_ids = [int(number) for number in device_ids]
model = DataParallel(model, device_ids=device_ids)
adam = Adam(model.parameters())
# 进行训练
for i in range(1):
        x = torch.rand([128, 10])
        y = model(x)
        loss = torch.norm(y)
        loss.backward()
        adam.step()
```

<font style="color:rgba(0, 0, 0, 0.87);">执行程序，需要加载torch环境</font>

```plain
$ srun -n 8 -p dgx2 --gres=gpu:2 -w vol08 --pty /bin/bash
$ module load miniconda3
$ source activate
(base) $ conda activate pytorch-env
(pytorch-env) $ python pytorch_test.py
CUDA_VISIBLE_DEVICES: 0,1
torch.Size([64, 10]) cuda:0
torch.Size([64, 10]) cuda:1
```





其他队列的使用指南，参考：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741510552701-daec7416-c70e-4d2e-8623-875c8a1b35d5.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741510627232-7fcf80d5-986e-4fc1-b7d0-e1c50079212f.png)





## 七，容器
[https://docs.hpc.sjtu.edu.cn/container/index.html](https://docs.hpc.sjtu.edu.cn/container/index.html)

这一部分我用的不多，仅参考上述部分

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741510751213-e1b3126c-4f88-4344-8892-b11b4b2778d1.png)



# 八，软件模块
[https://docs.hpc.sjtu.edu.cn/app/index.html](https://docs.hpc.sjtu.edu.cn/app/index.html)

## 1，软件模块使用方法[¶](https://docs.hpc.sjtu.edu.cn/app/module.html#id1)
[https://docs.hpc.sjtu.edu.cn/app/module.html](https://docs.hpc.sjtu.edu.cn/app/module.html)

## <font style="color:rgba(0, 0, 0, 0.87);">module 命令</font>[¶](https://docs.hpc.sjtu.edu.cn/app/module.html#module)
<font style="color:rgba(0, 0, 0, 0.87);">集群软件以 module 形式供全局调用。常见的 module 命令如下</font>

`module load [MODULE]`<font style="color:rgba(0, 0, 0, 0.87);">: 加载模块</font>

`module avail`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">或</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`module av`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">: 列出所有模块</font>

`module av intel`<font style="color:rgba(0, 0, 0, 0.87);">: 列出含有 intel 名字的所有模块</font>

`module list`<font style="color:rgba(0, 0, 0, 0.87);">: 列出所有已加载的模块</font>

`module show [MODULE]`<font style="color:rgba(0, 0, 0, 0.87);">: 列出该模块的信息，如路径、环境变量等</font>

<font style="color:rgba(0, 0, 0, 0.87);">也可以一次加载或卸载多个模块。</font>

```plain
$ module load gcc openmpi
$ module unload gcc openmpi
```

<font style="color:rgba(0, 0, 0, 0.87);">如果您喜欢最新的稳定版本，则可以忽略版本号（默认加载带有 D 标识的版本）。</font>

<font style="color:rgba(0, 0, 0, 0.87);">下方两句命令效果一致：</font>

```plain
$ module load gcc openmpi
$ module load gcc/9.3.0-gcc-4.8.5 openmpi/4.0.5-gcc-9.2.0
```

```plain
----------------------------------------------------- /lustre/share/spack/modules/cascadelake/linux-centos7-x86_64 -----------------------------------------------------
abinit/8.10.3-gcc-9.2.0-openblas-openmpi        hdf5/1.10.6-gcc-9.2.0-openmpi                            netcdf-fortran/4.5.2-intel-19.0.4-impi   (D)
alphafold/2-python-3.8                          hdf5/1.10.6-gcc-9.2.0                                    netlib-lapack/3.8.0-intel-19.0.4         (D)
bcftools/1.9-gcc-9.2.0                   (D)    hdf5/1.10.6-intel-19.0.4-impi                            nwchem/6.8.1-intel-19.0.4-impi
bedtools2/2.27.1-intel-19.0.4            (D)    hdf5/1.10.6-intel-19.0.5-impi                            openblas/0.3.7-gcc-9.2.0                 (D)
bismark/0.19.0-intel-19.0.4                     hdf5/1.10.6-intel-19.0.5-openmpi                  (D)    openjdk/1.8.0_222-b10-gcc-9.2.0
boost/1.70.0-gcc-9.2.0                          hisat2/2.1.0-intel-19.0.4                         (D)    openjdk/11.0.2-gcc-9.2.0
boost/1.70.0-intel-19.0.4                       hypre/2.20.0-gcc-9.2.0-openblas-openmpi           (D)    openjdk/11.0.2-intel-19.0.4              (D)
boost/1.70.0-intel-19.0.5                (D)    intel-mkl/2019.3.199-intel-19.0.4                        openmpi/3.1.5-gcc-9.2.0
bowtie/1.2.3-gcc-9.2.0                   (D)    intel-mkl/2019.5.281-intel-19.0.5                        openmpi/3.1.5-gcc-9.3.0
bowtie2/2.3.5.1-intel-19.0.4                    intel-mkl/2020.1.217-intel-19.1.1                 (D)    openmpi/3.1.5-intel-19.0.5
bwa/0.7.17-gcc-9.2.0                            intel-mpi/2019.4.243-intel-19.0.4                        openmpi/4.0.5-gcc-9.2.0                  (D)
bwa/0.7.17-intel-19.0.4                  (D)    intel-mpi/2019.6.154-gcc-9.2.0                           perl/5.30.0-gcc-9.2.0
cdo/1.9.8-gcc-9.2.0                             intel-mpi/2019.6.154-intel-19.0.5                 (D)    perl/5.30.0-gcc-9.3.0
cp2k/8.2-gcc-9.2.0-openblas              (D)    intel-parallel-studio/cluster.2019.4-intel-19.0.4        perl/5.30.0-intel-19.0.4
cuda/9.0.176-intel-19.0.4                       intel-parallel-studio/cluster.2019.5-intel-19.0.5        perl/5.30.0-intel-19.0.5
cuda/10.1.243-gcc-9.2.0                         intel-parallel-studio/cluster.2020.1-intel-19.1.1 (D)    perl/5.30.0-intel-19.1.1                 (D)
cuda/10.2.89-intel-19.0.4                (D)    jasper/2.0.16-gcc-9.2.0                                  picard/2.19.0-gcc-9.2.0
cufflinks/2.2.1-gcc-9.2.0                       jdk/12.0.2_10-gcc-9.2.0                                  picard/2.19.0-intel-19.0.4               (D)
cufflinks/2.2.1-intel-19.0.4             (D)    jdk/12.0.2_10-intel-19.0.4                        (D)    python/2.7.16-intel-19.0.4
eigen/3.3.7-gcc-9.2.0                    (D)    json-fortran/7.1.0-gcc-9.2.0                             python/2.7.16-intel-19.1.1
fastqc/0.11.7-gcc-9.2.0                         lammps/20200721-intel-19.0.5-openmpi                     python/3.7.4-gcc-9.2.0
fastqc/0.11.7-intel-19.0.4               (D)    lammps/20210310-intel-19.0.5-openmpi                     python/3.7.4-intel-19.0.4
fftw/3.3.8-gcc-9.2.0-openmpi                    lammps/20210702-intel-19.0.5-impi                        python/3.7.4-intel-19.0.5
fftw/3.3.8-gcc-9.2.0                            libxc/4.3.2-gcc-9.2.0                                    quantum-espresso/6.4.1-intel-19.0.4-impi
fftw/3.3.8-gcc-9.3.0-openmpi                    libxc/4.3.2-intel-19.0.4                          (D)    quantum-espresso/6.4.1-intel-19.0.5-impi
fftw/3.3.8-intel-19.0.4-impi                    lumpy-sv/0.2.13-gcc-9.2.0                                quantum-espresso/6.5-intel-19.0.4-impi
fftw/3.3.8-intel-19.0.5-impi                    mcl/14-137-gcc-9.2.0                                     quantum-espresso/6.5-intel-19.0.5-impi
fftw/3.3.8-intel-19.0.5-openmpi                 megahit/1.1.4-intel-19.0.4                        (D)    rna-seqc/1.1.8-gcc-9.2.0
fftw/3.3.8-intel-19.1.1-impi                    metis/5.1.0-gcc-9.2.0                             (D)    rna-seqc/1.1.8-intel-19.0.4              (D)
fftw/3.3.9-gcc-9.2.0-openmpi                    mpich/3.3.2-gcc-9.2.0                                    rosettafold/1-python-3.8
fftw/3.3.9-gcc-9.3.0                     (D)    mpich/3.3.2-intel-19.0.4                                 samtools/1.9-gcc-9.2.0
flash/1.2.11-gcc-9.2.0                   (D)    mpich/3.3.2-intel-19.0.5                          (D)    samtools/1.9-intel-19.0.4                (D)
fsl/6.0-fsl-gcc-4.8.5                           mvapich2/2.3.2-intel-19.0.5                       (D)    siesta/4.0.1-intel-19.0.4-impi
gatk/3.8-1-gcc-9.2.0                     (D)    namd/2.14-gcc-9.2.0-openmpi                              stream/5.10-intel-19.0.4
gromacs/2019.2-gcc-9.2.0-openmpi                nano/4.7-gcc-4.8.5                                       stream/5.10-intel-19.0.5                 (D)
gromacs/2019.4-gcc-9.2.0-openmpi                nektar/5.0.0-intel-19.0.4-impi                           sumo/1.10.0-sumo
gromacs/2020.2-gcc-9.2.0-openmpi                netcdf-c/4.7.3-gcc-9.2.0-openmpi                         sundials/3.1.2-gcc-9.2.0
gromacs/2021-gcc-9.3.0-openmpi                  netcdf-c/4.7.3-gcc-9.2.0                                 svaba/1.1.3-gcc-4.8.5
gsl/2.5-gcc-9.2.0                               netcdf-c/4.7.3-intel-19.0.4-impi                         tophat/2.1.2-intel-19.0.4
gsl/2.5-intel-19.0.4                            netcdf-c/4.7.3-intel-19.0.5-impi                         vsearch/2.4.3-intel-19.0.4               (D)
gsl/2.5-intel-19.0.5                     (D)    netcdf-c/4.7.3-intel-19.0.5-openmpi               (D)    wrf/4.2-gcc-9.2.0-openmpi
hdf5/1.10.5-intel-19.0.4-impi                   netcdf-fortran/4.5.2-gcc-9.2.0-openmpi
```

<font style="color:rgba(0, 0, 0, 0.87);">在SLURM上，我们应用了以下规则来选取最合适的模块。</font>

1. <font style="color:rgba(0, 0, 0, 0.87);">编译器：如果加载了</font>`gcc`<font style="color:rgba(0, 0, 0, 0.87);">或</font>`icc`<font style="color:rgba(0, 0, 0, 0.87);">，请根据相应的编译器加载已编译的模块。或在必要时加载默认的编译器</font>`gcc`<font style="color:rgba(0, 0, 0, 0.87);">。</font>
2. <font style="color:rgba(0, 0, 0, 0.87);">MPI库：如果已加载其中一个库（</font>`openmpi`<font style="color:rgba(0, 0, 0, 0.87);">，</font>`impi`<font style="color:rgba(0, 0, 0, 0.87);">，</font>`mvapich2`<font style="color:rgba(0, 0, 0, 0.87);">，</font>`mpich`<font style="color:rgba(0, 0, 0, 0.87);">），加载针对相应MPI编译的模块。在必要的时候,默认MPI lib中的</font>`openmpi`<font style="color:rgba(0, 0, 0, 0.87);">将被装载。</font>
3. <font style="color:rgba(0, 0, 0, 0.87);">Module版本：每个模块均有默认版本，如果未指定版本号，则将加载该默认版本。</font>

## <font style="color:rgba(0, 0, 0, 0.87);">参考资料</font>[¶](https://docs.hpc.sjtu.edu.cn/app/module.html#id2)
+ <font style="color:rgba(0, 0, 0, 0.87);">Lmod: A New Environment Module System</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>[https://lmod.readthedocs.io/en/latest/](https://lmod.readthedocs.io/en/latest/)
+ <font style="color:rgba(0, 0, 0, 0.87);">Environment Modules Project</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>[http://modules.sourceforge.net/](http://modules.sourceforge.net/)
+ <font style="color:rgba(0, 0, 0, 0.87);">Modules Software Environment on NERSC </font>[https://www.nersc.gov/users/software/nersc-user-environment/modules/](https://www.nersc.gov/users/software/nersc-user-environment/modules/)

我么主要关注的就是其中的[https://docs.hpc.sjtu.edu.cn/app/bioinformatics/index.html](https://docs.hpc.sjtu.edu.cn/app/bioinformatics/index.html)（生信软件）



# 九，常见问题
[https://docs.hpc.sjtu.edu.cn/faq/index.html](https://docs.hpc.sjtu.edu.cn/faq/index.html)

个人挑选了几个我认为比较重要的：

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741511081056-8c9fbe9a-569e-4679-b759-1af94050bde8.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741511102859-3271cc41-3b58-439a-b8b4-c467f1125ad5.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741511115526-d586b518-e323-41c1-bac4-8ff5b40f5761.png)

### <font style="color:rgba(0, 0, 0, 0.87);">3.4 Q：计算节点不能访问互联网/不能下载数据</font>[¶](https://docs.hpc.sjtu.edu.cn/faq/index.html#id14)
**<font style="color:rgba(0, 0, 0, 0.87);">A：</font>**<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">计算节点是通过proxy节点代理进行网络访问的，因此一些软件需要特定的代理设置。需要找到软件的配置文件，修改软件的代理设置。</font>

1. <font style="color:rgba(0, 0, 0, 0.87);">git、wget、curl等软件支持通用变量，代理参数设置为：</font>

```plain
# 思源一号计算节点通用代理设置
https_proxy=http://proxy2.pi.sjtu.edu.cn:3128
http_proxy=http://proxy2.pi.sjtu.edu.cn:3128
no_proxy=puppet,proxy,172.16.0.133,pi.sjtu.edu.cn

 # π2.0计算节点通用代理设置
http_proxy=http://proxy.pi.sjtu.edu.cn:3004/
https_proxy=http://proxy.pi.sjtu.edu.cn:3004/
no_proxy=puppet
```

1. <font style="color:rgba(0, 0, 0, 0.87);">Python、MATLAB、Rstudio、fasterq-dump等软件需要查询软件官网确定配置参数：</font>

```plain
### fasterq-dump文件，配置文件路径 ~/.ncbi/user-settings.mkfg

# 思源一号节点代理设置
/tools/prefetch/download_to_cache = "true"
/http/proxy/enabled = "true"
/http/proxy/path = "http:/proxy2.pi.sjtu.edu.cn:3128"

# π2.0节点代理设置
/tools/prefetch/download_to_cache = "true"
/http/proxy/enabled = "true"
/http/proxy/path = "http://proxy.pi.sjtu.edu.cn:3004"

### Python需要在代码里面指定代理设置，不同Python包代理参数可能不同

# 思源一号节点代理设置
proxies = {
    'http': 'http://proxy2.pi.sjtu.edu.cn:3128',
    'https': 'http://proxy2.pi.sjtu.edu.cn:3128',
}
# π2.0节点代理设置
proxies = {
    'http': 'http://proxy.pi.sjtu.edu.cn:3004',
    'https': 'http://proxy.pi.sjtu.edu.cn:3004',
}

### MATLAB

# 思源一号节点代理设置
proxy2.pi.sjtu.edu.cn:3128

# π2.0节点代理设置
proxy.hpc.sjtu.edu.cn:3004
```

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741511149205-4e39ad7c-873c-42c6-b2e3-93b6d6cd0276.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741511162734-98671877-06a8-49b1-9553-ea61e91e9e9c.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741511168659-742cf278-07ba-43d9-b38e-a3acb7baaaac.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741511218882-95533119-058b-4cdb-b5bb-35409c25c648.png)

## <font style="color:rgba(0, 0, 0, 0.87);">10. 如何重置 .bashrc 和 .bash_profile</font>[¶](https://docs.hpc.sjtu.edu.cn/faq/index.html#bashrc-bash-profile)
**<font style="color:rgba(0, 0, 0, 0.87);">A：</font>**<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">用户家目录下的</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`~/.bashrc`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">和</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`~/.bash_profile`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">记录bash shell配置，若配置不当可能会导致无法找到可执行文件、无法在Studio中启动RSession等问题，需要重置这两个配置文件的内容。</font>

<font style="color:rgba(0, 0, 0, 0.87);">重置</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`~/.bashrc`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">操作流程如下，首先登录集群，然后备份现有配置文件，再调用</font><font style="color:rgba(0, 0, 0, 0.87);"> </font>`vim`<font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">或其他文本编辑器打开文件：</font>

```plain
$ /bin/cp ~/.bashrc ~/.bashrc.bak
$ /bin/vim ~/.bashrc
```

<font style="color:rgba(0, 0, 0, 0.87);">将</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">~/.bashrc</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">文件内容重置如下，保存后退出编辑器：</font>

```plain
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
```

<font style="color:rgba(0, 0, 0, 0.87);">类似地，在命令行中使用</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">/bin/cp ~/.bash_profile ~/.bash_profile.bak; /bin/vim ~/.bash_profile</font><font style="color:rgba(0, 0, 0, 0.87);"> </font><font style="color:rgba(0, 0, 0, 0.87);">将文件内容重置如下：</font>

```plain
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/.local/bin:$HOME/bin

export PATH
```

<font style="color:rgba(0, 0, 0, 0.87);">最后重新登录集群，确认重置配置文件后，先前的问题是否解决。 重置配置文件会导致您先前对bash shell的自定义配置失效，如果您仍需要保留这些自定义配置，建议您从bak备份文件中逐条转移这些配置，避免引入导致应用异常语句。</font>

