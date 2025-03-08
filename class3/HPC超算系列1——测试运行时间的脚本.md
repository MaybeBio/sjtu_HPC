超算具体使用说明，参考：[https://docs.hpc.sjtu.edu.cn/](https://docs.hpc.sjtu.edu.cn/)

问题背景：估计在超算上的程序运行时间   
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741360449881-f258d9fa-4e0c-4ea6-96df-891c74a3b81c.png)

我们在超算上运行的时候，对于节点、核心、内存等计算资源的把握或者说是掌控需要非常的深入，

以及对于每一个运行程序（底层运行原理）的时间复杂度以及空间复杂度，都需要一定程度的把控，

因为在计算机群上提交job、执行任务，与在个人主机，或者说是小型服务器上提交并运行作业是十分不一样的，

资源、空间以及算力的分配都需要开销，

一个流传很广的例子：  
某某计算课题组某研究生在周五在超算上提交了一个job，利用了大量的计算节点资源，自信的以为下周一就可以看到初步的分析结果了；

结果在课题组复盘代码的时候发现了算法逻辑错误，等到周末取消job的时候，超算所运行核时已经消费了上w元！

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741360665756-05e768ea-1619-4e7c-acf0-0634f603c090.png)

——》我们使用另外一个测试数据进行时间运行复杂度的演示：

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741410344527-4e3707dc-8b90-4a2f-84b8-e0826e7fb36e.png)

任务很简单：同样是为了明确程序提交时候如何选取节点核心数、线程数，才能最优化运行时间，达成时间开销成本最小化；

具体来说：

运行程序不变，但是控制变量法修改core数（-n选项，一般在提交slurm脚本时候设置参数）、线程数（一般是支持多线程运行软件本身携带的参数选项），

我们的一切前提建立在我们所运行的软件以及程序是不支持MPI，也就是多节点/跨节点运行的，

所以**我们的目标是为了在单节点运行下如何最大化运行效率以及最小化运行成本**  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741410515679-cd2ca47b-66a9-4873-bb2c-54265b70a735.png)

当然我们在实际测试的时候，也就是在提交大型脚本的时候，都会先做一个小规模测试、中规模测试，等到这两个测试都没问题了，我们才会进一步正式提交大规模运行脚本。

逻辑很简单：上线前预测试，否则终身被超算平台禁权限。

对于我们手头这一个简单的比对问题而言，小规模测试就是随机抽取reads数目，进行小规模的比对，事实上这也是一个优化运行时间的策略（拆分成多个chunk，然后并行运行，投递到单节点job上）；

我们的测试以随机抽取5w reads作为测试，使用python、awk或者是任何你偏好的linux软件都可以。





1，程序主力脚本（伪代码版）：

一个blastn程序，我们需要统计不同核心cpu不同线程下  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741360938964-f51eb724-49ce-424a-82e0-a45a86b5b269.png)

**<font style="color:rgb(255,0,0);">解读：</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741361044906-a5f58e50-4e9b-4df2-8006-58a25add74d5.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741361048739-8250222b-98c3-4f76-b9d1-71728cd4a466.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741361053602-3a05f6c3-14ef-478f-8863-06583713efba.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741361058032-255e5abe-1479-4c60-8ad4-71160aedf425.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741361062686-b2c597fc-cca9-408f-8818-45cd8c0bf8b2.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741361067732-abd1cbbd-346d-48c1-a14b-1fb676891bd6.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741361071187-6f15171d-3214-4d64-a89f-a36151ca0c99.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741361075038-6e7e8ceb-5b6d-4c9f-b2cf-a51d3b3f3bea.png)

**使用-task blastn 指定基本核酸序列比对，-query SRR5029637.fasta 指定待比对序列文件，-db arabidopsis_index 指定参考基因组数据库前缀，-evalue 1e-5 设置期望值阈值，-outfmt 7 输出为表格格式，-max_target_seqs 1 限定只保留最高匹配结果1条，-num_threads 2 使用 2 个线程加快比对，-out out_2.blastn 定义结果输出文件；**

****

2，随机抽样的下采样（subsample）：

如题干所述，我们需要在小样本上进行测试，所以我们不会使用所有的raw read直接运行脚本，而是使用其中的一小部分reads进行处理；

