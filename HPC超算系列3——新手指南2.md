可以参考我的上一篇博客：

[https://blog.csdn.net/weixin_62528784/article/details/146122850?sharetype=blogdetail&sharerId=146122850&sharerefer=PC&sharesource=weixin_62528784&spm=1011.2480.3001.8118](https://blog.csdn.net/weixin_62528784/article/details/146122850?sharetype=blogdetail&sharerId=146122850&sharerefer=PC&sharesource=weixin_62528784&spm=1011.2480.3001.8118)

这一节主要是对上一节的一些内容的补充：

1，<font style="color:rgb(0,0,0);">Pi2.0 超算介绍：</font>

<font style="color:rgb(0,0,0);">我们教学用的队列主要是这个队列</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741443311493-b67bec90-d351-4730-b152-bf1a3ac738af.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741443517338-6ebcfde5-9504-4e5e-81a1-21c352cd2f9c.png)





2，<font style="color:rgb(0,0,0);">作业递交脚本模板</font>

```python
#!/bin/bash
#SBATCH --job-name=xxx #作业名称
#SBATCH --partition=xx #节点类型，即要使用哪个队列
#SBATCH -n xx #作业指定使用总核数，核心core数，有时也称之为进程数
#SBATCH --ntasks-per-node=xx #指定每个节点使用核数
#SBATCH --mail-type=end # 作业结束发送邮件
#SBATCH --mail-user=[your_mail_address] #指定发送邮件的地址
#SBATCH --output=%j.out #指定输出文件
#SBATCH --error=%j.err #指定报错文件

xxx #实际执行的命令
```

我们需要注意的是1个节点（我们可以理解为超算集群中的1台电脑）是有很多核心core数的，详情见1中的每个队列的展示；



我们此处再举几个例子，来加深一下最基本的slurm脚本理解，主要是其中的一些参数，都是我上课用的教学例子。

<font style="color:rgb(0,0,0);">编写作业脚本（</font><font style="color:rgb(0,0,0);">*.slurm</font><font style="color:rgb(0,0,0);">）</font><font style="color:rgb(0,0,0);">,</font><font style="color:rgb(0,0,0);">作业脚本示例： </font>

<font style="color:rgb(0,0,0);">该作业指定作业名字为 fastqc，指定 cpu 节点，作业使用总核数为 16 个核，每个节点分配 16 个核，作业计算结束给用户发送邮件，有标准的报错文件。</font>

```python
#!/bin/bash
#SBATCH --job-name=fastqc
#SBATCH --partition=cpu
#SBATCH -n 16
#SBATCH --ntasks-per-node=16
#SBATCH --mail-type=end
#SBATCH --mail-user=[your_mail_address] #请改成自己邮件的地址
#SBATCH --output=%j.out
#SBATCH --error=%j.err
module load fastqc
fastqc -t 40 -o ./ /lustre/share/class/BIO8402/lab/data/SRR5811639.fastq.gz
```

```python
注：①其他可选参数： （Note： optional parameters）
#SBATCH -n XX #占用节点数（number of nodes）
#SBATCH --exclusive #独占节点 （number of exclusive nodes）
#SBATCH –time=[dd-hh:mm:ss] #指定作业最多运行时间 （max run time）
#SBATCH –nodelist=[nodes] #指定在该节点运行作业（specify a node(s)）
#SBATCH --exclude=[nodes] #指定不在该节点运行作业 (specify not run on these nodes)
#SBATCH -o %J.out #输出结果文件 (output file name)
```

```python
★区分-n 和--ntasks-per-node (Note：-n and --ntasks-per-node is different)
②当任务不支持 MPI 时时，假如要指定 cpu 节点 8 个核，-n 和--ntasks-per- node 要一起使用才能保证 8 个核心都来自同一个节点上。
（If MPI is not required for a task, if you want to assign 8 coresto a cpu node, you
should use options -n and -ntasks-per-node simultaneously to ensure the 8 cores are
from the same node.）

#SBATCH -n 8
#SBATCH --ntasks-per-node=8
```

