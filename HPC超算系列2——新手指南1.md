一，平台简介：

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741429111332-1b740c76-511e-46cd-9f32-f2d1b45d52b9.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741429219248-48a6ec0f-f8a2-4a19-b6b6-22e9f9606271.png)

主要是官方手册指南、B站视频（培训视频、软件视频）

1，超算平台架构：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741429371628-4e0afb43-7c11-45e2-9a47-12f2e73cb773.png)

和普通的家用电脑的架构不同，

主要区别在于：层次化的结构

（1）超算是有很多张cpu卡、gpu卡，以及硬盘，然后通过高速网络联合起来；

（2）一般情况下，多个核心首先会组织成一个节点，然后多个节点联合起来就形成了一个集群



然后节点部分又分为3个部分：  
（1）功能节点：登入节点+可视化平台节点+数据传输节点，

都是一些特殊的节点，设计用来执行不同的任务；比如说登入节点，承载用户的登入，类似于前台；

可视化节点用于在可视化平台上的访问服务；

数据传输节点：用户数据传输的服务

——》专事专用，比如说禁止在登入节点上进行大型数据传输，当然一些小文件是可以的

（2）计算系统：一些节点，也就是计算资源，用于处理我们所提交的计算任务；

用户实际上是不直接面对计算节点的，但是我们的作业需要在计算节点上去运行；

我们一般是在登入节点上用一种特定的格式，提交我们的作业，然后这个命令/作业会被提交给slurm作业调度系统——》这个系统其实就相当于是一种排队机制，有好几种队列，就相当于是食堂的窗口，我们的作业来了就是排在队尾；当前面的任务执行完毕之后，我们的作业就会被投入到某一个空闲的计算节点当中去运行

（3）文件系统：简单理解来说就是硬盘，存储远程的数据

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741430181217-4563ff1e-608d-4918-9a17-30a6e87575dd.png)

但是这个硬盘不是铁板一块，也是分层的：  
最高层的是一个全闪存文件系统，如果我们的作业有一个高输入输出的高频率的需求，我们一般会将我们作业中用到的data copy到这个全闪存系统中，然后再去运行我们的作业；

当然，一般的文件传输需求就是我们的主文件系统；

最底层的归档文件系统：冷存储，就是长时间不用的数据放在这里



2，slurm作业调度系统

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741430453177-d54bdd23-b48d-4a38-a794-9149cdbbfaab.png)

在登入节点提交slurm脚本，然后由slurm作业调度系统分配作业到不同计算节点进行任务执行



3，集群登入：

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741430628230-11f07079-36fb-4336-9dea-780401221553.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431708857-3ce241bf-1c4b-435b-b523-90640b9f18e2.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431754987-48bf1441-741a-491c-8656-9fcc77c442f5.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431780284-a4d0a4b1-8c48-4568-a5e3-b66297a508bf.png)

这个其实就很基本了，学生信的基本功；

我们在主校区一般是登入pi2.0节点

登入集群我首推vscode，毕竟以后要做coding，也得多平台；其次是在没办法了再用mobaxterm

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741430928888-64f011cc-bf9a-47f7-a744-41b6fb9ec540.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741430945048-f65f7b3b-1970-4b9e-82b7-139678812bea.png)

集群中一般使用的都是zsh，当然bash都可以随时切换。

然后这里要提一嘴免密登入的问题了：  
可以参考我之前的博客：[https://blog.csdn.net/weixin_62528784/article/details/146028231?spm=1001.2014.3001.5501](https://blog.csdn.net/weixin_62528784/article/details/146028231?spm=1001.2014.3001.5501)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431244686-d9d23c0b-70f7-45e3-be09-bd7c6b36a59c.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431889010-6f56a882-470c-43bf-8339-d7a8628e9b8d.png)

比如说，这是一个我们的账户主界面：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431283942-f5614eff-b53c-4140-8e2b-7fd4bd0bab83.png)	

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431350878-26de7ca8-2934-4051-90f4-e24bdfd0c0e4.png)

申请免密证书需要验证邮箱，但是鉴于这是一个教学用临时账号，我们此处不进行免密登入。

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431482933-970811d7-2bf7-4272-913e-b84379966467.png)