此处采用seqtk为例子：  
参考：[https://docs.hpc.sjtu.edu.cn/faq/index.html](https://docs.hpc.sjtu.edu.cn/faq/index.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741411258505-e553376b-3a08-4c75-be43-d44924a0213c.png)

部分常用软件见：[https://docs.hpc.sjtu.edu.cn/app/index.html](https://docs.hpc.sjtu.edu.cn/app/index.html)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741421887434-503aca98-9f3b-4edd-960f-8966ce422bf4.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741421967259-2e44b6b4-0671-40a3-a062-7fa47ebee582.png)

可以发现在超算上是没有这个工具的，所以我们需要手动安装。



在超算上下载软件，首选conda解决，其次如果是其他开源软件，在权限许可的前提下自行编译安装；

因为我们需要实时在前台界面中进行conda虚拟环境操作，所以此处我不建议提交slurm脚本去申请计算节点并后台操作，一切都要所见即所得。

而且提交slurm之后需要后台监控sequeue，才能知道该脚本被提交到了什么计算节点上，才能对应ssh进入该节点，才能使用该节点加载以及创建的环境。

参考：[https://docs.hpc.sjtu.edu.cn/app/compilers_and_languages/conda.html](https://docs.hpc.sjtu.edu.cn/app/compilers_and_languages/conda.html)



正常来说，我们如果有conda的话，也只需要module load，然后在slurm脚本中load+conda安装+执行命令即可；

但是甚至是conda我们正常的登入节点中也是没有的，所以要使用conda也得先申请计算节点资源进行load。



总之我们使用交互式作业

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422033398-de73c1b3-1247-4d89-922b-430fa457dd04.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422096656-77749db1-1322-43c4-a0f6-6200ef9af520.png)

此处因为资源限制原因，我们只需要申请cpu队列即可

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741411151281-0f52a00e-691e-47ea-b525-705ff33048a1.png)

然后申请cpu计算节点也是需要排队的，等到资源匹配的时候，我们就进入了拥有申请资源的该节点之中，可以看到我们已经启动了这个计算节点的命令行终端，即bash之中；

只有在申请了即时的计算节点资源之后，我们才可以加载conda软件，

然后我们再在常见channel中搜索seqtk软件

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422391198-f7ba1874-c67e-4081-846d-1d12c7c605ad.png)

但是亲测不能在下载软件的同时才安装环境，得先新建环境之后才能安装软件；

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422476472-8005fc31-b5c2-46b9-9b1c-0f1c79284458.png)

而且最好使用source![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422481779-3bbf8dca-6e91-4fe1-89b8-3037ecf75036.png)

然后安装seqtk软件，如果软件版本有问题，比如说有冲突的话，直接conda install，就和module load一样的原理，适合的版本

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422485910-9ea34f57-de49-4e3b-8ab2-2346b3246d83.png)



前面只是下载软件，我们接下来要做的才是正事：

seqtk的使用很简单，--help看几秒就能写命令行  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422594525-63e523bb-76cf-40b6-a19e-358d33c71a02.png)

然后就是该计算节点资源提交的job得取消，也就是退出对于该计算节点资源的占用；

因为分配给教学用的资源每个人仅限于提交1个job，如果是两个就是PD也就是等待，以及达到max limit了；

我目前对交互式计算节点资源的退换方式，是直接squeue然后scancel取交这个job，其实就是类似于kill PID进程id；

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422602256-38e61807-c054-4038-a382-e5672103c315.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422764746-fb4a4053-eec9-4ce5-bcb0-bead1146c668.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422769647-20a8afe2-16df-45ab-bfa9-fba913264a6b.png)



3，正式提交脚本：  
首先我们的原始脚本（也就是正常执行的某个规定参数的脚本）：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741422823069-a57a22d0-b58e-4151-a0aa-a03ebcae23d8.png)

```python
#!/bin/bash
#SBATCH --job-name=blast
#SBATCH --partition=cpu
#SBATCH -n 40
#SBATCH --ntasks-per-node=40
#SBATCH --output=%j.out
#SBATCH --error=%j.err
module load blast-plus
makeblastdb -in /lustre/share/class/BIO8402/lab/data/chr21.fa -dbtype nucl -out ./chr21
blastn -query /lustre/share/class/BIO8402/lab/data/SRR5029637.fasta -db ./chr21 -evalue 1e-5 -
outfmt 7 -max_target_seqs 1 -num_threads 40 -out ./result
```



然后就是我们需要进行时间复杂度的评估的：