尤其是MPI这一点，要非常关注

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444006031-c1c94b39-8b3c-43cb-936c-8369f26724d6.png)因为有很多软件运行的时候是不支持MPI即跨节点运行的，

所以我们在运行的时候需要注意，

如果要保证运行某个程序时所有的核心也就是进程都是来自于同一个节点，也就是用同一台电脑计算运行的，

我们就需要确保我们每个节点设置的核心数和我们指定的任务总核心数是一致的，这样才能确保这些核心都是来自于同一个节点运行，减少不可控因素的影响。





3，常用命令：

这一部分详细可以参考我的上一篇博客：  
[https://blog.csdn.net/weixin_62528784/article/details/146122850?spm=1001.2014.3001.5501](https://blog.csdn.net/weixin_62528784/article/details/146122850?spm=1001.2014.3001.5501)  
(1) 提交任务(task submission)：sbatch blastn.slurm

(2) 杀掉任务(kill a task)：scancel jobid

(3) 查看任务(check the status of a task)：squeue

squeue –u “clswcc-wmz” 只展示个人目录下任务情况(only show tasks under an individual directory)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444313014-6a4f524e-505d-40a1-bd7b-3e40535b1bdc.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444318034-4e72fbea-49d2-4bae-bfe3-49f97cffb59e.png)

l PARTITION: **<font style="color:rgb(255,0,0);">节点类型</font>****<font style="color:rgb(255,0,0);">(queue</font>****<font style="color:rgb(255,0,0);"> </font>****<font style="color:rgb(255,0,0);">name)</font>****<font style="color:rgb(255,0,0);">  </font>****<font style="color:rgb(255,0,0);">cpu</font>**

l ST： 任务状态——**<font style="color:rgb(255,0,0);">R</font>****<font style="color:rgb(255,0,0);">：正在运行</font>**，PD：排队(task status---R: running, PD: pending)

l NODES: **<font style="color:rgb(255,0,0);">任务所用节点数目</font>****<font style="color:rgb(255,0,0);">(the</font>****<font style="color:rgb(255,0,0);"> </font>****<font style="color:rgb(255,0,0);">number</font>****<font style="color:rgb(255,0,0);"> </font>****<font style="color:rgb(255,0,0);">of</font>****<font style="color:rgb(255,0,0);"> </font>****<font style="color:rgb(255,0,0);">nodes</font>****<font style="color:rgb(255,0,0);"> </font>****<font style="color:rgb(255,0,0);">for</font>****<font style="color:rgb(255,0,0);"> </font>****<font style="color:rgb(255,0,0);">a</font>****<font style="color:rgb(255,0,0);"> </font>****<font style="color:rgb(255,0,0);">task)</font>****<font style="color:rgb(255,0,0);"> </font>****<font style="color:rgb(255,0,0);">1</font>**

l NODESLIST: 任务所在节点，可通过**ssh**** **登陆该节点，例：ssh node220

(the list of node to run the task. You can use ssh to login to this node. For example, ssh node220. )

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444331405-e6ce9d54-722e-434d-98dc-cc0773c3c273.png)

(4) 查看节点状态：sinfo (check the status of a node：sinfo)

sinfo –p cpu /gpu/fat: 查看某一类型节点状态（check the status of nodes in a queue）

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444361740-443b8ff6-1a9f-4e73-8431-63e610295ff6.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444364786-58093104-a787-4a1e-b065-ff28da38afac.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444368516-fbf9dcb7-feb4-4066-b141-779a3cd3dbbf.png)

STATE:  drain：节点有问题，alloc：正在全部正在使用，mix：部分可以用，idle：闲置，down：节点不可用

(5) 监控和再修改作业(monitor or revise a task)

scontrol show job JOB_ID 显示该作业的一些信息

scontrol -dd show job JOB_ID 显示该作业的作业脚本

(6) module avail: 显示超算已安装的软件(show the modules installed)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444407331-cc85eb70-c5d3-4b8b-ab90-170d7d1936a6.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444419119-e67e2d43-39d0-4599-a873-e4b7fc4b7708.png)