总之传统的方式是不管用的



![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431960033-240e7101-6edd-40ba-abdb-2778457bcedb.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741431967621-5cd9140c-37be-4a84-b0fc-34438ec61fc8.png)



4，数据传输：  
和登入节点类似，数据传输节点也有专门的节点，如果需要大批量数据的传输，建议登入到数据传输节点再进行；

登入方式和ssh类似

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741432158141-0cdc61ba-e71d-446a-8bc2-ccc13797137a.png)

但是教学用账号是没有权限登入数据传输节点的，需要科研用正式实验室账号以及子账号，才有权限登入（ssh stu542@dataxxxx，亲测）

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741432563974-d4a6a5dc-9499-407f-8abb-1e5cd5781875.png)

小白的话当然使用图形化处理界面的SFTP：

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741432676976-c01b1d2c-adaf-4324-8c3c-c03b0e74c8bb.png)

其实可视化登入界面上也没有什么，就很正常的linux-desktop

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741432676976-c01b1d2c-adaf-4324-8c3c-c03b0e74c8bb.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741432849021-796a7c69-6ac3-47c0-b7fa-b60423f1d66e.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741432911664-14b20214-0691-4278-82e8-fb5adbbbaa8f.png)

进入之后是这样的：![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741432995479-8a4aba98-6b49-4a4c-8e78-3edc7f503ba6.png)



非小白的话，使用正常的文件传输命令操作即可：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741433107097-713fb1c4-4f21-4d59-ada7-7733594ada02.png)

也是常识，可以参考我之前博客：![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741433237383-735319b6-055b-4e20-892d-f574b956d5dd.png)  
[https://blog.csdn.net/weixin_62528784/article/details/146071769?spm=1001.2014.3001.5501](https://blog.csdn.net/weixin_62528784/article/details/146071769?spm=1001.2014.3001.5501)



5，作业提交与查询：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741433437917-e63abe61-31c5-45d3-b36a-8553c0ec28c4.png)

各种字母，后续在作业提交以及后台监控时候会经常遇见

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741433521544-59f58948-d708-4d73-ab97-286c4c6e3fa5.png)

sbatch就很基本了，最基本的任务投递命令

（1）sinfo：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741433625727-048346f0-6652-408e-b393-5c7ebafe91ab.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436248524-e5442cc3-ba55-45df-bc1b-369bcc00c4b1.png)

我们能够查看每一个队列的每一个状态的节点到底有多少个

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436297228-5aa65b1c-8854-4bd2-9135-7850b2d16129.png)

比如说上面，每一行都是一个队列的一个状态的节点情况，比如说cpu队列中idle空闲的节点数目有233个

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436356847-679ee43e-0e6c-489c-a61b-b22091a3b616.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436368328-43714b55-e81f-4df7-a16d-e70652895ca6.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436510315-ec991c0e-a1e7-40bc-8f7a-47d6298a6792.png)

然后比如说上面的gpu节点，就完全没有空闲的了，就全被占用了，说明深度学习炼丹跑模型的人很多。





**我们一般关注的是空闲也就是idle的节点，一般来说我们应该在队列任务投递之前先查看一下这个队列中节点的运行情况，如果说其中idle的节点比较多，那就意味着我们在任务投递之后很快就可以轮到我们的任务执行了。**



比如说我想查看一下无论哪个队列中什么节点是空闲的：

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741433736751-019bc133-9505-4b11-991d-91ab459b3d13.png)

可以看到cpu队列中很多节点都是空闲的

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741433780240-e96fe986-e62d-442c-8a8f-a5fa181d2106.png)

当前cpu队列中有124个节点都是空闲的

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741434085442-d1fa4b7c-a9ac-4e8b-8647-fa2cf524ba49.png)

当然可以实时查看

（2）squeue：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741434194776-b71dc866-bb66-475b-b0fa-d3a65339b406.png)

查看作业排队情况，

比如说下面的sleep的c程序，

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436638488-9eeaf369-4f14-426e-857d-f2968e30d72d.png)

我们就可以看到作业状态是在运行

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436662652-2118ea9d-3c32-47a1-a95f-a9a70b6d35c7.png)



![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436757244-572285f9-ff82-491b-b919-40c19343474a.png)