```python
echo -e " -n2-threads2 starts at `date`.\n" >>n2.log start=$(date +%s)
blastn -task blastn -query query.fasta -db index -evalue 1e-5 -outfmt 7 – max_target_seqs 1 -num_threads 2 -out 2.blastn
end=$(date +%s) time=$(( $end - $start ))
echo -e "difftime is $time\n" >>n2.log
echo -e " -n2-threads2 ends at `date`.\n" >>n2.log
```



我们的目的是为了监测在申请不同核心数目，然后在使用不同线程的情况下，统一命令执行的运行时间差异，如果运行核时有很大差异，我们就得考虑超算的计费开销问题了。

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741423022706-ad44f630-24c2-4202-a45f-9b55098391b4.png)

此处以使用4核心的单节点，仅仅测试不同线程的运行时间差异为例子：

我的脚本如下

```python
#!/bin/bash
#SBATCH --job-name=blast_5w
#SBATCH --partition=cpu
#SBATCH -n 4
#SBATCH --ntasks-per-node=4
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --mail-type=end
#SBATCH --mail-user=zht161932@sjtu.edu.cn

module load blast-plus

threads_list=(4 8 16 32 64)

for threads in "${threads_list[@]}";do
  echo -e "testing -n4 -num_threads=$threads starts at $(date).\n"
  start=$(date +%s)
  blastn -query /lustre/home/acct-stu/stu542/hw/test_class3/blast_time_test/SRR5029637_sub5w.fasta  \
    -db /lustre/share/class/BIO8402/lab/test/chr21 \
    -evalue 1e-5 \
    -outfmt 7  \
    -max_target_seqs 1 \
    -num_threads "$threads" \
    -out /lustre/home/acct-stu/stu542/hw/test_class3/blast_time_test/5w_test/n4t${threads}.out
  end=$(date +%s)
  time=$(( end - start ))
  echo -e "difftime for -n4 -num_threads=$threads is $time s\n"
  echo -e "testing -n4 -num_threads=$threads ends at $(date).\n"
done
```

idea其实很简单，就是在slurm中提供shell中循环逻辑，测试；

当然可能程序放在一起测试可能会有影响，

如果是个人主机或者是小型服务器，我可能会写成一个提供shell参数脚本的形式，

然后分别执行5个脚本，分别提供4~64这5个不同的运行线程参数；当然因为教学用计算节点资源有限，所以我们不一定能够多任务并行，只能串行。

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741423235714-3bc1e470-7c0f-4c5c-96f1-678ae85bee57.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741423240796-f62454e5-3f90-4aec-8042-8b23a46b66c0.png)

我们的任务已经投递到了cpu队列的cas027节点，其实我们可以直接ssh进入，但鉴于任务很简单，5w条reads其实不到几分钟就可以执行完成，所以我们直接在后台监管即可。

测试结果输出如下：

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741423297607-dd73617d-f58c-4c1a-81a2-61e59d3aa8b6.png)

可以看到我们在64线程前的测试还是可以拿来参考提供的，运行时间其实区别很小，当然还原到原始raw reads数据上可能就会有很大差距了。原始数据是千万10M级别的reads，

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741423308615-ec66a817-b585-4a78-b438-18473c6e2b31.png)



现在我们运行一下raw reads的计算时间复杂度，计算脚本如下

```python
#!/bin/bash
#SBATCH --job-name=blast
#SBATCH --partition=cpu
#SBATCH -n 4
#SBATCH --ntasks-per-node=4
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --mail-type=end
#SBATCH --mail-user=zht161932@sjtu.edu.cn

module load blast-plus

threads_list=(4 8 16 32 64)

for threads in "${threads_list[@]}";do
  echo -e "testing -n4 -num_threads=$threads starts at $(date).\n"
  start=$(date +%s)
  blastn -query /lustre/share/class/BIO8402/lab/data/SRR5029637.fasta  \
    -db /lustre/share/class/BIO8402/lab/test/chr21 \
    -evalue 1e-5 \
    -outfmt 7  \
    -max_target_seqs 1 \
    -num_threads "$threads" \
    -out /lustre/home/acct-stu/stu542/hw/test_class3/blast_time_test/n4t${threads}.out
  end=$(date +%s)
  time=$(( end - start ))
  echo -e "difftime for -n4 -num_threads=$threads is $time s\n"
  echo -e "testing -n4 -num_threads=$threads ends at $(date).\n"
done
```

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741423579622-fd2edea4-d79f-404a-a14d-7a3c52cd1e01.png)

可以看到也就差了十多分钟，而且线程越多反而不是好事。

我们的测试结果仅仅作为一个参考提供，实际评测效果依据数据差距而定。

  