module purge: 清理现有环境

module load ：加载某个软件

module unload: 卸载某个软件

module list: 显示自己目录下已加载的软件

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741444441071-a2c3c705-b714-4419-9dc0-8030c3faeb31.png)



6、超算计时方法(the method to measure the computational time) 

这个是我们需要掌握并且理解的，考虑到超算的计费标准。

（1）CPU 计时方法(CPU time measuring method)

单个作业所消耗的机时，与其运行时长及使用的CPU 核心数有关，

计算公式为T= t * n

n 为作业使用的CPU 核心数，t 为作业的运行时长(单位：小时，作业排队时间不计入消耗的机时)，T 为该作业消耗的机时量(单位：核小时)。

例：一个作业在独占一个16 核的cpu 节点，运行一个小时则消耗机时为16 核小时。Pi2.0 单个cpu 节点上的核数可能有所增加。请注意查看具体情况。



（2）GPU 计时方法(GPU time measuring method)

运行在GPU 队列上的作业，机时使用是按照CPU 耗时折算而来的。一个GPU

节点上配置了96 个CPU 核,包括16 块GPU 加速卡。

GPU 机时的计费单位是“卡小时”(cardhours),卡小时与用于计算CPU 机时费的单位“核小时”(corehours) 换算关系为1 cardhour = 120 corehours（该换算关系比较老了，现在交大的换算关系应该不是这个了）

例：一个作业独占一个GPU 节点运行一个小时则消耗机时为16*120 核小时！



7. 读写压缩文件的小窍门（压缩状态下分析文件的方法）(tricks using compressed files)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741446795192-f2a06b02-07a6-4cb8-96ef-535858ab6581.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741446802084-757bf7fe-6111-46bb-84a6-322906a2bde8.png)



8，除了超算，生科院生信系的小超算：使用系教学集群时参考（任务递交系统为 **torque**），而非slurm

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741446924116-f8968331-e28b-43d7-8956-0442d1c93e78.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741446932227-7aff5529-d4d9-40c9-b76a-f22d2e30d6ed.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741446943694-000fcbe4-cf1a-4c60-93fe-6820d1b4427c.png)

（1）Environment Module

• 可用Environment Modules 切换多个版本的工具软件

• 查看可用软件及版本：module avail

• 加载工具：module load

• 卸载工具：module unload

• 查看当前加载工具：module list

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741446962326-2f64895d-8451-43ad-97b0-e3acdaf40987.png)

（2）作业调度系统常用命令

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741446996973-9fdc7345-15f9-4faa-ab66-1cdbd1ffb079.png)

**qstat**（查看作业状态，作业结束一段时间后不能查看）

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741447040222-d9ff6368-f1cd-469d-a686-28135e818cf9.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741447053903-2c4903f9-f9af-44db-a766-941a5c42637d.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741447068782-4425b44e-5e5e-4d62-8d8b-77a03030e410.png)

（3）作业调度系统脚本示例，脚本模板如下：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741447100849-4333eaff-e6f2-4e23-a9f1-a81669cc3bbc.png)

请在你自己熟悉的编辑器中按如上模板编辑如下脚本，保存为test.pbs

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741447123989-f600dff2-9cd9-4a89-a7d1-b718d216e69b.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741447137580-de2fdaa9-ffa5-4df2-8be4-dc679649f0fe.png)

另外一个例子：  
RNA 序列比对——以 blast 为例建库+比对

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741447156868-b08a81c0-d501-4efe-888c-1766281902cb.png)