然后不一会就failed

（3）<font style="color:rgb(0,0,0);">scontrol：  
</font><font style="color:rgb(0,0,0);">查看作业提交详细情况；</font>

<font style="color:rgb(0,0,0);">比如说下面提交的一个hello world的c程序，</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741435572263-6370876b-4b35-4d8f-98fe-114db883e11b.png)

<font style="color:rgb(0,0,0);">（4）sacct：</font>

<font style="color:rgb(0,0,0);">查看作业记录，其实也是看不到细节的；</font>

<font style="color:rgb(0,0,0);">所以说sacct和squeue都是只能查看任务的排队情况，或者说是作业的提交情况，至于详细的细节，都需要使用scontrol命令中去查看；</font>

<font style="color:rgb(0,0,0);">如果不加命令的话，sacct只能查看当天中所有的运行记录；</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741435596062-bfefec57-9f06-48b5-9c91-492df90323ff.png)

<font style="color:rgb(0,0,0);">比如说上面运行的hello world的程序就是完成了的</font>

<font style="color:rgb(0,0,0);"></font>

<font style="color:rgb(0,0,0);">如果需要查看所有的运行记录的话，需要指定参数</font>

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741434453205-3c403d91-3627-40a6-992c-be371bce70b2.png)

比如说上面就是我从上周3开始执行的示例任务，有很多是完成了，部分是fail了，还有被取消的

<font style="color:rgb(0,0,0);">（5）scancel：  
</font><font style="color:rgb(0,0,0);">取消作业提交，尤其是在超算收费情况下，对于不需要或者说是fail的任务job，有必要及时cancel也就是kill</font>



![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741434678493-29c170aa-ff7b-4410-93bc-eb98eeb71bfc.png)

然后至于每个队列也就是带有不同资源的这些食堂窗口，提供不同的食品；

我们常用的是在pi2.0集群上的cpu队列，如果要用gpu，则是dgx2队列。



然后就是我们最关心的了：  
sluram脚本的细节

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741434792511-16f77803-a020-46e7-b5a7-507cc69af2ff.png)

其实就是前面“#”细节的差异，其余都和shell脚本没有区别

**1核就是最小1个cpu的核心、最小单位**。

**注意：“%j”就是作业号的意义！！！**

****

**示例：  
**![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741435207413-f54b1bf7-8c4e-4044-a67d-28de5ca1f079.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436145885-f7c13074-77aa-44a1-8dac-2a79249754fe.png)



![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436120150-1492911c-53d5-44fd-bd4f-1ac9f7d1ae11.png)





——》

当然，除了我们上面写出来的，需要在slurm中提交到后台计算节点的运行任务；

还有一种作业提交或者说是任务运行方式，需要我们注意

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741436993957-3f4dd972-d122-49bb-85f8-7fe8c438edbe.png)

这个其实也很常见，就是排队申请计算节点资源，申请到了，我们就相当于ssh进入了这个计算节点，然后一切操作就相当于是在前台操作了，就和个人主机或者说是小型服务器很像了。

需要即时编译，或者即时修改，即时debug时候操作。

可以参考我的上一篇博客：

[https://blog.csdn.net/weixin_62528784/article/details/146118720?spm=1001.2014.3001.5502](https://blog.csdn.net/weixin_62528784/article/details/146118720?spm=1001.2014.3001.5502)

在这一篇博客中，我就是使用了srun即时前台申请了一个计算节点，然后安装conda软件seqtk，做了一个简单的处理，然后稍微复杂一点的任务，我再将处理好之后的数据进行sbatch提交。



6，软件模块使用：

超算另外一个比较特殊的地方在于，那些非自己编译的软件、非本地软件，常规方法是无法获取的；

基本上需要你在执行某个任务的环境中去加载，module load，然后然后就相当于在你load的那个环境中将该程序添加到环境变量当中了，然后你就可以使用该软件了。  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741437728323-235bb3d2-15f9-4b2a-b6c2-c59bc830a0e3.png)

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741437926087-91ae2501-8987-46cf-9ca5-5faabebc39a4.png)

我们以加载g++（c语言的编译器）的演示来做示例：  
首先当前环境中无任何模块加载：  
![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741437991477-1cf31592-c570-47a1-8936-d752c632c604.png)



然后这是当前的gcc版本，也就是登入节点中默认的版本，v8.5.0

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741438032432-9d3b1cac-cb5d-4bb9-89aa-1b35e5d036c6.png)



我们可以先查看一下可用的gcc版本（注意是gcc/去搜索，否则会搜到很多也是gcc编译但不是我们想要的软件）

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741438133714-fa39fa86-b69f-41df-95ca-b61d59e8489e.png)

然后D就是默认版本，比如说我们module load gcc，然后没有指定版本的话，那么加载的就是这个默认版本；

但是如果我们想要加载特定版本的gcc，

我们就需要指定版本参数，比如说我们想要加载11.2.0这个版本

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741438297288-c098f1ea-9211-4df0-a4b6-f7d8a3cb432b.png)

可以发现这个模块被加载了

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741438329707-a7885d5e-36b9-4768-ae5e-83a7d8f9be4c.png)

然后版本也确实对应变了；

我们可以接着卸载这个模块，可以发现一切都变回来了：![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741438404424-8e1749d6-55af-43a8-ace5-49e283d4820e.png)



![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741438549927-6a075514-7585-4b7f-864c-363757d55a56.png)





二，slurm作业调度系统详解

Slurm（Simple Linux Utility for Resource Management）是一个常用的作业调度系统，已被全世界的国家超级计算机中心广泛采用，它可以管理计算集群上的资源，并调度和执行作业（例如脚本或分析任务），帮助用户高效地在多个计算节点上运行任务。

我们需要知道的是：**1台计算机就是1个节点**（1个cpu节点，对应着1个台电脑；1个gpu节点，1张gpu卡）