9，对于我之前测试运行时间的脚本的更正：  
参考我的博客：  
[https://blog.csdn.net/weixin_62528784/article/details/146118720](https://blog.csdn.net/weixin_62528784/article/details/146118720)

（1）之前的写法我是slurm中写个循环，也就是在同一个脚本内依次执行 4/8/16/32/64 线程

```python
#!/bin/bash
#SBATCH --job-name=blast
#SBATCH --partition=cpu
#SBATCH -n 64                 # 申请最多 64 核
#SBATCH --ntasks-per-node=64
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --mail-type=end
#SBATCH --mail-user=zht161932@sjtu.edu.cn

module load blast-plus

# 声明一个线程列表
THREADS_LIST=(4 8 16 32 64)

for THREADS in "${THREADS_LIST[@]}"; do
  echo -e "Testing -num_threads=$THREADS starts at $(date).\n"
  start=$(date +%s)

  blastn -query /lustre/share/class/BIO8402/lab/data/SRR5029637.fasta \
    -db /lustre/share/class/BIO8402/lab/test/chr21 \
    -evalue 1e-5 \
    -outfmt 7 \
    -max_target_seqs 1 \
    -num_threads "$THREADS" \
    -out /lustre/home/acct-stu/stu542/hw/test_class3/blast_time_test/n${THREADS}t${THREADS}

  end=$(date +%s)
  time=$(( end - start ))
  echo -e "difftime for -num_threads=$THREADS is $time s\n"
done
```



（2）第2种方法就是提供shell脚本的$1参数：通过脚本参数 $1 指定线程数

将下方脚本另存为 blast_param.sh，提交作业时通过 sbatch blast_param.sh 8 等方式指定线程参数

```python
#!/bin/bash
#SBATCH --job-name=blast
#SBATCH --partition=cpu
#SBATCH -n 64
#SBATCH --ntasks-per-node=64
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --mail-type=end
#SBATCH --mail-user=zht161932@sjtu.edu.cn

module load blast-plus

if [ -z "$1" ]; then
  echo "Usage: sbatch $0 <threads>"
  exit 1
fi

THREADS=$1

start=$(date +%s)

blastn -query /lustre/share/class/BIO8402/lab/data/SRR5029637.fasta \
  -db /lustre/share/class/BIO8402/lab/test/chr21 \
  -evalue 1e-5 \
  -outfmt 7 \
  -max_target_seqs 1 \
  -num_threads "$THREADS" \
  -out /lustre/home/acct-stu/stu542/hw/test_class3/blast_time_test/n${THREADS}t${THREADS}

end=$(date +%s)
time=$(( end - start ))
echo "Blast job with $THREADS threads took $time s"
```

（3）在脚本中写一个循环，依次提交多个作业：  
此方法会自动为列表中的每个线程数提交一个独立作业：

```python
#!/bin/bash

THREADS_LIST=(4 8 16 32 64)

for THREADS in "${THREADS_LIST[@]}"; do

  sbatch <<EOF
#!/bin/bash
#SBATCH --job-name=blast
#SBATCH --partition=cpu
#SBATCH -n $THREADS                  # 申请 $THREADS 核（如有需要，可改为更大值）
#SBATCH --ntasks-per-node=$THREADS
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --mail-type=end
#SBATCH --mail-user=zht161932@sjtu.edu.cn

module load blast-plus

start=\$(date +%s)

blastn -query /lustre/share/class/BIO8402/lab/data/SRR5029637.fasta \
  -db /lustre/share/class/BIO8402/lab/test/chr21 \
  -evalue 1e-5 \
  -outfmt 7 \
  -max_target_seqs 1 \
  -num_threads $THREADS \
  -out /lustre/home/acct-stu/stu542/hw/test_class3/blast_time_test/n${THREADS}t${THREADS}

end=\$(date +%s)
time=\$(( end - start ))
echo "Blast job with $THREADS threads took \$time s"
EOF

done
```



——》

```python
一些draft
```

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741329852164-1198428f-9697-47ce-a35a-f34d89c4e090.png)

每个节点（机器）有40个核，每个核有4内存

内存需要20G的话，如果一个节点是4G内存，需要多核至少5核

单个节点最大40G，最高192G内存



时间也要省（时间复杂度+计算资源计算），空间也要省（压缩+压缩形式下读写）

压缩时间忽略不计：  
读写在硬盘上，cpu都计算超过硬盘