![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741439047958-73831658-ff37-4fee-8e84-55312e25cce2.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741439069431-884beab7-e3e7-4df5-949e-c4d7cda734b5.png)![](https://cdn.nlark.com/yuque/0/2025/png/33753661/1741439089742-31d1deab-c849-4bee-bafb-2c7e0dba69a2.png)

### **<font style="color:rgb(255, 255, 255);">Slurm 的基本概念</font>**
**作业 (Job)**: 用户提交给集群去执行的任务，例如运行一个脚本。

+ **<font style="color:rgb(0, 0, 0);">节点 (Node)</font>**<font style="color:rgb(0, 0, 0);">: 集群中的一台计算机，负责执行作业。</font>
    - **<font style="color:rgb(0, 0, 0);">登录节点 (Login Node)</font>**<font style="color:rgb(1, 1, 1);">: 用户通过 SSH 进入集群后最初登录的节点。它用于准备作业、编写脚本和提交作业，但不是作业执行的地方。</font>
    - **<font style="color:rgb(0, 0, 0);">分配节点 (Allocated Node)</font>**<font style="color:rgb(1, 1, 1);">: 当你提交作业并成功分配资源时，作业将在这些节点上运行，它们由 Slurm 根据你的资源需求来选择。</font>
+ **<font style="color:rgb(0, 0, 0);">分区 (Partition)</font>**<font style="color:rgb(0, 0, 0);">: 一组节点的集合（也可以叫做“队列”），不同分区可能有不同的资源配置或策略。</font>
+ **<font style="color:rgb(0, 0, 0);">调度器 (Scheduler)</font>**<font style="color:rgb(0, 0, 0);">: Slurm 根据资源可用性和作业的优先级，决定作业何时、在哪些节点上执行。</font>

### **<font style="color:rgb(255, 255, 255);">基本工作流程</font>**
+ **<font style="color:rgb(0, 0, 0);">提交作业</font>**<font style="color:rgb(0, 0, 0);">: 用户通过</font>`<font style="color:rgb(30, 107, 184);">sbatch</font>`<font style="color:rgb(0, 0, 0);">命令提交编写好的作业脚本，描述作业的资源需求（如节点数、CPU数、内存等）和执行命令，或者使用 </font>`<font style="color:rgb(30, 107, 184);">srun</font>`<font style="color:rgb(0, 0, 0);"> 运行交互式作业。</font>
+ **<font style="color:rgb(0, 0, 0);">资源分配：</font>**<font style="color:rgb(0, 0, 0);">Slurm的调度器根据当前的资源可用情况和作业队列中的优先级，分配资源给新提交的作业。</font>
+ **<font style="color:rgb(0, 0, 0);">监控作业</font>**<font style="color:rgb(0, 0, 0);">: 使用 </font>`<font style="color:rgb(30, 107, 184);">squeue</font>`<font style="color:rgb(0, 0, 0);"> 查看作业的状态（例如，正在运行或等待中）。</font>
+ **<font style="color:rgb(0, 0, 0);">作业输出</font>**<font style="color:rgb(0, 0, 0);">: 作业完成后，输出通常会保存在用户指定的文件中。</font>

![](https://cdn.nlark.com/yuque/0/2025/jpeg/33753661/1741438969591-b7773cc5-ed16-4008-b6ba-c24f9822b15a.jpeg)

### **<font style="color:rgb(255, 255, 255);">Slurm 概览</font>**
| Slurm | 功能 |
| --- | --- |
| sinfo | 集群状态 |
| srun | 启动交互式作业 |
| squeue | 排队作业状态，当前作业状态监控 |
| sbatch | 作业/脚本提交 |
| scontrol | 查看和修改作业参数 |
| sacct | 显示用户的作业历史，默认情况下，sacct显示过去 **24小时** 的账号作业信息。 |
| scancel | 删除作业 |
| sreport | 生成使用报告 |


### `**<font style="color:rgb(255, 255, 255);">sinfo</font>**`**<font style="color:rgb(255, 255, 255);"> 查看集群状态和信息</font>**
| Slurm | 功能 |
| --- | --- |
| sinfo -s | 简要格式输出 |
| sinfo -N | 查看节点级信息 |
| sinfo -N --states=idle | 查看可用节点信息 |
| sinfo --partition=cpu | 查看队列信息 |
| sinfo --help | 查看所有选项 |


**输出字段**:

+ **<font style="color:rgb(0, 0, 0);">PARTITION</font>**<font style="color:rgb(1, 1, 1);">: 分区名称</font>
+ **<font style="color:rgb(0, 0, 0);">AVAIL</font>**<font style="color:rgb(1, 1, 1);">: 节点可用性状态（up/down）</font>
+ **<font style="color:rgb(0, 0, 0);">TIMELIMIT</font>**<font style="color:rgb(1, 1, 1);">: 分区的时间限制</font>
+ **<font style="color:rgb(0, 0, 0);">NODES</font>**<font style="color:rgb(1, 1, 1);">: 分区中的节点数量</font>
+ **<font style="color:rgb(0, 0, 0);">STATE</font>**<font style="color:rgb(1, 1, 1);">: 节点状态：</font>`<font style="color:rgb(1, 1, 1);">drain</font>`<font style="color:rgb(1, 1, 1);">(节点故障)，</font>`<font style="color:rgb(1, 1, 1);">alloc</font>`<font style="color:rgb(1, 1, 1);">(节点在用)，</font>`<font style="color:rgb(1, 1, 1);">idle</font>`<font style="color:rgb(1, 1, 1);">(节点可用)，</font>`<font style="color:rgb(1, 1, 1);">down</font>`<font style="color:rgb(1, 1, 1);">(节点下线)，</font>`<font style="color:rgb(1, 1, 1);">mix</font>`<font style="color:rgb(1, 1, 1);">（节点被占用，但仍有剩余资源）</font>
+ **<font style="color:rgb(0, 0, 0);">NODELIST</font>**<font style="color:rgb(1, 1, 1);">: 节点名称列表</font>

查看总体资源信息：

```plain
sinfo
#PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
#cpu         up  7-00:00:0    656   idle cas[001-656]
#dgx2        up  7-00:00:0      8   idle vol[01-08]
```

查看某个特定节点的信息：

```plain
sinfo -n <节点名称>
```

### `**<font style="color:rgb(255, 255, 255);">sacct</font>**`**<font style="color:rgb(255, 255, 255);">显示用户作业历史</font>**
+ **<font style="color:rgb(0, 0, 0);">用途</font>**<font style="color:rgb(0, 0, 0);">: 查询作业历史记录，显示已完成和正在进行的作业信息，默认情况下，sacct显示过去 </font>**<font style="color:rgb(0, 0, 0);">24小时</font>**<font style="color:rgb(0, 0, 0);"> 的账号作业信息。</font>
+ **<font style="color:rgb(0, 0, 0);">参数</font>**<font style="color:rgb(0, 0, 0);">:</font>
    - `<font style="color:rgb(1, 1, 1);">-j <jobid></font>`<font style="color:rgb(1, 1, 1);"> 查询特定作业</font>
    - `<font style="color:rgb(1, 1, 1);">-S <YYYY-MM-DD></font>`<font style="color:rgb(1, 1, 1);"> 查询指定开始日期的作业</font>
    - `<font style="color:rgb(1, 1, 1);">-u <username></font>`<font style="color:rgb(1, 1, 1);"> 查询特定用户的作业</font>
+ **<font style="color:rgb(0, 0, 0);">输出字段</font>**<font style="color:rgb(0, 0, 0);">:</font>
    - **<font style="color:rgb(0, 0, 0);">JobID</font>**<font style="color:rgb(0, 0, 0);">: 作业ID</font>
    - **<font style="color:rgb(0, 0, 0);">JobName</font>**<font style="color:rgb(0, 0, 0);">: 作业名称</font>
    - **<font style="color:rgb(0, 0, 0);">Partition</font>**<font style="color:rgb(0, 0, 0);">: 分区名称</font>
    - **<font style="color:rgb(0, 0, 0);">Account</font>**<font style="color:rgb(0, 0, 0);">: 用户账户</font>
    - **<font style="color:rgb(0, 0, 0);">State</font>**<font style="color:rgb(0, 0, 0);">: 作业状态（COMPLETED、FAILED、CANCELLED等）</font>
    - **<font style="color:rgb(0, 0, 0);">Elapsed</font>**<font style="color:rgb(0, 0, 0);">: 作业运行时间</font>

### `**<font style="color:rgb(255, 255, 255);">squeue</font>**`**<font style="color:rgb(255, 255, 255);"> 查看作业信息</font>**
| Slurm | 功能 |
| --- | --- |
| squeue -j <作业的jobid> | 查看作业信息 |
| squeue -l | 查看细节信息 |
| squeue --nodelist=<节点名称> | 查看特定节点作业信息 |
| squeue | 查看USER_LIST的作业 |
| squeue --state=R | 查看特定状态的作业 |
| squeue --help | 查看所有的选项 |


**输出字段**:

+ **<font style="color:rgb(0, 0, 0);">JOBID</font>**<font style="color:rgb(1, 1, 1);">: 作业ID</font>
+ **<font style="color:rgb(0, 0, 0);">PARTITION</font>**<font style="color:rgb(1, 1, 1);">: 分区名称</font>
+ **<font style="color:rgb(0, 0, 0);">NAME</font>**<font style="color:rgb(1, 1, 1);">: 作业名称</font>
+ **<font style="color:rgb(0, 0, 0);">USER</font>**<font style="color:rgb(1, 1, 1);">: 用户名</font>
+ **<font style="color:rgb(0, 0, 0);">ST</font>**<font style="color:rgb(1, 1, 1);">: 作业状态包括</font>`<font style="color:rgb(1, 1, 1);">R</font>`<font style="color:rgb(1, 1, 1);">(正在运行)，</font>`<font style="color:rgb(1, 1, 1);">PD</font>`<font style="color:rgb(1, 1, 1);">(正在排队)，</font>`<font style="color:rgb(1, 1, 1);">CG</font>`<font style="color:rgb(1, 1, 1);">(即将完成)，</font>`<font style="color:rgb(1, 1, 1);">CD</font>`<font style="color:rgb(1, 1, 1);">(已完成)</font>
+ **<font style="color:rgb(0, 0, 0);">TIME</font>**<font style="color:rgb(1, 1, 1);">: 作业运行时间</font>
+ **<font style="color:rgb(0, 0, 0);">NODES</font>**<font style="color:rgb(1, 1, 1);">: 作业使用的节点数量</font>
+ **<font style="color:rgb(0, 0, 0);">NODELIST(REASON)</font>**<font style="color:rgb(1, 1, 1);">: 作业所在节点或排队原因</font>

默认情况下，`<font style="color:rgb(30, 107, 184);">squeue</font>`只会展示在排队或在运行的作业。

```plain
$ squeue
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
18046      dgx2   ZXLing     eenl  R    1:35:53      1 vol04
17796      dgx2   python    eexdl  R 3-00:22:04      1 vol02
```

显示您自己账户下的作业：

```plain
squeue -u <用户名>
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
17923      dgx2     bash    hpcwj  R 1-12:59:05      1 vol05
```

`<font style="color:rgb(30, 107, 184);">-l</font>`选项可以显示更细节的信息。

```plain
squeue -u <用户名> -l
JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
17923      dgx2     bash    hpcwj  RUNNING 1-13:00:53 30-00:00:00    1 vol05
```

### `**<font style="color:rgb(255, 255, 255);">srun</font>**`**<font style="color:rgb(255, 255, 255);"> 启动交互式作业</font>**
```plain
srun --partition=<分区> --nodes=<节点数> --ntasks=<任务数> --t=<时长> --mem=<内存> --c=<调用的CPU数量> --pty bash
```

运行此命令后，你会获得一个交互式的 shell，能够在该节点上直接执行命令。但是srun的缺点是一旦断线就无法重新连接回去，因此推荐使用Linux终端复用神器tmux/screen+srun配合运行

### `**<font style="color:rgb(255, 255, 255);">sbatch</font>**`**<font style="color:rgb(255, 255, 255);">作业提交</font>**
准备作业脚本然后通过`<font style="color:rgb(30, 107, 184);">sbatch</font>`提交是 Slurm 的最常见用法。为了将作业脚本提交给作业系统，Slurm 使用

```plain
sbatch my_job_script.sh
```

Slurm 具有丰富的参数集。以下最常用的。

| Slurm | 含义 |
| --- | --- |
| `-n [count]` | 总进程数 |
| `--ntasks-per-node=[count]` | 每台节点上的进程数 |
| `-p [partition]` | 作业队列 |
| `--job-name=[name]` | 作业名 |
| `--output=[file_name]` | 标准输出文件 |
| `--error=[file_name]` | 标准错误文件 |
| `--time=[dd-hh:mm:ss]` | 作业最大运行时长 |
| `--exclusive` | 独占节点 |
| `--mail-type=[type]` | 通知类型，可选 all, fail, end，分别对应全通知、故障通知、结束通知 |
| `--mail-user=[mail_address]` | 通知邮箱 |
| `--nodelist=[nodes]` | 偏好的作业节点 |
| `--exclude=[nodes]` | 避免的作业节点 |
| `--depend=[state:job_id]` | 作业依赖 |
| `--array=[array_spec]` | 序列作业 |


这是一个名为`<font style="color:rgb(30, 107, 184);">cpu.slurm</font>`的作业脚本，该脚本向cpu队列申请1个节点40核，并在作业完成时通知。在此作业中执行的命令是`<font style="color:rgb(30, 107, 184);">/bin/hostname</font>`。

+ `<font style="color:rgb(1, 1, 1);">#SBATCH --job-name</font>`<font style="color:rgb(1, 1, 1);"> 作业名称</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --output</font>`<font style="color:rgb(1, 1, 1);"> 标准输出文件：如/share/home/pengchen/work/%x_%A_%a.out</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --error</font>`<font style="color:rgb(1, 1, 1);"> ERROR输出文件：如/share/home/pengchen/work/%x_%A_%a.err</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --partition</font>`<font style="color:rgb(1, 1, 1);"> 工作分区，我们用cpu之类的</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --nodelist</font>`<font style="color:rgb(1, 1, 1);"> 可以制定在哪个节点运行任务</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --exclude</font>`<font style="color:rgb(1, 1, 1);"> 可以设置不放在某个节点跑任务</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --nodes</font>`<font style="color:rgb(1, 1, 1);"> 使用nodes数量</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --ntasks</font>`<font style="color:rgb(1, 1, 1);"> tasks数量，可能分配给不同node</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --ntasks-per-node</font>`<font style="color:rgb(1, 1, 1);"> 每个节点的tasks数量，由于我们只有1 node，所以ntasks和ntasks-per-node是相同的</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --cpus-per-task</font>`<font style="color:rgb(1, 1, 1);"> 每个task使用的core的数量（默认 1 core per task），同一个task会在同一个node</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --mem</font>`<font style="color:rgb(1, 1, 1);"> 这个作业要求的内存 (Specified in MB，GB)</font>
+ `<font style="color:rgb(1, 1, 1);">#SBATCH --mem-per-cpu</font>`<font style="color:rgb(1, 1, 1);"> 每个core要求的内存 (Specified in MB，GB)</font>

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

用以下方式提交作业：

```plain
sbatch cpu.slurm
```

`<font style="color:rgb(30, 107, 184);">squeue</font>`可用于检查作业状态。用户可以在作业执行期间通过SSH登录到计算节点。输出将实时更新到文件[jobid] .out和[jobid] .err。

这里展示一个更复杂的作业要求，其中将启动80个进程，每台主机40个进程。

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

以下作业请求4张GPU卡，其中1个CPU进程管理1张GPU卡。

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

以下作业启动一个3任务序列（从0到2），每个任务需要1个CPU内核。

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

在提交到SLURM的作业脚本中，可以激活Conda环境以确保作业在正确的软件环境中运行。以下是一个示例SLURM作业脚本：

```plain
#!/bin/bash
#SBATCH --job-name=myjob            # 作业名称
#SBATCH --output=myjob.out          # 标准输出和错误日志
#SBATCH --error=myjob.err           # 错误日志文件
#SBATCH --ntasks=1                  # 运行的任务数
#SBATCH --time=01:00:00             # 运行时间
#SBATCH --partition=compute         # 作业提交的分区

# 加载Conda
source ~/miniconda3/etc/profile.d/conda.sh

# 激活环境
conda activate myenv

# 运行命令
python my_script.py
```

### `**<font style="color:rgb(255, 255, 255);">scancel</font>**`**<font style="color:rgb(255, 255, 255);">取消指定作业</font>**
+ **<font style="color:rgb(0, 0, 0);">用途</font>**<font style="color:rgb(0, 0, 0);">: 取消一个或多个作业。</font>
+ **<font style="color:rgb(0, 0, 0);">示例</font>**<font style="color:rgb(0, 0, 0);">:</font>

```plain
scancel 12345
```

+ **<font style="color:rgb(0, 0, 0);">参数</font>**<font style="color:rgb(0, 0, 0);">:</font>
    - `<font style="color:rgb(30, 107, 184);">-u <username></font>`<font style="color:rgb(0, 0, 0);"> 取消特定用户的所有作业</font>
    - `<font style="color:rgb(30, 107, 184);">-p <partition></font>`<font style="color:rgb(0, 0, 0);"> 取消特定分区中的作业</font>

### **<font style="color:rgb(255, 255, 255);">Slurm环境变量</font>**
| Slurm | 功能 |
| --- | --- |
| $SLURM_JOB_ID | 作业ID |
| $SLURM_JOB_NAME | 作业名 |
| $SLURM_JOB_PARTITION | 队列的名称 |
| $SLURM_NTASKS | 进程总数 |
| $SLURM_NTASKS_PER_NODE | 每个节点请求的任务数 |
| $SLURM_JOB_NUM_NODES | 节点数 |
| $SLURM_JOB_NODELIST | 节点列表 |
| $SLURM_LOCALID | 作业中流程的节点本地任务ID |
| $SLURM_ARRAY_TASK_ID | 作业序列中的任务ID |
| $SLURM_SUBMIT_DIR | 工作目录 |
| $SLURM_SUBMIT_HOST | 提交作业的主机名 |




参考：

[https://docs.hpc.sjtu.edu.cn/index.html](https://docs.hpc.sjtu.edu.cn/index.html)  
[https://mp.weixin.qq.com/s/sTTCokxsYFaffdt2aZyoDw](https://mp.weixin.qq.com/s/sTTCokxsYFaffdt2aZyoDw)

[https://mp.weixin.qq.com/s/BpbRL958AugB_dcYfYaXEg](https://mp.weixin.qq.com/s/BpbRL958AugB_dcYfYaXEg)

