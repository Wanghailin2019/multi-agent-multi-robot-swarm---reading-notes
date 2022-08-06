# 群机器人：一种正式的方法

*作者：Heiko Hamann*

目录

[TOC]

## 序

我思考集群机器人（Swarm Robotics）技术的主要动机是小型机器人的概率局部动作如何总结成群体显示的理性全局模式的问题。对于工程师来说，它有可能让梦想成真，因为复杂的问题可以通过设计简单的协作组件来解决。对于科学家来说，它有可能帮助解释关于意识、人类社会和复杂性出现的重大问题。
集群器人技术是一个成熟的领域，值得一本书完全专注于它。到目前为止，除了其他主题之外，还有许多有趣的书籍也涉及集群机器人技术，但还没有完全专门的书籍。作为一种资源，尤其是对于年轻的研究人员和学生来说，一本书似乎很重要。我正在努力填补这个空白。这本书讲述了设计具有最大可扩展性和鲁棒性的机器人系统的故事，这对于今天的方法论来说是一个挑战。因此，与我一起寻找更好、更新颖的方法来设计去中心化机器人系统。
像许多其他书籍一样，这本书有着相当悠久的历史。 2013 年，我开始在德国帕德博恩大学教授一门非常特别的计算机科学家硕士课程，名为“Swarm Robotics”。当我创建这门课程时，没有一本完整地教授集群机器人技术的书特别令人头疼。当然，有些书籍包含与集群机器人相关的材料，例如 Bonabeau 等人关于群体智能的书籍[^48] 和 Kennedy 和 Eberhart [^210] 以及 Floreano 和 Mattiussi [^122] 撰写的关于仿生人工智能的伟大著作。但是，它们没有定义完整的课程。因此，在从头开始创建课程时，我必须经历任何老师必须做的事情：定义每个学生需要了解的最相关学科的标准，并创建所有教学材料。可能那时我已经想到写一本书可能有助于该领域并且是一个合理的选择。当学生们拼命要求随附的阅读材料时，这一点无疑得到了证实，我不得不不断地告诉他们：“是的，那会很棒，但还没有人写过那本关于集群机器人的书。”所以我在 2013 年慢慢开始了漫长的写作过程，在接下来的几年里，学生们一直在向我要那本书。在2013 年和 2016 年的几年间，我教了四次该课程，并且改进了授课材料。在 2017 年的主要写作期间，我转到了德国吕贝克的另一个教授职位；与此同时，我很高兴我经常可以写下我在教授这门课时已经想到的东西。然而，为了更完整地了解群机器人技术，有必要添加更多显然超出课程内容的材料。从本书的参考文献中可以看出，我努力不遗漏相关论文。然而，仍然有可能，或者说很可能，我忽略了与集群器人技术非常相关的某些东西或某些人。在这种情况下，请接受我的道歉，并请让我知道。
我希望这本书至少能帮助一些讲师，如果他们教授一门完全专注于集群器人的课程，或者一门部分包含集群机器人材料的课程。你不应该经历我在无中生有地准备课程时所经历的痛苦。希望学生也能发现它有用、易于理解，甚至可能有点有趣。想要研究该领域并研究集群机器人技术的年轻研究人员也希望在这里找到一些有用的信息作为起点。至少忽略一篇重要论文的问题可能不太可能发生。最后，我也希望有热情的集群机器人爱好者，他们敢于研究这样一本相当具有科学性的书，其中包含所有难以理解的术语。再一次，我努力让每个人或至少对那些对计算机科学或（计算）生物学有一点了解的人都可以阅读这篇文章。
当然，这本书之所以成为可能，是因为有很多人在这本书上直接帮助我，或者通过在过去十年中与我讨论集群机器人技术或通过让我了解相关的新旧论文来间接帮助我。我要感谢 Marco Dorigo、Thomas Schmickl、Payam Zahadat、Gabriele Valentini、Yara Khaluf、Karl Crailsheim、Ronald Thenius、Ralf Mayet、Jürgen Stradner、Sebastian von Mammen、Michael Allwright、Mostafa Wahby、Mohammad Divband Soorati、Tanja Kaiser、Eliseo Ferrante , Nicolas Bredeche, Sanaz Mostaghim, Jon Timmis 和 Kasper Støy。此外，我要感谢 2013 年至 2016 年间在帕德博恩参加群体机器人课程的学生提出了这么多有趣的问题，分享了他们对集群机器人问题的解决方案，他们的热情激励了我，并以怀疑的态度挑战我问题。
我衷心感谢我在 Springer Science+Business Media 的联系人的耐心，并在正确的时刻推动我：Mary E. James、Rebecca R.
Hytowitz、Murugesan Tamilsevan 和 Brian Halm。
也非常感谢授予文中图片的权限：Thomas Schmickl、Francesco Mondada、Farshad Arvin、José Hallloy、Ralf Mayet、Katie Swanson 和 Martin Ladstätter。

**德国吕贝克  Heiko Hamann**



## 第一章 群机器人介绍

*But it was one thing to release a population of virtual agents inside a computer's memory to solve a problem. It was another thing to set real agents free in the real world.*

——Michael Crichton, Prey 

*The flying swarm is immediately sent into the 'cloud-brain' formation and its collective memory reawakens.*

——Stanislaw Lem, The Invincible

*In fact, the colony is the real organism, not the individual.*

—— Daniel Suarez, Kill Decision

### **简要介绍**

我们介绍了群体机器人的基本概念并进行了一些概述。

群体机器人技术是一种复杂的方法，需要了解如何定义群体行为、是否存在最小群体规模、群体系统的要求和特性。我们定义自组织并发展对反馈系统的理解。群体不一定需要是同质的，可以由不同类型的机器人组成，使它们成为异质的。我们还讨论了机器人群与人类的交互作为一个因素。

群体机器人技术是研究如何使机器人协作并集体解决一项任务，否则，这些机器人中的单个个体是不可能解决的。这种真正的协作是群体机器人的魅力，是团队合作的理想。每个群体成员都平等地做出贡献，每个人都有相同的更高层次的目标。不过，公众对群体机器人的看法似乎过分强调了不可思议的方面。也许你读过 Stanisław Lem 的 The Invincible、Michael Crichton 的 Prey、Frank Schätzing 的 The Swarm 或 Daniel Suarez 的 Kill Decision。也许您想知道为什么这些机器人群或自然群中的大多数都是邪恶的或至少具有威胁性。不幸的是，这些作家将我们蜂拥而至的未来想象成一个反乌托邦（即可怕的反乌托邦）。一个不应不讨论的方面，但我们将在本章结束讨论未来技术时将其留到后面。也许您还想知道 Lem、Crichton、Schätzing 或 Suarez 的假设是否真的是现实的。主要问题是如何实现机器人集群，这就是我们将在本书中研究的内容。

虽然集群机器人及其协作机器人的直接应用为您提供了许多为您服务的机器人，但它也有助于为未来的工程开发可能更重要的方法。今天的技术已经达到了很高的水平。人类创造的最复杂的系统包括欧洲核子研究中心的大型强子对撞机和美国宇航局的航天飞机。其他已经设计好的复杂系统是 CPU（每个芯片的晶体管随时间呈指数增长）和最先进的（单片）机器人 [^35] [^244]。这些系统的共同点是一个人几乎不可能理解它们，因为它们包含许多相互作用的组件，其中许多组件本身很复杂。为了在未来维持对工程产品复杂性的控制，当我们的方法需要进行范式转变时，我们可能会达到一定程度的复杂性[^130]。有没有一种方法可以生成和组织复杂系统，而不依赖于征服大量异构的组件和子组件？是否有可能基于简单的组件生成复杂的系统？幸运的是，答案是肯定的，我们在自然界中找到了此类系统的示例。例如群体、羊群和牛群（见图 1.1）。成千上万的椋鸟自组织成群并表现出集体运动。鱼群在圆圈中移动，形成大而混乱的集群，这使得捕食者难以感知个体鱼类。蝗虫成群结队地穿过沙漠，蚂蚁表现出我们大多数人都知道的行为的多样性。所有这些系统都由大量类似的单元组成。这些单元中的每一个都相当简单，相对于它们形成的整个系统的复杂性而言，它们的功能相当少。这些自然群体现了复杂系统的非凡概念，该系统由基于简单规则交互的简单组件组成。从工程的角度来看，知道如何设计这样的系统会很有吸引力，而从科学的角度来看，知道这些系统是如何运作的也会很有吸引力。这两个角度是贯穿本书的两条主线，幸运的是它们直接相互关联。如果你能建造一些东西，那么你就真正理解了它；如果你理解了它，那么你就可以构建它。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="pictures\figure1-1.png" width="65%" alt="">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图1-1</div>
</center>

回到本章开头 Crichton 和 Lem 的引文，不是在模拟代理中而是在具体代理（即机器人）中实现群体行为将是一个额外的挑战。区分某个观察到的行为是集体效应（例如集体记忆）还是机器人本身的个人反应也将是一个挑战。

除了工程挑战之外，还有一个更深层次的挑战是将每个群体必须拥有的两种观点结合起来。有些特征只存在于群体层面，有些特征只存在于个体群体成员的层面。作为观察者，我们可以看到群体的整体形状及其动态，并且我们通常可以理解每个运动的目的或这些群体构建的架构。对于单个机器人、蚂蚁或鱼来说，这些特征中的大部分都是隐藏的，但大多数时候它会选择正确的动作。**了解简单行为的总和如何在群体层面产生定性的新特征，而这些特征可能无法通过其部分来理解，这是群体智能和群体机器人技术中最有趣的问题**。解决这个谜题将产生深远的影响，因为它肯定与神经科学、社会学和物理学有关。

### 1.1 集群机器人的初步方法

#### 1.1.1 什么是集群/群体？

有趣的是，文献中似乎没有明确的集群/群体（swarms）定义。例如，来自群体智能领域的共同贡献敢于在这个问题上保持空白 [^48][^210]。相反，人们找到了蜂群行为的定义。在生物学中，蜂群行为的一个常见定义是“聚集通常与集体运动相结合”（aggregation often combined with collective motion），这与我们上面的例子基本一致。群体作为一个概念的重要性也通过语言本身来传达。例如，在英语中有一些群体行为的词，例如鸟类的聚集（flocking），鱼类的浅滩（shoaling）或群落（schooling），以及四足动物的放牧（herding）。因此，我们有一个群体的定义，并且现在得出结论，**一个群体是通过它的行为来定义的**。

#### 1.1.2 集群/群体有多大？

群体机器人技术中一个经常引起会心一笑的问题是关于群体大小的问题。似乎很清楚的是，它必须有很多。但具体有多少？幸运的是，在这种情况下，Beni [^37] 在文献中找到了一个定义，尽管他通过它不是来定义群体大小：“不像处理统计平均值那么大”和“不像处理统计平均值那么小的few-body问题。”因此，对于 Beni [^37] 之后的适当群体大小 N，我们得到 $10^2 < N \ll 10^{23} $，这应该以群体不应是“Avogadro-large”的方式来解释。Avogadro罗常数为 $N_A \approx 6.02 ×10^{23}l/mol$，而“mol”是指含有与 12 g 纯碳-12 一样多的基本实体的物质的量。乍一看，这个上限的大小可能看起来高得不合理，但它很有用，因为它具有定性意义。 Avogadro 大型系统（例如气体、液体或固体）通常进行统计处理。

下界也被定义为与方法相关，不作为“few-body”（少体）问题处理。如果我们从小组规模开始研究few-body问题的复杂性，那么我们可以只从一个小组开始。只有一个人的系统肯定会缺少所有有趣的功能，例如沟通和协作。所以我们应该从二开始，这是可能的最小的社会群体，也称为二元。从一个人到两个人的转变从根本上改变了系统[^38]。Few-body问题的一个例子是经典力学中的二体(two-body)问题。问题是根据牛顿力学确定两个相互作用的物体的运动，例如行星及其卫星。值得注意的是，三体问题已经显示出质的差异（见图 1.2），因为它被证明是一个困难的数学问题 [^30]。因此，有人可能会争辩说，三体问题不属于简单的少体问题。因此，选择的下限 $10^2$ 可能已经太高了，而下限 3 也可能太极端了。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="pictures\figure1-2.png" width="65%" alt="">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图1-2</div>
</center>

*图 1.2 三体问题的轨迹示例（牛顿力学、两个静止的重质量和一颗运动中的卫星）作为“少体”问题复杂性的示例*

有趣的是，这个问题与 sorites 悖论（来自 soros，希腊语sorites为堆）有关。这是一个堆必须包含多少粒才能成为一个堆的问题。听起来很熟悉？虽然这个问题听起来很傻，但实际上它比人们想象的要深得多。人们很想用一个阈值（例如，10,000 个颗粒）来解决问题，然后才注意到否认 9,999 个颗粒是一个堆是不公平的。最后，Russell [^335] 等著名玩家讨论了自然语言中的模糊性问题。

摆脱这种困境的方法是防止设置阈值并保持与方法相关的定义。**群体不一定由其大小定义，而是由其行为定义。**如果它实现了一个群体行为，那么它应该被认为是一个群体，一个由 $N=3$ 个群体成员组成的群体可能会表现出可验证的群体行为。下面讨论群体行为的属性和要求。

#### 1.1.3 什么是集群机器人？

集群机器人的第一个实现可以追溯到 Maja J. Mataric 和她的“Nerd Herd”，参见 Mataric [^253]  [^254]  [^255]。遵循 Dorigo 和 Sahin [^99] 的定义是：“群体机器人学是研究如何设计大量相对简单的物理实体**代理（agents）**，以便从代理之间以及代理与环境之间的局部交互中出现所需的集体行为（collective behavior）”。

---

*“代理”的概念定义明确且复杂，参见 Russell 和 Norvig [^337] 和 2.4.2节.*

*“集体行为”是社会学中常见的技术术语，在群体机器人研究中使用得相当巧妙。用简单的意思“整个群体的行为”来阅读它似乎就足够了，即所有群体成员的整体行为。*

---

此外，术语“相对简单的”代理可以被指定为相对于指定的任务而言相对无能力或效率低下的机器人。代理和环境之间的本地交互应该是可能的这一事实要求机器人具有**本地感知能力**，并且可能还具有**通信能力**。事实上，（本地）通信通常被认为是群体的一个关键特征[^54]。通信也是允许群体成员之间协作和合作的必要要求。反过来，协作需要超越群系统中的单纯并行化。例如，在最简单的并行化形式中，每个处理器接收一个工作包，然后处理该工作包，而无需处理器之间的任何其他所需通信，直到该工作包完成。尽管这种简单形式的并行化是机器人组的一种选择（想象一个清洁任务，每个机器人单独清洁一个小的分配区域），但在群体机器人技术中，我们希望明确超越该概念并通过机器人之间的协作产生额外的性能提升。

Beni [^37] 将智能群（intelligent swarm）定义为“**一组非智能机器人，作为一个组形成一个智能机器人。换言之，一组‘机器’能够‘不可预知’地形成‘有序’的材料图案。**”然而，这个定义需要许多其他涉及的概念，例如智能、秩序和不可预测性，因此这里不再赘述。相反，我们注意到他对机器人群属性的定义：分散控制、缺乏同步性、简单（准）相同的成员或大规模生产的准同质成员[^37]。分散控制（Decentralized Control）与集中控制相反，集中控制是由中央单元控制远程单元。因此，**分散控制是一种控制范式，它禁止其他单元对单元的控制，这意味着每个单元都可以控制自己**。缺乏同步性意味着集群是异步的，即缺少每个集群成员都可以访问的全局时钟。反过来，如果需要，集群必须显式同步。

Sharkey [^359] 讨论了不同类型的群体机器人。她认为，群体机器人的早期方法是受自然启发的“极简主义方法”，主要限制了硬件的功能，这似乎不再是社区的共识。因此，她确定了可扩展的群体机器人技术，它不是极简主义的，也不是直接受自然启发的，实用的极简群体机器人技术不是直接受自然启发的，以及受自然启发的极简群体机器人技术，如果你愿意的话，它是原始的。



#### 1.1.4 为什么是集群机器人？

当讨论群体机器人系统的好处时，通常会提到三个主要优点[^99]。第一个优点是这些系统是鲁棒的。鲁棒性被定义为容错和故障安全，它是通过大量冗余和避免单点故障来实现的。群体是准同质的，因此每个机器人都可以被任何其他机器人替代。请注意，冗余还需要一定的最小群大小。上面我们已经提到，根据方法，可以将一组大小为 3 的组视为一个群体。这仍然是一个公平的评估，但同样清楚的是，一个大小为 3 的蜂群所达到的故障安全水平与大小为 100 的蜂群不同。

群体的控制是分散的，因此不存在单点故障。每个机器人只与直接邻居交互，它也只存储本地获得的数据（即本地信息）。因此，由于任何类型的故障而失去机器人只会产生容易克服的局部影响。在适当组织的机器人群中，即使是一定比例的机器人也可能会丢失，而不会对其有效性产生很大影响。效率可能会下降，但给定的任务仍然完全解决。

我们举一个例子来更详细地理解鲁棒性。群机器人网络可以用图论来解释。假设对于给定的任务，群在一个大的连接组件中保持连接是必不可少的。为了保证鲁棒性而要避免的一种情况是，去除一条边会导致图分成两个分量。在给定的去中心化环境中，明确确定某个链接是否至关重要是非常重要且困难的。一个简单的解决方案是保证某个最小的机器人密度（即每个区域的机器人数量），这意味着每个机器人的某个平均程度（即通信范围内的相邻机器人的数量）。这只是一个概率保证，然而，这对于群体机器人系统来说是典型的。可以在图中考虑鲁棒性。在图 1.3 中，节点 A 和边 b 代表单点故障。如果 A 或 b 失败，则图将被分成几个连接的组件（参见图论中的顶点分隔符）。该图可以表示由群机器人组成的通信网络及其通信范围。最初，每个机器人可以通过多跳通信显式地或通过网络传播信息（例如，本地广播序列）间接地与其他机器人联系。一旦机器人 A 发生故障或无法再沿边缘 b 进行通信，机器人群就会崩溃成两个无法再相互通信的子群。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="pictures\figure1-3.png" width="30%" alt="">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图1-3</div>
</center>

*图 1.3 在这个图中，节点 A 和边 b 代表单点故障，因为如果它们失败，图就会被分成几个组件*

以下是冗余鲁棒性的一个很好的例子。在当今的太空飞行中，通常会发射构成中兴部件的单个太空探测器（见图 1.4）。每当此探测器发生致命故障时，整个任务都会失败。因此，不遗余力地完善这些探针并以任何可能的方式对其进行测试。另一种方法是生产大量小型且可能很简单的探针，在理想情况下以相同数量的资金进行。每个单独的群体探测器的有限能力（例如，无线电信号强度、相机的分辨率）通过合作得到补偿。丢失一定比例的集群探针不会危及整个项目，因为整体仍然有效。一个健壮的系统是一个可靠的系统，一个可靠的系统可以通过冗余[^29]基于不可靠的部分创建。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="pictures\figure1-4.png" width="75%" alt="">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图1-4</div>
</center>

*图 1.4 冗余的鲁棒性：可以发射一组探测器或卫星，而不是发射单个太空探测器（左）（右，EDSN 项目——小卫星网络的爱迪生演示）（NASA 提供的照片）*

第二个优势是灵活性。同样由于准同质性，在硬件方面没有专业化。每个机器人都能够完成任何其他机器人的任务。 集群能够适应广泛的任务，因为通常不需要硬件方面的专业化。机器人通过合作克服了能力的限制。例如，太弱的执行器通过物体的集体运输来补偿，太低的信号强度通过形成机器人的多跳通信线路来补偿，太小的尺寸通过集体穿越孔或其他困难障碍物的自组装机器人来补偿.

第三个优势是可扩展性。所应用的方法（例如机器人的控制算法）可以扩展到任何规模的群体。它“可以在增加其尺寸的同时保持其功能，而无需重新定义其部件的交互方式”[^96]。这是因为每个机器人只与它的本地邻居交互。禁止不扩展的方法，例如向整个群体广播或依赖于调查大部分群体的过程。对于可扩展性，可能需要保证恒定的机器人密度，但是当群操作的区域相应扩大时，可以自由选择总体群大小。

经典计算机系统中可扩展性限制的一个例子是 Internet 服务。越来越多的服务器接听越来越多的用户。困难在于通常存在一些瓶颈。例如，传入的服务请求可能需要在中央机器上注册。因此，响应时间不会随着用户数量的增加而保持不变，而且它们也不会呈线性而是呈指数增长。

#### 1.1.5 什么不是集群机器人？

有时，多智能体或多机器人系统很难在概念上与群体机器人系统区分开来。然而，多智能体系统通常依赖于全局信息的分发或访问、广播、需要可靠的机器人-机器人通信的复杂通信协议（例如，谈判）、需要机器人识别单个机器人或知道群体的总大小 [^117][^298]。这些系统通常也无法扩展，因为**应用了蓝牙或 WLAN（无线局域网，又名 WiFi）等不可扩展的技术**。



### 1.2 早期的调查和研究

接下来，我们讨论群体机器人系统的三个基本特征。我们从对群体性能的详细讨论开始，即群体的表现取决于群体大小和群体密度。由于我们希望具有良好的可扩展性，因此研究群体性能很重要。作为第二个主题，我们讨论通信，因为群体有时会以意想不到的方式通信。作为第三个主题，我们指定了群体水平（swarm level）和个体群体成员水平（level of the individual swarm member）的概念。

#### 1.2.1 群体性能

如果集群活动的区域保持不变，机器人集群的平均性能取决于集群密度或集群规模。这很容易通过传递木桶（bucket brigade）的例子来形象化。传递木桶是一种多级分区传输方案，可以看作是任务分区（task partitioning）的一种形式[^8]。一旦蜂群可以覆盖源和汇之间的大部分距离，形成一个传递木桶是有用的。传递木桶也出现在许多蚂蚁物种中[^8]。当只有一个机器人时，它必须在一端捡水并通过驱动将其运送到另一端。同样，对于数量较少的机器人，它们必须来回移动，而两个机器人产生的水流是单个机器人的两倍，四个机器人的水流是两个机器人的两倍，依此类推（见图 1.5a）。
比如说，一旦有六个机器人，它们就可以组成一个水桶大队，这意味着它们只能固定在自己的位置上，只需将水交给大队中的下一个来运送水。通过这种简单的合作形式，这六个机器人能够运输的水量是三个机器人的两倍多（见图 1.5b）。这构成了超线性的性能提升，如图 1.5c 中的大幅跃升所示。超线性绩效增长意味着不仅整体绩效随着团队规模的增加而增加，而且个人绩效也随着团队规模的增加而增加。想象一下，你正在一群学生中学习，通过增加小组规模，你们每个人都会变得更有效率。这是一个伟大且非常理想的效果，但不幸的是在团队中很少观察到。例如，在黄蜂 [^198] 中观察到随着群体大小的增加个体性能的提高，并且在我们稍后将看到的群体机器人中也可以观察到（第 4 章）。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="pictures\figure1-5.png" width="65%" alt="">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图1-5</div>
</center>

当越来越多的机器人添加到系统中时，机器人密度会继续增加，并且对于一定的机器人密度，可以达到最佳的群体性能。当机器人密度进一步增加时，集群性能实际上会降低，因为机器人会相互干扰并减慢自身速度。当机器人密度进一步增加时，当几乎所有运动由于干扰而停止时，性能继续下降可能接近于零[^236]。即使是蚂蚁有时也必须应对物理干扰，尽管它们具有出色的爬行和攀爬能力。在拥挤的条件下，它们通过负反馈效应避免交通拥堵[^107]。

除了物理干扰之外，由于高群密度，其他影响也会增加开销。人类群体表现的一个例子是所谓的林格尔曼效应(Ringelmann effect)，它描述了随着群体规模的增加，在群体中工作的个人失去动力。 Ingham 等人报道了随着群体规模的增加，个人表现的非线性下降。 [^193]（另见 [^210]p236）。

我们将机器人群的性能定性地划分为四个区域：**亚线性增加**(sub-linear increase)、**超线性增加**(superlinear increase)、**最佳群体密度**(optimal swarm density)（也可以称为操作点）和**由于干扰而减少**(decrease due to interference)（见图 1.5d）。图 1.5d 所示的函数形状出现在许多不同的群体系统中 [^34], [^164], [^171], [^182], [^216], [^227], [^236]，因此将其定义为群体性能的一般方案似乎是合理的 [^166] . Østergaard 等人也报道了这种一般形状的存在，他们 [^299] 在多机器人系统的背景下写道：

*我们知道，通过改变给定任务的实现，我们可以移动“最大性能”的点，我们可以改变曲线两侧的形状，但我们不能改变图的一般形状。*

我们定义群体性能 $\Pi$ 的一般形状˘取决于群体密度$\rho$（每个区域的机器人）、固定区域 A 和群体大小$N = \rho A$：
$$
\Pi (N) = \frac{N}{e^N},\tag{1.1}
$$
大约是 $0.37N^{1-N}$。有了所有相关的常数，我们得到：
$$
\begin{gather*}
\Pi(N) = C(N)(I(N)-d) \tag{1.2}\\
\ \ \ \  =a_1N^ba_2exp(cN) \tag{1.3}
\end{gather*}
$$
对于参数$c<0,a_1,a_2>0,b>0,d \ge 0$[^166]。参数$d$用于设置限制$\Pi(N \rightarrow \infin) = I(N \rightarrow \infin)-d=0$。此外，我们通过引入两个分量 $C$ 和 $I$ 来完善我们对$\Pi(N)$的定义。我们定义了一个理论上的群体effort，可以通过合作函数在没有任何负反馈的情况下达到：
$$
C(N) = a_1N^b.\tag{1.4}
$$
Breder [^55] 在鱼群内的凝聚力以及 Bjerknes 和 Winfield [^42] 使用相同的公式来模拟紧急出租车中的群体速度。他们使用了 $b<1$ 的参数，而我们也允许 $b>1$。这可能是一个主要区别，因为 $b<1$ 表示由于合作而导致的亚线性性能提高，而 $b>1$ 表示超线性提高。我们定义干扰函数为：
$$
I(N) = a_2exp(cN)+d.\tag{1.5}
$$
这可以解释为无需任何合作（即没有正反馈）即可实现的理论群体性能。由于负反馈过程，例如一个机器人的碰撞避免行为会触发其他几个机器人在高密度情况下的碰撞避免行为，非线性效应会随着群体规模的增加而降低效率是合理的。尽管如此，有许多可用的非线性函数选项，但最好的结果是使用指数函数获得的。Lerman 和 Galstyan [^236] (图 10b) 也报告了在觅食任务中每个机器人的效率呈指数下降。















## 参考文献

[^1]: Abbott, R. (2004). Emergence, entities, entropy, and binding forces. In The Agent 2004 Conference on: Social Dynamics: Interaction, Reflexivity, and Emergence, Argonne National Labs and University of Chicago, October 2004.
[^2]: Abelson, H., Allen, D., Coore, D., Hanson, C., Homsy, G., Knight, T., et al. (2000). Amorphous computing. Communications of the ACM, 43(5), 74–82.
[^3]: Adamatzky, A. (2010). Physarum machines: Computers from slime mould. Singapore: World Scientific.
[^4]: Akhtar, N., Ozkasap, O., & Ergen, S. C. (2013). Vanet topology characteristics under realistic mobility and channel models. In 2013 IEEE Wireless Communications and Networking Conference (WCNC) (pp. 1774–1779). New York: IEEE.
[^5]: Allwright, M., Bhalla, N., & Dorigo, M. (2017). Structure and markings as stimuli for autonomous construction. In 2017 18th International Conference on Advanced Robotics (ICAR) (pp. 296–302). New York: IEEE.
[^6]: Allwright, M., Bhalla, N., El-faham, H., Antoun, A., Pinciroli, C., & Dorigo, M. (2014). SRoCS: Leveraging stigmergy on a multi-robot construction platform for unknown environments. In International Conference on Swarm Intelligence (pp. 158–169). Cham: Springer.
[^7]: Ame, J.-M., Rivault, C., & Deneubourg, J.-L. (2004). Cockroach aggregation based on strain odour recognition. Animal Behaviour, 68, 793–801.
[^8]: Anderson, C., Boomsma, J. J., & Bartholdi, J. J. (2002). Task partitioning in insect societies: Bucket brigades. Insectes Sociaux, 49, 171–180.
[^9]: Anderson, C., Theraulaz, G., & Deneubourg, J.-L. (2002). Self-assemblages in insect societies. Insectes Sociaux, 49(2), 99–110.
[^10]: Anderson, P. W. (1972). More is different. Science, 177(4047), 393–396.
[^11]: Arbuckle, D. J. (2007). Self-assembly and Self-repair by Robot Swarms, University of Southern California.
[^12]: Arbuckle, D. J., & Requicha, A. A. G. (2010). Self-assembly and self-repair of arbitrary shapes by a swarm of reactive robots: Algorithms and simulations. Autonomous Robots, 28(2), 197–211. ISSN 1573-7527. https://doi.org/10.1007/s10514-009-9162-7
[^13]: Arkin, R. C. (1998). Behavior-based robotics. Cambridge, MA: MIT Press.
[^14]: Arkin, R. C., & Egerstedt, M. (2015). Temporal heterogeneity and the value of slowness in robotic systems. In 2015 IEEE International Conference on Robotics and Biomimetics (ROBIO) (pp. 1000–1005). New York: IEEE.
[^15]: Arthur, W. B. (1989). Competing technologies, increasing returns, and lock-in by historical events. The Economic Journal, 99(394), 116–131.
[^16]: Arvin, F., Murray, J., Zhang, C., & Yue, S. (2014). Colias: An autonomous micro robot for swarm robotic applications. International Journal of Advanced Robotic Systems, 11(7), 113. http://dx.doi.org/10.5772/58730
[^17]: Arvin, F., Turgut, A. E., Bazyari, F., Arikan, K. B., Bellotto, N., & Yue, S. (2014). Cue-based aggregation with a mobile robot swarm: A novel fuzzy-based method. Adaptive Behavior, 22(3), 189–206.
[^18]: Arvin, F., Turgut, A. E., Krajník, T., & Yue, S. (2016). Investigation of cue-based aggregation in static and dynamic environments with a mobile robot swarm. Adaptive Behavior, 24(2), 102–118. https://doi.org/10.1177/1059712316632851
[^19]: Arvin, F., Turgut, A. E., & Yue, S. (2012). Fuzzy-based aggregation with a mobile robot swarm. In Swarm intelligence (ANTS’12). Lecture notes in computer science (Vol. 7461, pp. 346–347). Berlin: Springer. ISBN 978-3-642-32649-3. https://doi.org/10.1007/978-3-642- 32650-9_39
[^20]: Augugliaro, F., Lupashin, S., Hamer, M., Male, C., Hehn, M., Mueller, M. W., et al. (2014). The flight assembled architecture installation: Cooperative construction with flying machines. IEEE Control Systems, 34(4), 46–64.
[^21]: Axelrod, R. (1997). The dissemination of culture: A model with local convergence and global polarization. Journal of Conflict Resolution, 41(2), 203–226.
[^22]: Bachrach, J., & Beal, J. (2006). Programming a sensor network as an amorphous medium. In Distributed computing in sensor systems (DCOSS’06, extended abstract).
[^23]: Bak, P. (2013). How nature works: The science of self-organized criticality. Berlin: Springer Science & Business Media.
[^24]: Bak, P., Tang, C., & Wiesenfeld, K. (1988). Self-organized criticality. Physical Review A, 38(1), 364–374. https://doi.org/10.1103/PhysRevA.38.364
[^25]: Balch, T. (2000). Hierarchic social entropy: An information theoretic measure of robot group diversity. Autonomous Robots, 8(3), 209–238. ISSN 0929-5593.
[^26]: Ball, P. (2015). Forging patterns and making waves from biology to geology: A commentary on Turing (1952) ‘the chemical basis of morphogenesis’. Philosophical Transactions of the Royal Society of London B: Biological Sciences, 370(1666), 20140218. ISSN 0962-8436. https://doi.org/10.1098/rstb.2014.0218
[^27]: Ballerini, M., Cabibbo, N., Candelier, R., Cavagna, A., Cisbani, E., Giardina, I., et al. (2008). Interaction ruling animal collective behavior depends on topological rather than metric distance: Evidence from a field study. Proceedings of the National Academy of Sciences of the United States of America, 105(4), 1232–1237.
[^28]: Baluska, F., & Levin, M. (2016). On having no head: Cognition throughout biological  systems. Frontiers in Psychology, 7, 902. ISSN 1664-1078. https://www.frontiersin.org/ article/10.3389/fpsyg.2016.00902
[^29]: Baran, P. (1960). Reliable digital communications systems using unreliable network repeater nodes. Technical report, The RAND Corporation, Santa Monica, CA.
[^30]: Barrow-Green, J. (1997). Poincaré and the three body problem. American Mathematical Society, London: London Mathematical Society.
[^31]: Bass, F. M. (1969). A new product growth for model consumer durables. Management Science, 15(5), 215–227.
[^32]: Bayindir, L. (2015). A review of swarm robotics tasks. Neurocomputing, 172(C), 292–321. http://dx.doi.org/10.1016/j.neucom.2015.05.116
[^33]: Bayindir, L., & ¸ Sahin, E. (2007). A review of studies in swarm robotics. Turkish Journal of Electrical Engineering and Computer Sciences, 15, 115–147. http://journals.tubitak.gov.tr/ elektrik/issues/elk-07-15-2/elk-15-2-2-0705-13.pdf
[^34]: Beckers, R., Holland, O. E., & Deneubourg, J.-L. (1994). From local actions to global tasks: Stigmergy and collective robotics. In Artificial life IV (pp. 189–197). Cambridge, MA: MIT Press.
[^35]: Bekey, G., Ambrose, R., Kumar, V., Lavery, D., Sanderson, A., Wilcox, B., et al. (2008). Robotics: State of the art and future challenges. Singapore: World Scientific.
[^36]: Belykh, I., Jeter, R., & Belykh, V. (2017). Foot force models of crowd dynamics on a wobbly bridge. Science Advances, 3(11), e1701512. https://doi.org/10.1126/sciadv.1701512
[^37]: Beni, G. (2005). From swarm intelligence to swarm robotics. In E. ¸ Sahin & W. M. Spears (Eds.), Swarm Robotics - SAB 2004 International Workshop, Santa Monica, CA, July 2005. Lecture notes in computer science (Vol. 3342, pp. 1–9). Berlin: Springer. http://dx.doi.org/10. 1007/978-3-540-30552-1_1
[^38]: Berea, A., Cohen, I., D’Orsogna, M. R., Ghosh, K., Goldenfeld, N., Goodnight, C. J., et al. (2014). IDR team summary 6. In Collective behavior: From cells to societies. Washington, DC: The National Academies Press.
[^39]: Berg, B. A., & Billoire, A. (2007). Markov chain Monte Carlo simulations. In B. W. Wah (Ed.), Wiley encyclopedia of computer science and engineering. New York: Wiley. https://dx. doi.org/10.1002/9780470050118.ecse696
[^40]: Berman, S., Lindsey, Q., Sakar, M. S., Kumar, V., & Pratt, S. C. (2011). Experimental study and modeling of group retrieval in ants as an approach to collective transport in swarm robotic systems. Proceedings of the IEEE, 99(9), 1470–1481. ISSN 0018-9219. https://doi.org/10. 1109/JPROC.2011.2111450
[^41]: Biancalani, T., Dyson, L., & McKane, A. J. (2014). Noise-induced bistable states and their mean switching time in foraging colonies. Physical Review Letters, 112, 038101. http://link. aps.org/doi/10.1103/PhysRevLett.112.038101
[^42]: Bjerknes, J. D., & Winfield, A. (2013). On fault-tolerance and scalability of swarm robotic systems. In A. Martinoli, F. Mondada, N. Correll, G. Mermoud, M. Egerstedt, M. Ani Hsieh, L. E. Parker, & K. Støy (Eds.), Distributed autonomous robotic systems (DARS 2010). Springer tracts in advanced robotics (Vol. 83, pp. 431–444). Berlin: Springer. ISBN 978-3- 642-32722-3. http://dx.doi.org/10.1007/978-3-642-32723-0_31
[^43]: Bjerknes, J. D., Winfield, A., & Melhuish, C. (2007). An analysis of emergent taxis in a wireless connected swarm of mobile robots. In Y. Shi & M. Dorigo (Eds.), IEEE Swarm Intelligence Symposium, Los Alamitos, CA (pp. 45–52). New York: IEEE Press.
[^44]: Bodi, M., Thenius, R., Schmickl, T., & Crailsheim, K. (2011). How two cooperating robot swarms are affected by two conflictive aggregation spots. In Advances in Artificial Life: Darwin Meets von Neumann (ECAL’09). Lecture notes in computer science (Vol. 5778, pp. 367–374). Heidelberg/Berlin: Springer.
[^45]: Bodi, M., Thenius, R., Szopek, M., Schmickl, T., & Crailsheim, K. (2011). Interaction of robot swarms using the honeybee-inspired control algorithm BEECLUST. Mathematical and Computer Modelling of Dynamical Systems, 18, 87–101. http://www.tandfonline.com/doi/ abs/10.1080/13873954.2011.601420
[^46]: Bogacz, R., Brown, E., Moehlis, J., Holmes, P., & Cohen, J. D. (2006). The physics of optimal decision making: A formal analysis of models of performance in two-alternative forcedchoice tasks. Psychological Review, 113(4), 700.
[^47]: Bonabeau, E. (2002). Predicting the unpredictable. Harvard Business Review, 80(3), 109–116.
[^48]: Bonabeau, E., Dorigo, M., & Theraulaz, G. (1999). Swarm intelligence: From natural to artificial systems. New York, NY: Oxford University Press.
[^49]: Bonani, M., Longchamp, V., Magnenat, S., Rétornaz, P., Burnier, D., Roulet, G., et al. (2010). The marXbot, a miniature mobile robot opening new perspectives for the collective-robotic research. In IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS) (pp. 4187–4193). New York: IEEE.
[^50]: Bongard, J. C. (2013). Evolutionary robotics. Communications of the ACM, 56(8), 74–83. http://doi.acm.org/10.1145/2493883
[^51]: Bonnet, F., Cazenille, L., Gribovskiy, A., Halloy, J., & Mondada, F. (2017). Multi-robot control and tracking framework for bio-hybrid systems with closed-loop interaction. In 2017 IEEE International Conference on Robotics and Automation (ICRA), May 2017 (pp. 4449– 4456). https://doi.org/10.1109/ICRA.2017.7989515
[^52]: Braitenberg, V. (1984). Vehicles: Experiments in synthetic psychology. Cambridge, MA: MIT Press.
[^53]: Brambilla, M. (2014). Formal Methods for the Design and Analysis of Robot Swarms. PhD thesis, Université Libre de Bruxelles.
[^54]: Brambilla, M., Ferrante, E., Birattari, M., & Dorigo, M. (2013). Swarm robotics: A review from the swarm engineering perspective. Swarm Intelligence, 7(1), 1–41. ISSN 1935-3812. http://dx.doi.org/10.1007/s11721-012-0075-2
[^55]: Breder, C. M. (1954). Equations descriptive of fish schools and other animal aggregations. Ecology, 35(3), 361–370.
[^56]: Brooks, R. (1986). A robust layered control system for a mobile robot. IEEE Journal of Robotics and Automation, 2(1), 14–23.
[^57]: Brooks, R. A. (1991). Intelligence without representation. Artificial Intelligence, 47, 139–159.
[^58]: Brutschy, A., Pini, G., Pinciroli, C., Birattari, M., & Dorigo, M. (2014). Self-organized task allocation to sequentially interdependent tasks in swarm robotics. Autonomous Agents and Multi-Agent Systems, 28(1), 101–125. ISSN 1387-2532. http://dx.doi.org/10.1007/s10458- 012-9212-y
[^59]: Buhl, J., Sumpter, D. J. T., Couzin, I. D., Hale, J. J., Despland, E., Miller, E. R., & Simpson, S. J. (2006). From disorder to order in marching locusts. Science, 312(5778), 1402–1406. https://doi.org10.1126/science.1125142
[^60]: Camazine, S., Deneubourg, J.-L., Franks, N. R., Sneyd, J., Theraulaz, G., & Bonabeau, E. (2001). Self-organizing biological systems. Princeton, NJ: Princeton University Press.
[^61]: Campo, A., Garnier, S., Dédriche, O., Zekkri, M., & Dorigo, M. (2011). Self-organized discrimination of resources. PLoS One, 6(5), e19888.
[^62]: Caprari, G., Balmer, P., Piguet, R., & Siegwart, R. (1998). The autonomous microbot ‘Alice’: A platform for scientific and commercial applications. In Proceedings of the Ninth International Symposium on Micromechatronics and Human Science, Nagoya, Japan (pp. 231–235).
[^63]: Caprari, G., Colot, A., Siegwart, R., Halloy, J., & Deneubourg, J.-L. (2005). Animal and robot mixed societies: Building cooperation between microrobots and cockroaches. IEEE Robotics & Automation Magazine, 12(2), 58–65. https://doi.org/10.1109/MRA.2005.1458325
[^64]: Carlsson, H., & Van Damme, E. (1993). Global games and equilibrium selection. Econometrica: Journal of the Econometric Society, 6(5), 989–1018.
[^65]: Carneiro, J., Leon, K., Caramalho, Í., Van Den Dool, C., Gardner, R., Oliveira, V., et al. (2007). When three is not a crowd: A crossregulation model of the dynamics and repertoire selection of regulatory cd4+ t cells. Immunological Reviews, 216(1), 48–68.
[^66]: Castellano, C., Fortunato, S., & Loreto, V. (2009). Statistical physics of social dynamics. Reviews of Modern Physics, 81, 591–646. https://link.aps.org/doi/10.1103/RevModPhys.81. 591
[^67]: Cavalcanti, A., & Freitas, R. A. Jr. (2005). Nanorobotics control design: A collective behavior approach for medicine. IEEE Transactions on NanoBioscience, 4(2), 133–140.
[^68]: Chaimowicz, L., & Kumar, V. (2007). Aerial shepherds: Coordination among UAVs and swarms of robots. In Distributed Autonomous Robotic Systems 6 (pp. 243–252). Berlin: Springer.
[^69]: Chazelle, B. (2015). An algorithmic approach to collective behavior. Journal of Statistical Physics, 158(3), 514–548.
[^70]: Chen, J., Gauci, M., Price, M. J., & Groß, R. (2012). Segregation in swarms of e-puck robots based on the Brazil nut effect. In Proceedings of the 11th International Conference on Autonomous Agents and Multiagent Systems (AAMAS 2012), Richland, SC (pp. 163–170). IFAAMAS.
[^71]: Christensen, A. L., O’Grady, R., & Dorigo, M. (2009). From fireflies to fault-tolerant swarms of robots. IEEE Transactions on Evolutionary Computation, 13(4), 754–766. ISSN 1089- 778X. https://doi.org/10.1109/TEVC.2009.2017516
[^72]: Ciocchetta, F., & Hillston, J. (2009). Bio-PEPA: A framework for the modelling and analysis of biological systems. Theoretical Computer Science, 410(33), 3065–3084.
[^73]: Clifford, P., & Sudbury, A. (1973). A model for spatial conflict. Biometrika, 60(3), 581–588. https://doi.org/10.1093/biomet/60.3.581.
[^74]: Correll, N., & Hamann, H. (2015). Probabilistic modeling of swarming systems. In J. Kacprzyk & W. Pedrycz (Eds.) Springer handbook of computational intelligence (pp. 1423– 1431). Berlin: Springer.
[^75]: Correll, N., Schwager, M., & Rus, D. (2008). Social control of herd animals by integration of artificially controlled congeners. In Proceedings of the 10th International Conference on Simulation of Adaptive Behavior: From Animals to Animats. Lecture notes in computer science (Vol. 5040, pp. 437–446). Berlin: Springer.
[^76]: Couzin, I. D., Ioannou, C. C., Demirel, G., Gross, T., Torney, C. J., Hartnett, A., et al. (2011). Uninformed individuals promote democratic consensus in animal groups. Science, 334(6062), 1578–1580. ISSN 0036-8075. https://doi.org/10.1126/science.1210280. http:// science.sciencemag.org/content/334/6062/1578 
[^77]: Couzin, I. D., Krause, J., Franks, N. R., & Levin, S. A. (2005). Effective leadership and decision-making in animal groups on the move. Nature, 433, 513–516.
[^78]: Couzin, I. D., Krause, J., James, R., Ruxton, G. D., & Franks, N. R. (2002). Collective memory and spatial sorting in animal groups. Journal of Theoretical Biology, 218, 1–11. https://doi.org/10.1006/jtbi.2002.3065 
[^79]: Crespi, V., Galstyan, A., & Lerman, K. (2008). Top-down vs bottom-up methodologies in multi-agent system design. Autonomous Robots, 24(3), 303–313.
[^80]: Crutchfield, J. (1994). The calculi of emergence: Computation, dynamics, and induction. Physica D, 75(1–3), 11–54.
[^81]: Crutchfield, J. (1994). Is anything ever new? In G. Cowan, D. Pines, & D. Melzner (Eds.), Complexity: metaphors, models, and reality. SFI series in the sciences of complexity proceedings (Vol. 19, pp. 479–497). Reading, MA: Addison-Wesley.
[^82]: da Silva Guerra, R., Aonuma, H., Hosoda, K., & Asada, M. (2010). Behavior change of crickets in a robot-mixed society. Journal of Robotics and Mechatronics, 22, 526–531.
[^83]: Darley, V. (1994). Emergent phenomena and complexity. In R. Brooks & P. Maes (Eds.), Artificial life IV (pp. 411–416). Cambridge, MA: MIT Press.
[^84]: De Nardi, R., & Holland, O. E. (2007). Ultraswarm: A further step towards a flock of miniature helicopters. In E. ¸ Sahin, W. M. Spears, & A. F. T. Winfield (Eds.), Swarm Robotics - Second SAB 2006 International Workshop. Lecture notes in computer science (Vol. 4433, pp. 116–128). Berlin: Springer.
[^85]: De Nicola, R., Ferrari, G. L., & Pugliese, R. (1998). KLAIM: A kernel language for agents interaction and mobility. IEEE Transactions on Software Engineering, 24(5), 315–330.
[^86]: De Nicola, R., Katoen, J., Latella, D., Loreti, M., & Massink, M. (2007). Model checking mobile stochastic logic. Theoretical Computer Science, 382(1), 42–70.
[^87]: de Oca, M. M., Ferrante, E., Scheidler, A., Pinciroli, C., Birattari, M., & Dorigo, M. (2011). Majority-rule opinion dynamics with differential latency: A mechanism for self-organized collective decision-making. Swarm Intelligence, 5, 305–327. ISSN 1935-3812. http://dx.doi. org/10.1007/s11721-011-0062-z 
[^88]: De Wolf, T., & Holvoet, T. (2005). Emergence versus self-organisation: Different concepts but promising when combined. In S. Brueckner, G. D. M. Serugendo, A. Karageorgos, & R. Nagpal (Eds.), Proceedings of the Workshop on Engineerings Self Organising Applications. Lecture notes in computer science (Vol. 3464, pp. 1–15). Berlin: Springer.
[^89]: Deguet, J., Demazeau, Y., & Magnin, L. (2006). Elements about the emergence issue: A survey of emergence definitions. Complexus, 3(1–3), 24–31.
[^90]: Deneubourg, J.-L., Gregoire, J.-C., & Le Fort, E. (1990). Kinetics of larval gregarious behavior in the bark beetle Dendroctonus micans (coleoptera: Scolytidae). Journal of Insect Behavior, 3(2), 169–182.
[^91]: Deneubourg, J.-L., Lioni, A., & Detrain, C. (2002). Dynamics of aggregation and emergence of cooperation. Biological Bulletin, 202, 262–267.
[^92]: Deng, B. (2015). Machine ethics: The robot’s dilemma. Nature, 523, 24–66. http://dx.doi.org/ 10.1038/523024a 
[^93]: Ding, H., & Hamann, H. (2014). Sorting in swarm robots using communication-based cluster size estimation. In M. Dorigo, M. Birattari, S. Garnier, H. Hamann, M. M. de Oca, C. Solnon, & T. Stützle (Eds.), Ninth International Conference on Swarm Intelligence (ANTS 2014). Lecture notes in computer science (Vol. 8667, pp. 262–269). Berlin: Springer.
[^94]: Divband Soorati, M., & Hamann, H. (2016). Robot self-assembly as adaptive growth process: Collective selection of seed position and self-organizing tree-structures. In IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS 2016) (pp. 5745–5750). New York: IEEE. http://dx.doi.org/10.1109/IROS.2016.7759845 
[^95]: Dixon, C., Winfield, A., & Fisher, M. (2011). Towards temporal verification of emergent behaviours in swarm robotic systems. Towards Autonomous Robotic Systems (TAROS) (pp. 336–347). Berlin: Springer.
[^96]: Dorigo, M., Birattari, M., & Brambilla, M. (2014). Swarm robotics. Scholarpedia, 9(1), 1463. http://dx.doi.org/10.4249/scholarpedia.1463 
[^97]: Dorigo, M., Bonabeau, E., & Theraulaz, G. (2000). Ant algorithms and stigmergy. Future Generation Computer Systems, 16(9), 851–871.
[^98]: Dorigo, M., Floreano, D., Gambardella, L. M., Mondada, F., Nolfi, S., Baaboura, T., et al. (2013). Swarmanoid: A novel concept for the study of heterogeneous robotic swarms. IEEE Robotics & Automation Magazine, 20(4), 60–71.
[^99]: Dorigo, M., & ¸ Sahin, E. (2004). Guest editorial: Swarm robotics. Autonomous Robots, 17(2– 3), 111–113.
[^100]: Dorigo, M., Trianni, V., Sahin, E., Groß, R., Labella, T. H., Baldassarre, G., et al. (2004). Evolving self-organizing behaviors for a swarm-bot. Autonomous Robots, 17, 223–245. https://doi.org/10.1023/B:AURO.0000033972.50769.1c.
[^101]: Dorigo, M., Tuci, E., Trianni, V., Groß, R., Nouyan, S., Ampatzis, C., et al. (2006). SWARMBOT: Design and implementation of colonies of self-assembling robots. In G. Y. Yen & D. B. Fogel (Eds.), Computational intelligence: Principles and practice (pp. 103–135). Los Alamitos, CA: IEEE Press.
[^102]: Douven, I., & Riegler, A. (2009). Extending the Hegselmann–Krause model I. Logic Journal of IGPL, 18(2), 323–335.
[^103]: Duarte, M., Costa, V., Gomes, J., Rodrigues, T., Silva, F., Oliveira, S. M., et al. (2016). Evolution of collective behaviors for a real swarm of aquatic surface robots. PLoS One, 11(3), 1–25. https://doi.org/10.1371/journal.pone.0151834.
[^104]: Duarte, M., Costa, V., Gomes, J., Rodrigues, T., Silva, F., Oliveira, S. M., et al. (2016). Unleashing the potential of evolutionary swarm robotics in the real world. In Proceedings of the 2016 on Genetic and Evolutionary Computation Conference Companion, GECCO ’16 Companion, New York, NY, USA (pp. 159–160). New York: ACM. ISBN 978-1-4503-43237. http://doi.acm.org/10.1145/2908961.2930951 
[^105]: Ducatelle, F., Di Caro, G. A., & Gambardella, L. M. (2010). Cooperative self-organization in a heterogeneous swarm robotic system. In Proceedings of the 12th Conference on Genetic and Evolutionary Computation (GECCO) (pp. 87–94). New York: ACM.
[^106]: Dussutour, A., Beekman, M., Nicolis, S. C., & Meyer, B. (2009). Noise improves collective decision-making by ants in dynamic environments. Proceedings of the Royal Society London B, 276, 4353–4361.
[^107]: Dussutour, A., Fourcassié, V., Helbing, D., & Deneubourg, J.-L. (2004). Optimal traffic organization in ants under crowded conditions. Nature, 428, 70–73.
[^108]: Ehrenfest, P., & Ehrenfest, T. (1907). Über zwei bekannte Einwände gegen das Boltzmannsche H-Theorem. Physikalische Zeitschrift, 8, 311–314.
[^109]: Eiben, Á. E., & Smith, J. E. (2003). Introduction to evolutionary computing. Natural computing series. Berlin: Springer.
[^110]: Eigen, M., & Schuster, P. (1977). A principle of natural self-organization. Naturwissenschaften, 64(11), 541–565. ISSN 0028-1042. http://dx.doi.org/10.1007/BF00450633
[^111]: Eigen, M., & Schuster, P. (1979). The hypercycle: A principle of natural self organization. Berlin: Springer.
[^112]: Eigen, M., & Winkler, R. (1993). Laws of the game: How the principles of nature govern chance. Princeton, NJ: Princeton University Press. ISBN 978-0-691-02566-7.
[^113]: Elmenreich, W., Heiden, B., Reiner, G., & Zhevzhyk, S. (2015). A low-cost robot for multirobot experiments. In 12th International Workshop on Intelligent Solutions in Embedded Systems (WISES) (pp. 127–132). New York: IEEE.
[^114]: Erdmann, U., Ebeling, W., Schimansky-Geier, L., & Schweitzer, F. (2000). Brownian particles far from equilibrium. The European Physical Journal B - Condensed Matter and Complex Systems, 15(1), 105–113.
[^115]: Erdos, P., & Rényi, A. (1959). On random graphs. ˝ Publicationes Mathematicae Debrecen, 6(290–297), 156.
[^116]: Farrow, N., Klingner, J., Reishus, D., & Correll, N. (2014). Miniature six-channel range and bearing system: Algorithm, analysis and experimental validation. In 2014 IEEE International Conference on Robotics and Automation (ICRA) (pp. 6180–6185). New York: IEEE.
[^117]: Ferber, J. (1999). Multi-agent systems: An introduction to distributed artificial intelligence. New York: Addison-Wesley.
[^118]: Ferrante, E., Dúeñez Guzman, E., Turgut, A. E., & Wenseleers, T. (2013). Evolution of task partitioning in swarm robotics. In V. Trianni (Ed.), Proceedings of the Workshop on Collective Behaviors and Social Dynamics of the European Conference on Artificial Life (ECAL 2013). Cambridge, MA: MIT Press.
[^119]: Ferrante, E., Turgut, A. E., Duéñez-Guzmàn, E., Dorigo, M., & Wenseleers, T. (2015). Evolution of self-organized task specialization in robot swarms. PLoS Computational Biology, 11(8), e1004273. https://doi.org/10.1371/journal.pcbi.1004273.
[^120]: Ferrer, E. C. (2016). The blockchain: A new framework for robotic swarm systems. Preprint arXiv:1608.00695. https://arxiv.org/pdf/1608.00695
[^121]: Flora robotica. (2017). Project website. http://www.florarobotica.eu
[^122]: Floreano, D., & Mattiussi, C. (2008). Bio-inspired artificial intelligence: Theories, methods, and technologies. Cambridge, MA: MIT Press.
[^123]: Fokker, A. D. (1914). Die mittlere Energie rotierender elektrischer Dipole im Strahlungsfeld. Annalen der Physik, 348(5), 810–820.
[^124]: Ford, L. R. Jr, & Fulkerson, D. R. (2015). Flows in networks. Princeton, NJ: Princeton University Press.
[^125]: Francesca, G., Brambilla, M., Brutschy, A., Garattoni, L., Miletitch, R., Podevijn, G., et al. (2014). An experiment in automatic design of robot swarms: Automode-vanilla, evostick, and human experts. In M. Dorigo, M. Birattari, S. Garnier, H. Hamann, M. M. de Oca, C. Solnon, & T. Stützle (Eds.), Ninth International Conference on Swarm Intelligence (ANTS 2014). Lecture notes in computer science (Vol. 8667, pp. 25–37).
[^126]: Franks, N. R., Dornhaus, A., Fitzsimmons, J. P., & Stevens, M. (2003). Speed versus accuracy in collective decision making. Proceedings of the Royal Society of London - Series B: Biological Sciences, 270, 2457–2463.
[^127]: Franks, N. R., & Sendova-Franks, A. B. (1992). Brood sorting by ants: Distributing the workload over the work-surface. Behavioral Ecology and Sociobiology, 30(2), 109–123.
[^128]: Franks, N. R., Wilby, A., Silverman, B. W., & Tofts, C. (1992). Self-organizing nest construction in ants: Sophisticated building by blind bulldozing. Animal Behaviour, 44, 357–375.
[^129]: Freeman, P. R. (1983). The secretary problem and its extensions: A review. International Statistical Review/Revue Internationale de Statistique, 51(2), 189–206.
[^130]: Frei, R., & Serugendo, G. D. M. (2012). The future of complexity engineering. Centreal European Journal of Engineering, 2(2), 164–188. http://dx.doi.org/10.2478/s13531-011- 0071-0
[^131]: Galam, S. (1997). Rational group decision making: A random field Ising model at T=0. Physica A, 238(1–4), 66–80.
[^132]: Galam, S. (2004). Contrarian deterministic effect on opinion dynamics: The “hung elections scenario”. Physica A, 333(1), 453–460. http://dx.doi.org/10.1016/j.physa.2003.10.041
[^133]: Galam, S. (2008). Sociophysics: A review of Galam models. International Journal of Modern Physics C, 19(3), 409–440.
[^134]: Galam, S., & Moscovici, S. (1991). Towards a theory of collective phenomena: Consensus and attitude changes in groups. European Journal of Social Psychology, 21(1), 49–74. https:// doi.org/10.1002/ejsp.2420210105
[^135]: Galam, S., & Moscovici, S. (1994). Towards a theory of collective phenomena. II: Conformity and power. European Journal of Social Psychology, 24(4), 481–495.
[^136]: Galam, S., & Moscovici, S. (1995). Towards a theory of collective phenomena. III: Conflicts and forms of power. European Journal of Social Psychology, 25(2), 217–229.
[^137]: Garnier, S., Gautrais, J., Asadpour, M., Jost, C., & Theraulaz, G. (2009). Self-organized aggregation triggers collective decision making in a group of cockroach-like robots. Adaptive Behavior, 17(2), 109–133.
[^138]: Garnier, S., Murphy, T., Lutz, M., Hurme, E., Leblanc, S., & Couzin, I. D. (2013). Stability and responsiveness in a self-organized living architecture. PLoS Computational Biology, 9(3), e1002984. https://doi.org/10.1371/journal.pcbi.1002984.
[^139]: Gates, B. (2007). A robot in every home. Scientific American, 296(1), 58–65.
[^140]: Gauci, M., Nagpal, R., & Rubenstein, M. (2016). Programmable self-disassembly for shape formation in large-scale robot collectives. In 13th International Symposium on Distributed Autonomous Robotic Systems (DARS 16).
[^141]: Gauci, M., Ortiz, M. E., Rubenstein, M., & Nagpal, R. (2017). Error cascades in collective behavior: A case study of the gradient algorithm on 1000 physical agents. In Proceedings of the 16th Conference on Autonomous Agents and MultiAgent Systems (pp. 1404–1412). International Foundation for Autonomous Agents and Multiagent Systems.
[^142]: Gerkey, B., Vaughan, R. T., & Howard, A. (2003). The player/stage project: Tools for multirobot and distributed sensor systems. In Proceedings of the 11th International Conference on Advanced Robotics (ICAR 2003) (pp. 317–323).
[^143]: Gerling, V., & von Mammen, S. (2016). Robotics for self-organised construction. In 2016 IEEE 1st International Workshops on Foundations and Applications of Self* Systems (FAS*W), September 2016 (pp. 162–167). https://doi.org/10.1109/FAS-W.2016.45
[^144]: Gierer, A., & Meinhardt, H. (1972). A theory of biological pattern formation. Biological Cybernetics, 12(1), 30–39. http://dx.doi.org/10.1007/BF00289234
[^145]: Gjondrekaj, E., Loreti, M., Pugliese, R., Tiezzi, F., Pinciroli, C., Brambilla, M., et al. (2012). Towards a formal verification methodology for collective robotic systems. In Formal methods and software engineering (pp. 54–70). Berlin: Springer.
[^146]: Goldstein, I., & Pauzner, A. (2005). Demand–deposit contracts and the probability of bank runs. The Journal of Finance, 60(3), 1293–1327.
[^147]: Gomes, J., Mariano, P., & Christensen, A. L. (2015). Cooperative coevolution of partially heterogeneous multiagent systems. In Proceedings of the 2015 International Conference on Autonomous Agents and Multiagent Systems (pp. 297–305). International Foundation for Autonomous Agents and Multiagent Systems.
[^148]: Gomes, J., Urbano, P., & Christensen, A. L. (2013). Evolution of swarm robotics systems with novelty search. Swarm Intelligence, 7(2–3), 115–144.
[^149]: Gordon, D. M. (1996). The organization of work in social insect colonies. Nature, 380, 121– 124. https://doi.org/10.1038/380121a0
[^150]: Graham, R., Knuth, D., & Patashnik, O. (1998). Concrete mathematics: A foundation for computer science. Reading, MA: Addison-Wesley. ISBN 0-201-55802-5.
[^151]: Grassé, P.-P. (1959). La reconstruction du nid et les coordinations interindividuelles chez bellicositermes natalensis et cubitermes sp. la théorie de la stigmergie: essai d’interprétation du comportement des termites constructeurs. Insectes Sociaux, 6, 41–83.
[^152]: Gribovskiy, A., Halloy, J., Deneubourg, J.-L., Bleuler, H., & Mondada, F. (2010). Towards mixed societies of chickens and robots. In 2010 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS) (pp. 4722–4728). New York: IEEE.
[^153]: Grimmett, G. (1999). Percolation. Grundlehren der mathematischen Wissenschaften (Vol. 321). Berlin: Springer.
[^154]: Groß, R., & Dorigo, M. (2008). Evolution of solitary and group transport behaviors for autonomous robots capable of self-assembling. Adaptive Behavior, 16(5), 285–305.
[^155]: Groß, R., Magnenat, S., & Mondada, F. (2009). Segregation in swarms of mobile robots based on the Brazil nut effect. In IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS 2009) (pp. 4349–4356). New York: IEEE.
[^156]: Gross, T., & Sayama, H. (2009). Adaptive networks: Theory, models, and data. Berlin: Springer.
[^157]: Guillermo, M. (2005). Morphogens and synaptogenesis in drosophila. Journal of Neurobiology, 64(4), 417–434. http://dx.doi.org/10.1002/neu.20165. 
[^158]: Gunther, N. J. (1993). A simple capacity model of massively parallel transaction systems. In CMG National Conference (pp. 1035–1044).
[^159]: Gunther, N. J., Puglia, P., & Tomasette, K. (2015). Hadoop super-linear scalability: The perpetual motion of parallel performance. ACM Queue, 13(5), 46–55.
[^160]: Habibi, G., Xie, W., Jellins, M., & McLurkin, J. (2016). Distributed path planning for collective transport using homogeneous multi-robot systems. In N.-Y. Chong & Y.-J. Cho (Eds.), Distributed Autonomous Robotic Systems: The 12th International Symposium (pp. 151–164). Tokyo: Springer. ISBN 978-4-431-55879-8. https://doi.org/10.1007/978-4-431- 55879-8_11 
[^161]: Haken, H. (1977). Synergetics - An introduction. Berlin: Springer.
[^162]: Haken, H. (2004). Synergetics: Introduction and advanced topics. Berlin: Springer.
[^163]: Halloy, J., Sempo, G., Caprari, G., Rivault, C., Asadpour, M., Tâche, F., et al. (2007). Social integration of robots into groups of cockroaches to control self-organized choices. Science, 318(5853), 1155–1158. http://dx.doi.org/10.1126/science.1144259.
[^164]: Hamann, H. (2006). Modeling and Investigation of Robot Swarms. Master’s thesis, University of Stuttgart, Germany.
[^165]: Hamann, H. (2010). Space-time continuous models of swarm robotics systems: Supporting global-to-local programming. Berlin: Springer.
[^166]: Hamann, H. (2013). Towards swarm calculus: Urn models of collective decisions and universal properties of swarm performance. Swarm Intelligence, 7(2–3), 145–172. http://dx. doi.org/10.1007/s11721-013-0080-0 
[^167]: Hamann, H., Divband Soorati, M., Heinrich, M. K., Hofstadler, D. N., Kuksin, I., Veenstra, F., et al. (2017). Flora robotica - An architectural system combining living natural plants and distributed robots. Preprint arXiv:1709.04291.
[^168]: Hamann, H., Meyer, B., Schmickl, T., & Crailsheim, K. (2010). A model of symmetry breaking in collective decision-making. In S. Doncieux, B. Girard, A. Guillot, J. Hallam, J.-A. Meyer, & J.-B. Mouret (Eds.), From animals to animats 11. Lecture notes in artificial intelligence (Vol. 6226, pp. 639–648), Berlin: Springer. http://dx.doi.org/10.1007/978-3-642- 15193-4_60 
[^169]: Hamann, H., Schmickl, T., & Crailsheim, K. (2012). A hormone-based controller for evaluation-minimal evolution in decentrally controlled systems. Artificial Life, 18(2), 165– 198. http://dx.doi.org/10.1162/artl_a_00058 
[^170]: Hamann, H., Schmickl, T., & Crailsheim, K. (2012). Self-organized pattern formation in a swarm system as a transient phenomenon of non-linear dynamics. Mathematical and Computer Modelling of Dynamical Systems, 18(1), 39–50. http://www.tandfonline.com/doi/ abs/10.1080/13873954.2011.601418
[^171]: Hamann, H., Schmickl, T., Wörn, H., & Crailsheim, K. (2012). Analysis of emergent symmetry breaking in collective decision making. Neural Computing & Applications, 21(2), 207–218. http://dx.doi.org/10.1007/s00521-010-0368-6
[^172]: Hamann, H., Stradner, J., Schmickl, T., & Crailsheim, K. (2010). A hormone-based controller for evolutionary multi-modular robotics: From single modules to gait learning. In Proceedings of the IEEE Congress on Evolutionary Computation (CEC’10) (pp. 244–251).
[^173]: Hamann, H., Szymanski, M., & Wörn, H. (2007). Orientation in a trail network by exploiting its geometry for swarm robotics. In Y. Shi & M. Dorigo (Eds.), IEEE Swarm Intelligence Symposium, Honolulu, USA, April 1–5, Los Alamitos, CA, April 2007 (pp. 310–315). New York: IEEE Press.
[^174]: Hamann, H., Wahby, M., Schmickl, T., Zahadat, P., Hofstadler, D., Støy, K., et al. (2015).
[^175]:  robotica – Mixed societies of symbiotic robot-plant bio-hybrids. In Proceedings of IEEE Symposium on Computational Intelligence (IEEE SSCI 2015) (pp. 1102–1109). New York: IEEE. http://dx.doi.org/10.1109/SSCI.2015.158
[^176]: Hamann, H., & Wörn, H. (2007). An analytical and spatial model of foraging in a swarm of robots. In E. ¸ Sahin, W. Spears, & A. F. T. Winfield (Eds.), Swarm Robotics - Second SAB 2006 International Workshop. Lecture notes in computer science (Vol. 4433, pp. 43– 55). Berlin/Heidelberg: Springer.
[^177]: Hamann, H., & Wörn, H. (2008). A framework of space-time continuous models for algorithm design in swarm robotics. Swarm Intelligence, 2(2–4), 209–239. http://dx.doi.org/10.1007/ s11721-008-0015-3
[^178]: Hansell, M. H. (1984). Animal architecture and building behaviour. London: Longman.
[^179]: Harada, K., Corradi, P., Popesku, S., & Liedke, J. (2010). Heterogeneous multi-robot systems. In P. Levi & S. Kernbach (Eds.), Symbiotic multi-robot organisms: Reliability, adaptability, evolution. Cognitive systems monographs (Vol. 7, pp. 79–163). Berlin: Springer.
[^180]: Harriott, C. E., Seiffert, A. E., Hayes, S. T., & Adams, J. A. (2014). Biologically-inspired human-swarm interaction metrics. Proceedings of the Human Factors and Ergonomics Society Annual Meeting, 58(1), 1471–1475. https://doi.org/10.1177/1541931214581307
[^181]: Hasegawa, E., Mizumoto, N., Kobayashi, K., Dobata, S., Yoshimura, J., Watanabe, S., et al. (2017). Nature of collective decision-making by simple yes/no decision units. Scientific Reports, 7(1), 14436. https://doi.org/10.1038/s41598-017-14626-z 
[^182]: Hawking, S., & Israel, W. (1987). 300 years of gravitation. Cambridge: Cambridge University Press.
[^183]: Hayes, A. T. (2002). How many robots? group size and efficiency in collective search tasks. In H. Asama, T. Arai, T. Fukuda, & T. Hasegawa (Eds.), Distributed autonomous robotic systems 5 (pp. 289–298). Tokyo: Springer. ISBN 978-4-431-65941-9. http://dx.doi.org/10.1007/978- 4-431-65941-9_29
[^184]: Hegselmann, R., & Krause, U. (2002). Opinion dynamics and bounded confidence models, analysis, and simulation. Journal of Artifical Societies and Social Simulation, 5(3), 1–24.
[^185]: Heinrich, M. K., Wahby, M., Soorati, M. D., Hofstadler, D. N., Zahadat, P., Ayres, P., et al. (2016). Self-organized construction with continuous building material: Higher flexibility based on braided structures. In Proceedings of the 1st International Workshop on SelfOrganising Construction (SOCO) (pp. 154–159). New York: IEEE. https://doi.org/10.1109/ FAS-W.2016.43
[^186]: Helbing, D., Keltsch, J., & Molnár, P. (1997). Modelling the evolution of human trail systems. Nature, 388, 47–50.
[^187]: Helbing, D., Schweitzer, F., Keltsch, J., & Molnár, P. (1997). Active walker model for the formation of human and animal trail systems. Physical Review E, 56(3), 2527–2539.
[^188]: Hereford, J. M. (2011). Analysis of BEECLUST swarm algorithm. In Proceedings of the IEEE Symposium on Swarm Intelligence (SIS 2011) (pp. 192–198). New York: IEEE.
[^189]: Hoddell, S., Melhuish, C., & Holland, O. (1998). Collective sorting and segregation in robots with minimal sensing. In 5th International Conference on the Simulation of Adaptive Behaviour (SAB). Cambridge, MA: MIT Press.
[^190]: Hogg, T. (2006). Coordinating microscopic robots in viscous fluids. Autonomous Agents and Multi-Agent Systems, 14(3), 271–305.
[^191]: Holland, J. H. (1998). Emergence - From chaos to order. New York: Oxford University Press.
[^192]: Huepe, C., Zschaler, G., Do, A.-L., & Gross, T. (2011). Adaptive-network models of swarm dynamics. New Journal of Physics, 13, 073022. http://dx.doi.org/10.1088/1367-2630/13/7/ 073022
[^193]: Ijspeert, A. J., Martinoli, A., Billard, A., & Gambardella, L. M. (2001). Collaboration through the exploitation of local interactions in autonomous collective robotics: The stick pulling experiment. Autonomous Robots, 11, 149–171. ISSN 0929-5593. https://doi.org/10.1023/A: 1011227210047.
[^194]: Ingham, A. G., Levinger, G., Graves, J., & Peckham, V. (1974). The Ringelmann effect: Studies of group size and group performance. Journal of Experimental Social Psychology, 10(4), 371–384. ISSN 0022-1031. https://doi.org/10.1016/0022-1031(74)90033-X.
[^195]: Jackson, D. E., Holcombe, M., & Ratnieks, F. L. W. (2004). Trail geometry gives polarity to ant foraging networks. Nature, 432, 907–909.
[^196]: Jaeger, J., Surkova, S., Blagov, M., Janssens, H., Kosman, D., Kozlov, K. N., et al. (2004). Dynamic control of positional information in the early Drosophila embryo. Nature, 430, 368– 371. http://dx.doi.org/10.1038/nature02678
[^197]: Jansson, F., Hartley, M., Hinsch, M., Slavkov, I., Carranza, N., Olsson, T. S. G., et al. (2015). Kilombo: A Kilobot simulator to enable effective research in swarm robotics. Preprint arXiv:1511.04285.
[^198]: Jasmine. (2017). Swarm robot - project website. http://www.swarmrobot.org/ 
[^199]: Jeanne, R. L., & Nordheim, E. V. (1996). Productivity in a social wasp: Per capita output increases with swarm size. Behavioral Ecology, 7(1), 43–48.
[^200]: Jeanson, R., Rivault, C., Deneubourg, J.-L., Blanco, S., Fournier, R., Jost, C., et al. (2005). Self-organized aggregation in cockroaches. Animal Behavior, 69, 169–180.
[^201]: Johnson, S. (2001). Emergence: The connected lives of ants, brains, cities, and software. New York: Scribner.
[^202]: Jones, J. (2010). The emergence and dynamical evolution of complex transport networks from simple low-level behaviours. International Journal of Unconventional Computing, 6(2), 125– 144.
[^203]: Jones, J. (2010). Characteristics of pattern formation and evolution in approximations of physarum transport networks. Artificial Life, 16(2), 127–153.
[^204]: Jones, J. L., & Roth, D. (2004). Robot programming: A practical guide to behavior-based robotics. New York: McGraw Hill.
[^205]: Kac, M. (1947). Random walk and the theory of Brownian motion. The American Mathematical Monthly, 54, 369.
[^206]: Kalthoff, K. (1978). Pattern formation in early insect embryogenesis - data calling for modification of a recent model. Journal of Cell Science, 29(1), 1–15.
[^207]: Kanakia, A. P. (2015). Response Threshold Based Task Allocation in Multi-Agent Systems Performing Concurrent Benefit Tasks with Limited Information. PhD thesis, University of Colorado Boulder.
[^208]: Kapellmann-Zafra, G., Salomons, N., Kolling, A., & Groß, R. (2016). Human-robot swarm interaction with limited situational awareness. In M. Dorigo (Ed.), International Conference on Swarm Intelligence (ANTS 2016). Lecture notes in computer science (pp. 125–136). Berlin: Springer.
[^209]: Karsai, I., & Schmickl, T. (2011). Regulation of task partitioning by a “common stomach”: A model of nest construction in social wasps. Behavioral Ecology, 22, 819–830. https://doi.org/ 10.1093/beheco/arr060
[^210]: Kengyel, D., Hamann, H., Zahadat, P., Radspieler, G., Wotawa, F., & Schmickl, T. (2015). Potential of heterogeneity in collective behaviors: A case study on heterogeneous swarms. In Q. Chen, P. Torroni, S. Villata, J. Hsu, & A. Omicini (Eds.), PRIMA 2015: Principles and practice of multi-agent systems. Lecture notes in computer science (Vol. 9387, pp. 201–217). Berlin: Springer.
[^211]: Kennedy, J., & Eberhart, R. C. (2001). Swarm intelligence. Los Altos, CA: Morgan Kaufmann.
[^212]: Kernbach, S., Häbe, D., Kernbach, O., Thenius, R., Radspieler, G., Kimura, T., et al. (2013). Adaptive collective decision-making in limited robot swarms without communication. The International Journal of Robotics Research, 32(1), 35–55.
[^213]: Kernbach, S., Thenius, R., Kernbach, O., & Schmickl, T. (2009). Re-embodiment of honeybee aggregation behavior in an artificial micro-robotic swarm. Adaptive Behavior, 17, 237–259.
[^214]: Kessler, M. A., & Werner, B. T. (2003). Self-organization of sorted patterned ground. Science, 299, 380–383.
[^215]: Kettler, A., Szymanski, M., & Wörn, H. (2012). The Wanda robot and its development system for swarm algorithms. Advances in autonomous mini robots (pp. 133–146). Berlin: Springer.
[^216]: Khaluf, Y., Birattari, M., & Hamann, H. (2014). A swarm robotics approach to task allocation under soft deadlines and negligible switching costs. In A. P. del Pobil, E. Chinellato, E. Martinez-Martin, J. Hallam, E. Cervera, & A. Morales (Eds.), Simulation of adaptive behavior (SAB 2014). Lecture notes in computer science (Vol. 8575, pp. 270–279). Berlin: Springer.
[^217]: Khaluf, Y., Birattari, M., & Rammig, F. (2013). Probabilistic analysis of long-term swarm performance under spatial interferences. In A.-H. Dediu, C. Martín-Vide, B. Truthe, & M. A. Vega-Rodríguez (Eds.), Proceedings of Theory and Practice of Natural Computing (pp. 121– 132). Berlin/Heidelberg: Springer. ISBN 978-3-642-45008-2. http://dx.doi.org/10.1007/978- 3-642-45008-2_10
[^218]: Kim, L. H., & Follmer, S. (2017). UbiSwarm: Ubiquitous robotic interfaces and investigation of abstract motion as a display. The Proceedings of the ACM on Interactive, Mobile, Wearable and Ubiquitous Technologies, 1(3), 66:1–66:20. ISSN 2474-9567. http://doi.acm.org/10. 1145/3130931
[^219]: Klein, J. (2003). Continuous 3D agent-based simulations in the breve simulation environment. In Proceedings of NAACSOS Conference (North American Association for Computational, Social, and Organizational Sciences).
[^220]: Kolling, A., Walker, P., Chakraborty, N., Sycara, K., & Lewis, M. (2016). Human interaction with robot swarms: A survey. IEEE Transactions on Human-Machine Systems, 46(1), 9–26. ISSN 2168-2291. https://doi.org/10.1109/THMS.2015.2480801
[^221]: König, L., Mostaghim, S., & Schmeck, H. (2009). Decentralized evolution of robotic behavior using finite state machines. International Journal of Intelligent Computing and Cybernetics, 2(4), 695–723.
[^222]: Krapivsky, P. L., Redner, S., & Ben-Naim, E. (2010). A kinetic view of statistical physics. Cambridge: Cambridge University Press.
[^223]: Kube, C. R., & Bonabeau, E. (2000). Cooperative transport by ants and robots. Robotics and Autonomous Systems, 30, 85–101.
[^224]: Kubík, A. (2001). On emergence in evolutionary multiagent systems. In Proceedings of the 6th European Conference on Artificial Life (pp. 326–337).
[^225]: Kubík, A. (2003). Toward a formalization of emergence. Artificial Life, 9, 41–65.
[^226]: Kuramoto, Y. (1984). Chemical oscillations, waves, and turbulence. Berlin: Springer.
[^227]: Kwiatkowska, M., Norman, G., & Parker, D. (2011). PRISM 4.0: Verification of probabilistic real-time systems. In G. Gopalakrishnan & S. Qadeer (Eds.), Proceedings of 23rd International Conference on Computer Aided Verification (CAV’11). Lecture notes in computer science (Vol. 6806, pp. 585–591). Berlin: Springer.
[^228]: Lazer, D., & Friedman, A. (2007). The network structure of exploration and exploitation. Administrative Science Quarterly, 52, 667–694.
[^229]: Le Goc, M., Kim, L. H., Parsaei, A., Fekete, J.-D., Dragicevic, P., & Follmer, S. (2016). Zooids: Building blocks for swarm user interfaces. In Proceedings of the 29th Annual Symposium on User Interface Software and Technology (pp. 97–109). New York: ACM.
[^230]: Lee, R. E. Jr. (1980). Aggregation of lady beetles on the shores of lakes (coleoptera: Coccinellidae). American Midland Naturalist, 104(2), 295–304.
[^231]: Lehman, J., & Stanley, K. O. (2008). Exploiting open-endedness to solve problems through the search for novelty. In S. Bullock, J. Noble, R. Watson, & M. A. Bedau (Eds.), Artificial Life XI: Proceedings of the Eleventh International Conference on the Simulation and Synthesis of Living Systems (pp. 329–336). Cambridge, MA: MIT Press.
[^232]: Lehman, J., & Stanley, K. O. (2011). Improving evolvability through novelty search and self-adaptation. In Proceedings of the 2011 IEEE Congress on Evolutionary Computation (CEC’11) (pp. 2693–2700). New York: IEEE.
[^233]: Lemaire, T., Alami, R., & Lacroix, S. (2004). A distributed tasks allocation scheme in multi-UAV context. In Proceedings of the IEEE International Conference on Robotics and Automation (ICRA’04) (Vol. 4, pp. 3622–3627). New York: IEEE Press. https://doi.org/10. 1109/ROBOT.2004.1308816
[^234]: Lenaghan, S. C., Wang, Y., Xi, N., Fukuda, T., Tarn, T., Hamel, W. R., et al. (2013). Grand challenges in bioengineered nanorobotics for cancer therapy. IEEE Transactions on Biomedical Engineering, 60(3), 667–673.
[^235]: Leoncini, I., & Rivault, C. (2005). Could species segregation be a consequence of aggregation processes? Example of Periplaneta americana (L.) and P. fuliginosa (serville). Ethology, 111(5), 527–540.
[^236]: Lerman, K. (2004). A model of adaptation in collaborative multi-agent systems. Adaptive Behavior, 12(3–4), 187–198.
[^237]: Lerman, K., & Galstyan, A. (2002). Mathematical model of foraging in a group of robots: Effect of interference. Autonomous Robots, 13, 127–141.
[^238]: Lerman, K., Galstyan, A., Martinoli, A., & Ijspeert, A. (2001). A macroscopic analytical model of collaboration in distributed robotic systems. Artificial Life, 7, 375–393.
[^239]: Lerman, K., Jones, C., Galstyan, A., & Mataric, M. J. (2006). Analysis of dynamic task ´ allocation in multi-robot systems. International Journal of Robotics Research, 25(3), 225– 241.
[^240]: Lerman, K., Martinoli, A., & Galstyan, A. (2005). A review of probabilistic macroscopic models for swarm robotic systems. In E. ¸ Sahin & W. M. Spears (Eds.), Swarm Robotics - SAB 2004 International Workshop. Lecture notes in computer science (Vol. 3342, pp. 143– 152). Berlin: Springer.
[^241]: Levi, P., & Kernbach, S. (Eds.). (2010). Symbiotic multi-robot organisms: Reliability, adaptability, evolution. Berlin: Springer.
[^242]: Lind, J. (2000). Issues in agent-oriented software engineering. In M. Wooldridge (Ed.), Agentoriented software engineering. Berlin/Heidelberg/New York: Springer.
[^243]: Ludwig, L., & Gini, M. (2006). Robotic swarm dispersion using wireless intensity signals. In Distributed autonomous robotic systems 7 (pp. 135–144). Berlin: Springer.
[^244]: Luke, S., Cioffi-Revilla, C., Panait, L., & Sullivan, K. (2004). Mason: A new multi-agent simulation toolkit. In Proceedings of the 2004 Swarmfest Workshop (Vol. 8, p. 44).
[^245]: Mack, C. A. (2011). Fifty years of Moore’s law. IEEE Transactions on Semiconductor Manufacturing, 24(2), 202–207.
[^246]: Madhavan, R., Fregene, K., & Parker, L. E. (2004). Terrain aided distributed heterogeneous multirobot localization and mapping. Autonomous Robots, 17, 23–39.
[^247]: Mahmoud, H. (2008). Pólya urn models. Boca Raton, FL: Chapman and Hall/CRC.
[^248]: Mallon, E. B., & Franks, N. R. (2000). Ants estimate area using Buffon’s needle. Proceedings of the Royal Society of London B, 267(1445), 765–770.
[^249]: Mandelbrot, B. B., & Pignoni, R. (1983). The fractal geometry of nature (Vol. 173). New York: WH Freeman.
[^250]: Mariano, P., Salem, Z., Mills, R., Zahadat, P., Correia, L., & Schmickl, T. (2017). Design choices for adapting bio-hybrid systems with evolutionary computation. In Proceedings of the Genetic and Evolutionary Computation Conference Companion, GECCO ’17, New York, NY, USA (pp. 211–212). New York: ACM. ISBN 978-1-4503-4939-0. http://doi.acm.org/10. 1145/3067695.3076044
[^251]: Marsili, M., & Zhang, Y.-C. (1998). Interacting individuals leading to Zipf’s law. Physical Review Letters, 80(12), 2741.
[^252]: Martinoli, A. (1999). Swarm Intelligence in Autonomous Collective Robotics: From Tools to the Analysis and Synthesis of Distributed Control Strategies. PhD thesis, Ecole Polytechnique Fédérale de Lausanne.
[^253]: Martinoli, A., Easton, K., & Agassounon, W. (2004). Modeling swarm robotic systems: A case study in collaborative distributed manipulation. International Journal of Robotics Research, 23(4), 415–436.
[^254]: Mataric, M. J. (1992). Integration of representation into goal-driven behavior-based robots. ´ IEEE Transactions on Robotics and Automation, 8(3), 304–312.
[^255]: Mataric, M. J. (1992). Minimizing complexity in controlling a mobile robot population. In ´ IEEE International Conference on Robotics and Automation (pp. 830–835). New York: IEEE.
[^256]: Mataric, M. J. (1993). Designing emergent behaviors: From local interactions to collective ´ intelligence. Proceedings of the Second International Conference on From Animals to Animats 2: Simulation of Adaptive Behavior (pp. 432–441).
[^257]: Mataric, M. J. (1995). Designing and understanding adaptive group behavior. ´ Adaptive Behavior, 4, 51–80.
[^258]: Mataric, M. J. (1997). Reinforcement learning in the multi-robot domain. ´ Autonomous Robots, 4(1), 73–83.
[^259]: Mathews, N., Christensen, A. L., Ferrante, E., O’Grady, R., & Dorigo, M. (2010). Establishing spatially targeted communication in a heterogeneous robot swarm. In Proceedings of the 9th International Conference on Autonomous Agents and Multiagent Systems (AAMAS) (pp. 939– 946). International Foundation for Autonomous Agents and Multiagent Systems.
[^260]: Mayet, R., Roberz, J., Schmickl, T., & Crailsheim, K. (2010). Antbots: A feasible visual emulation of pheromone trails for swarm robots. In M. Dorigo, M. Birattari, G. A. Di Caro, R. Doursat, A. P. Engelbrecht, D. Floreano, L. M. Gambardella, R. Groß, E. ¸ Sahin, H. Sayama, & T. Stützle (Eds.), Swarm Intelligence: 7th International Conference, ANTS 2010. Lecture notes in computer science (Vol. 6234, pp. 84–94). Berlin/Heidelberg/New York: Springer. ISBN 978-3-642-15460-7. https://doi.org/10.1007/978-3-642-15461-4
[^261]: McEvoy, M. A., & Correll, N. (2015). Materials that couple sensing, actuation, computation, and communication. Science, 347(6228), 1261689 . ISSN 0036-8075. https://doi.org/10.1126/ science.1261689.
[^262]: McLurkin, J., Lynch, A. J., Rixner, S., Barr, T. W., Chou, A., Foster, K., et al. (2013). A low-cost multi-robot system for research, teaching, and outreach. In Distributed autonomous robotic systems (pp. 597–609). Berlin: Springer.
[^263]: McLurkin, J., & Smith, J. (2004). Distributed algorithms for dispersion in indoor environments using a swarm of autonomous mobile robots. In Distributed Autonomous Robotic Systems Conference.
[^264]: McLurkin, J., Smith, J., Frankel, J., Sotkowitz, D., Blau, D., & Schmidt, B. (2006). Speaking swarmish: Human-robot interface design for large swarms of autonomous mobile robots. In AAAI Spring Symposium: To Boldly Go Where No Human-Robot Team has Gone Before (pp. 72–75).
[^265]: Meinhardt, H. (1982). Models of biological pattern formation. New York: Academic.
[^266]: Meinhardt, H. (2003). The algorithmic beauty of sea shells. Berlin: Springer.
[^267]: Meinhardt, H., & Gierer, A. (1974). Applications of a theory of biological pattern formation based on lateral inhibition. Journal of Cell Science, 15(2), 321–346. ISSN 0021-9533. http:// view.ncbi.nlm.nih.gov/pubmed/4859215
[^268]: Meinhardt, H., & Gierer, A. (2000). Pattern formation by local self-activation and lateral inhibition. Bioessays, 22, 753–760.
[^269]: Meinhardt, H., & Klingler, M. (1987). A model for pattern formation on the shells of molluscs. Journal of Theoretical Biology, 126, 63–69.
[^270]: Meister, T., Thenius, R., Kengyel, D., & Schmickl, T. (2013). Cooperation of two different swarms controlled by BEECLUST algorithm. In Mathematical models for the living systems and life sciences (ECAL) (pp. 1124–1125).
[^271]: Melhuish, C., Wilson, M., & Sendova-Franks, A. (2001). Patch sorting: Multi-object clustering using minimalist robots. In J. Kelemen & P. Sosík (Eds.), Advances in Artificial Life: 6th European Conference, ECAL 2001 Prague, Czech Republic, September 10–14, 2001 Proceedings (pp. 543–552). Berlin/Heidelberg: Springer. ISBN 978-3-540-44811-2. https:// doi.org/10.1007/3-540-44811-X_62
[^272]: Mellinger, D., Shomin, M., Michael, N., & Kumar, V. (2013). Cooperative grasping and transport using multiple quadrotors. In Distributed autonomous robotic systems (pp. 545– 558). Berlin: Springer.
[^273]: Merkle, D., Middendorf, M., & Scheidler, A. (2007). Swarm controlled emergence-designing an anti-clustering ant system. In IEEE Swarm Intelligence Symposium (pp. 242–249). New York: IEEE.
[^274]: Merkle, D., Middendorf, M., & Scheidler, A. (2008). Organic computing and swarm intelligence. In C. Blum & D. Merkle (Eds.), Swarm intelligence: Introduction and applications. Berlin: Springer.
[^275]: Meyer, B., Beekman, M., & Dussutour, A. (2008). Noise-induced adaptive decision-making in ant-foraging. In Simulation of adaptive behavior (SAB). Lecture notes in computer science (Vol. 5040, pp. 415–425). Berlin: Springer.
[^276]: Meyer, B., Renner, C., & Maehle, E. (2016). Versatile sensor and communication expansion set for the autonomous underwater vehicle MONSUN. In Advances in Cooperative Robotics: Proceedings of the 19th International Conference on CLAWAR 2016 (pp. 250–257). Singapore: World Scientific.
[^277]: Mill, J. S. (1843). A system of logic: Ratiocinative and inductive. London: John W. Parker and Son.
[^278]: Milutinovic, D., & Lima, P. (2006). Modeling and optimal centralized control of a large-size robotic population. IEEE Transactions on Robotics, 22(6), 1280–1285.
[^279]: Milutinovic, D., & Lima, P. (2007). Cells and robots: Modeling and control of large-size agent populations. Berlin: Springer.
[^280]: Misner, C. W., Thorne, K. S., & Wheeler, J. A. (2017). Gravitation. Princeton, NJ: Princeton University Press.
[^281]: Möbius, M. E., Lauderdale, B. E., Nagel, S. R., & Jaeger, H. M. (2001). Brazil-nut effect: Size separation of granular particles. Nature, 414(6861), 270.
[^282]: Moeslinger, C., Schmickl, T., & Crailsheim, K. (2010). Emergent flocking with low-end swarm robots. In M. Dorigo, M. Birattari, G. Di Caro, R. Doursat, A. Engelbrecht, D. Floreano, L. Gambardella, R. Groß, E. Sahin, H. Sayama, & T. Stützle (Eds.), Swarm intelligence. Lecture notes in computer science (Vol. 6234, pp. 424–431). Berlin/Heidelberg: Springer.
[^283]: Moeslinger, C., Schmickl, T., & Crailsheim, K. (2011). A minimalist flocking algorithm for swarm robots. In Advances in Artificial Life: Darwin Meets von Neumann (ECAL’09). Lecture notes in computer science (Vol. 5778, pp. 357–382). Heidelberg/Berlin: Springer.
[^284]: Moioli, R., Vargas, P. A., & Husbands, P. (2010). Exploring the Kuramoto model of coupled oscillators in minimally cognitive evolutionary robotics tasks. In WCCI 2010 IEEE World Congress on Computational Intelligence - CEC IEEE (pp. 2483–2490).
[^285]: Monajjemi, V. M., Wawerla, J., Vaughan, R., & Mori, G. (2013). HRI in the sky: Creating and commanding teams of UAVs with a vision-mediated gestural interface. In IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), November 2013 (pp. 617–623). https://doi.org/10.1109/IROS.2013.6696415
[^286]: Mondada, F., Bonani, M., Raemy, X., Pugh, J., Cianci, C., Klaptocz, A., et al. (2009). The e-puck, a robot designed for education in engineering. In Proceedings of the 9th Conference on Autonomous Robot Systems and Competitions (Vol. 1, pp. 59–65).
[^287]: Mondada, F., Franzi, E., & Ienne, P. (1994). Mobile robot miniaturisation: A tool for investigation in control algorithms. In Proceedings of the 3rd International Symposium on Experimental Robotics (pp. 501–513). Berlin: Springer.
[^288]: Mondada, L., Karim, M. E., & Mondada, F. (2016). Electroencephalography as implicit communication channel for proximal interaction between humans and robot swarms. Swarm Intelligence, 10(4), 247–265. ISSN 1935-3820. https://doi.org/10.1007/s11721-016-0127-0
[^289]: Mondada, F., Pettinaro, G. C., Guignard, A., Kwee, I., Floreano, D., Deneubourg, J.-L., et al. (2004). SWARM-BOT: A new distributed robotic concept. Autonomous Robots, Special Issue on Swarm Robotics, 17(2–3), 193–221. doi: NA.
[^290]: Murray, J. D. (1981). A prepattern formation mechanism for animal coat markings. Journal of Theoretical Biology, 88, 161–199.
[^291]: Murray, J. D. (2003). On the mechanochemical theory of biological pattern formation with application to vasculogenesis. Comptes Rendus Biologies, 326(2), 239–252.
[^292]: Nagi, J., Giusti, A., Gambardella, L. M., & Di Caro, G. A. (2014). Human-swarm interaction using spatial gestures. In 2014 IEEE/RSJ International Conference on Intelligent Robots and Systems, September 2014 (pp. 3834–3841). https://doi.org/10.1109/IROS.2014.6943101
[^293]: Nair, R., Ito, T., Tambe, M., & Marsella, S. (2002). Task allocation in the RoboCup rescue simulation domain: A short note. In A. Birk, S. Coradeschi, & S. Tadokoro (Eds.), RoboCup 2001: Robot Soccer World Cup V (Vol. 2377, pp. 1–22). Berlin/Heidelberg: Springer. http:// dx.doi.org/10.1007/3-540-45603-1_129
[^294]: Nolfi, S., Bongard, J., Husbands, P., & Floreano, D. (2016). Evolutionary robotics. In Springer handbook of robotics (pp. 2035–2068). Berlin: Springer.
[^295]: Nouyan, S., Campo, A., & Dorigo, M. (2008). Path formation in a robot swarm: Selforganized strategies to find your way home. Swarm Intelligence, 2(1), 1–23.
[^296]: Nouyan, S., Groß, R., Bonani, M., Mondada, F., & Dorigo, M. (2009). Teamwork in selforganized robot colonies. IEEE Transactions on Evolutionary Computation, 13(4), 695–711.
[^297]: O’Grady, R., Groß, R., Mondada, F., Bonani, M., & Dorigo, M. (2005). Self-assembly on demand in a group of physical autonomous mobile robots navigating rough terrain. In Advances in Artificial Life, 8th European Conference (ECAL) (pp. 272–281). Berlin: Springer.
[^298]: O’Keeffe, K. P., Hong, H., & Strogatz, S. H. (2017). Oscillators that sync and swarm. Nature Communications, 8, 1504.
[^299]: Olfati-Saber, R., Fax, A., & Murray, R. M. (2007). Consensus and cooperation in networked multi-agent systems. Proceedings of the IEEE, 95(1), 215–233.
[^300]: Østergaard, E. H., Sukhatme, G. S., & Mataric, M. J. (2001). Emergent bucket brigading: ´ A simple mechanisms for improving performance in multi-robot constrained-space foraging tasks. In E. André, S. Sen, C. Frasson, & J. P. Müller (Eds.), Proceedings of the Fifth International Conference on Autonomous Agents (AGENTS’01), New York, NY, USA (pp. 29–35). New York: ACM. ISBN 1-58113-326-X. http://doi.acm.org/10.1145/375735.375825
[^301]: Osterloh, C., Pionteck, T., & Maehle, E. (2012). MONSUN II: A small and inexpensive AUV for underwater swarms. In ROBOTIK 2012 - 7th German Conference on Robotics.
[^302]: Ostwald, W. (1897). Studien über die Bildung und Umwandlung fester Körper. Zeitschrift für physikalische Chemie, 22(1), 289–330.
[^303]: Parrish, J. K., & Edelstein-Keshet, L. (1999). Complexity, pattern, and evolutionary tradeoffs in animal aggregation. Science, 284(5411), 99–101. ISSN 0036-8075. https://doi.org/10. 1126/science.284.5411.99
[^304]: Parunak, H. V. D., & Brueckner, S. A. (2004). Engineering swarming systems. In Methodologies and software engineering for agent systems (pp. 341–376). Berlin: Springer.
[^305]: Payton, D., Daily, M., Estowski, R., Howard, M., & Lee, C. (2001). Pheromone robotics. Autonomous Robots, 11(3), 319–324.
[^306]: Peires, F. T. (1926). Tensile tests for cotton yarns. Journal of the Textile Institute, 17, 355–368.
[^307]: Petersen, K., Nagpal, R., & Werfel, J. (2011). TERMES: An autonomous robotic system for three-dimensional collective construction. Proceedings Robotics: Science & Systems VII (pp. 257–264).
[^308]: Pickem, D., Lee, M., & Egerstedt, M. (2015). The GRITSBot in its natural habitat: A multirobot testbed. In IEEE International Conference on Robotics and Automation (ICRA) (pp. 4062–4067). New York: IEEE.
[^309]: Pinciroli, C., Trianni, V., O’Grady, R., Pini, G., Brutschy, A., Brambilla, M., et al. (2012). ARGoS: A modular, parallel, multi-engine simulator for multi-robot systems. Swarm Intelligence, 6(4), 271–295. ISSN 1935-3812. http://dx.doi.org/10.1007/s11721-012-0072-5
[^310]: Pini, G., Brutschy, A., Francesca, G., Dorigo, M., & Birattari, M. (2012). Multi-armed bandit formulation of the task partitioning problem in swarm robotics. In 8th International Conference on Swarm Intelligence (ANTS) (pp. 109–120). Berlin: Springer.
[^311]: Planck, M. (1917). Über einen Satz der statistischen Dynamik und seine Erweiterung in der Quantentheorie. Sitzungsberichte der Preußischen Akademie der Wissenschaften, 24, 324– 341.
[^312]: Podevijn, G., O’Grady, R., Mathews, N., Gilles, A., Fantini-Hauwel, C., & Dorigo, M. (2016). Investigating the effect of increasing robot group sizes on the human psychophysiological state in the context of human–swarm interaction. Swarm Intelligence, 10(3), 1–18. ISSN 1935-3820. http://dx.doi.org/10.1007/s11721-016-0124-3
[^313]: Pólya, G., & Eggenberger, F. (1923). Über die Statistik verketteter Vorgänge. Zeitschrift für Angewandte Mathematik und Mechanik, 3(4), 279–289.
[^314]: Popkin, G. (2016). The physics of life. Nature, 529, 16–18. https://doi.org/10.1038/529016a 314. Potter, M. A., Meeden, L. A., & Schultz, A. C. (2001). Heterogeneity in the coevolved behaviors of mobile robots: The emergence of specialists. In International Joint Conference on Artificial Intelligence (IJCAI) (pp. 1337–1343). Los Altos, CA: Morgan Kaufmann.
[^315]: Pourmehr, S., Monajjemi, V. M., Vaughan, R., & Mori, G. (2013). “you two! Take off!”: Creating, modifying and commanding groups of robots using face engagement and indirect speech in voice commands. In IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), November 2013 (pp. 137–142). https://doi.org/10.1109/IROS.2013.6696344
[^316]: Press, W. H., Teukolsky, S. A., Vetterling, W. T., & Flannery, B. P. (2002). Numerical recipes in C++. Cambridge: Cambridge University Press.
[^317]: Prigogine, I. (1997). The end of certainty: Time, chaos, and the new laws of nature. New York: Free Press.
[^318]: Prorok, A., Ani Hsieh, M., & Kumar, V. (2015). Fast redistribution of a swarm of heterogeneous robots. In International Conference on Bio-inspired Information and Communications Technologies (BICT).
[^319]: Prorok, A., Ani Hsieh, M., & Kumar, V. (2016). Formalizing the impact of diversity on performance in a heterogeneous swarm of robots. In 2016 IEEE International Conference on Robotics and Automation (ICRA) (pp. 5364–5371). https://doi.org/10.1109/ICRA.2016. 7487748
[^320]: Prorok, A., Correll, N., & Martinoli, A. (2011). Multi-level spatial models for swarm-robotic systems. The International Journal of Robotics Research, 30(5), 574–589.
[^321]: Raischel, F., Kun, F., & Herrmann, H. J. (2006). Fiber bundle models for composite materials. In Conference on Damage in Composite Materials.
[^322]: Ramaley, J. F. (1969). Buffon’s noodle problem. The American Mathematical Monthly, 76(8), 916–918. http://www.jstor.org/stable/2317945
[^323]: Ratnieks, F. L. W., & Anderson, C. (1999). Task partitioning in insect societies. Insectes Sociaux, 46(2), 95–108. https://doi.org/10.1007/s000400050119
[^324]: Reina, A., Dorigo, M., & Trianni, V. (2014). Towards a cognitive design pattern for collective decision-making. In M. Dorigo, M. Birattari, S. Garnier, H. Hamann, M. M. de Oca, C. Solnon, & T. Stützle (Eds.), Swarm intelligence. Lecture notes in computer science (Vol. 8667, pp. 194–205). Berlin: Springer International Publishing. ISBN 978-3-319-09951-4. http://dx. doi.org/10.1007/97 -3-319-09952-1_17
[^325]: Reina, A., Valentini, G., Fernández-Oto, C., Dorigo, M., & Trianni, V. (2015). A design pattern for decentralised decision making. PLoS One, 10(10), 1–18. https://doi.org/10.1371/ journal.pone.0140950
[^326]: Resnick, M. (1994). Turtles, termites, and traffic jams. Cambridge, MA: MIT Press.
[^327]: Reynolds, C. W. (1987). Flocks, herds, and schools. Computer Graphics, 21(4), 25–34.
[^328]: Riedo, F., Chevalier, M., Magnenat, S., & Mondada, F. (2013). Thymio II, a robot that grows wiser with children. In IEEE Workshop on Advanced Robotics and Its Social Impacts (ARSO 2013) (pp. 187–193). New York: IEEE.
[^329]: Risken, H. (1984). The Fokker-Planck equation. Berlin: Springer.
[^330]: Rosenblueth, A., Wiener, N., & Bigelow, J. (1943). Behavior, purpose and teleology. Philosophy of Science, 10(1), 18–24. ISSN 00318248, 1539767X. http://www.jstor.org/stable/ 184878
[^331]: Rubenstein, M., Ahler, C., & Nagpal, R. (2012). Kilobot: A low cost scalable robot system for collective behaviors. In IEEE International Conference on Robotics and Automation (ICRA 2012) (pp. 3293–3298). https://doi.org/10.1109/ICRA.2012.6224638
[^332]: Rubenstein, M., Cornejo, A., & Nagpal, R. (2014). Programmable self-assembly in a thousand-robot swarm. Science, 345(6198), 795–799. http://dx.doi.org/10.1126/science. 1254295
[^333]: Rubenstein, M., & Shen, W.-M. (2009). Scalable self-assembly and self-repair in a collective of robots. In Proceedings of the IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), St. Louis, MO, USA, October 2009.
[^334]: Rumelhart, D. E., Hinton, G. E., & Williams, R. J. (1986). Learning representations by backpropagating errors. Nature, 323, 533–536.
[^335]: Russell, B. (1923). Vagueness. Australasian Journal of Psychology and Philosophy, 1(2), 84– 92. http://dx.doi.org/10.1080/00048402308540623
[^336]: Russell, R. A. (1997). Heat trails as short-lived navigational markers for mobile robots. In Proceedings of the IEEE International Conference on Robotics and Automation (Vol. 4, pp. 3534–3539).
[^337]: Russell, S. J., & Norvig, P. (1995). Artificial intelligence: A modern approach. Englewood, Cliffs, NJ: Prentice Hall.
[^338]: ¸ Sahin, E. (2005). Swarm robotics: From sources of inspiration to domains of application. In E. Sahin & W. M. Spears (Eds.), ¸ Swarm Robotics - SAB 2004 International Workshop. Lecture notes in computer science (Vol. 3342, pp. 10–20). Berlin: Springer.
[^339]: Savkin, A. V. (2004). Coordinated collective motion of groups of autonomous mobile robots: Analysis of Vicsek’s model. IEEE Transactions on Automatic Control, 49(6), 981–982.
[^340]: Scheidler, A. (2011). Dynamics of majority rule with differential latencies. Physical Review E, 83(3), 031116.
[^341]: Scheidler, A., Merkle, D., & Middendorf, M. (2013). Swarm controlled emergence for ant clustering. International Journal of Intelligent Computing and Cybernetics, 6(1), 62–82.
[^342]: Schmelzer, J. W. P. (2006). Nucleation theory and applications. New York: Wiley.
[^343]: Schmickl, T., & Crailsheim, K. (2004). Costs of environmental fluctuations and benefits of dynamic decentralized foraging decisions in honey bees. Adaptive Behavior - Animals, Animats, Software Agents, Robots, Adaptive Systems, 12, 263–277.
[^344]: Schmickl, T., & Hamann, H. (2011). BEECLUST: A swarm algorithm derived from honeybees. In Y. Xiao (Ed.), Bio-inspired computing and communication networks (pp. 95– 137). Boca Raton, FL: CRC Press.
[^345]: Schmickl, T., Hamann, H., & Crailsheim, K. (2011). Modelling a hormone-inspired controller for individual- and multi-modular robotic systems. Mathematical and Computer Modelling of Dynamical Systems, 17(3), 221–242.
[^346]: Schmickl, T., Hamann, H., Wörn, H., & Crailsheim, K. (2009). Two different approaches to a macroscopic model of a bio-inspired robotic swarm. Robotics and Autonomous Systems, 57(9), 913–921. http://dx.doi.org/10.1016/j.robot.2009.06.002
[^347]: Schmickl, T., Möslinger, C., & Crailsheim, K. (2007). Collective perception in a robot swarm. In E. ¸ Sahin, W. M. Spears, & A. F. T. Winfield (Eds.), Swarm Robotics - Second SAB 2006 International Workshop. Lecture notes in computer science (Vol. 4433). Heidelberg/Berlin: Springer.
[^348]: Schmickl, T., Stradner, J., Hamann, H., Winkler, L., & Crailsheim, K. (2011). Major feedback loops supporting artificial evolution in multi-modular robotics. In S. Doncieux, N. Bredèche, & J.-B. Mouret (Eds.), New horizons in evolutionary robotics. Studies in computational intelligence (Vol. 341, pp. 195–209). Berlin/Heidelberg: Springer. ISBN 978-3-642-18271- 6. https://doi.org/10.1007/978-3-642-18272-3
[^349]: Schmickl, T., Thenius, R., Möslinger, C., Radspieler, G., Kernbach, S., & Crailsheim, K. (2008). Get in touch: Cooperative decision making based on robot-to-robot collisions. Autonomous Agents and Multi-Agent Systems, 18(1), 133–155.
[^350]: Schmickl, T., Thenius, R., Moslinger, C., Timmis, J., Tyrrell, A., Read, M., et al. (2011). Cocoro – the self-aware underwater swarm. In 2011 Fifth IEEE Conference on Self-Adaptive and Self-Organizing Systems Workshops (pp. 120–126). https://doi.org/10.1109/SASOW. 2011.11
[^351]: Schultz, A. C., Grefenstette, J. J., & Adams, W. (1996). Robo-Shepherd: Learning complex robotic behaviors. In M. Jamshidi, F. Pin, & P. Dauchez (Eds.), Proceedings of the International Symposium on Robotics and Automation (ICRA’96) (Vol. 6, pp. 763–768). New York, NY: ASME Press.
[^352]: Schumacher, R. (2002). Book review: Achim Stephan: Emergenz. Von der Unvorhersagbarkeit zur Selbstorganisation. European Journal of Philosophy, 10(3), 415–419 (Dresden/München: Dresden University Press, 1999).
[^353]: Schweitzer, F. (2002). Brownian agent models for swarm and chemotactic interaction. In D. Polani, J. Kim, & T. Martinetz (Eds.), Fifth German Workshop on Artificial Life. Abstracting and Synthesizing the Principles of Living Systems (pp. 181–190). Akademische Verlagsgesellschaft Aka.
[^354]: Schweitzer, F. (2003). Brownian agents and active particles. On the emergence of complex behavior in the natural and social sciences. Berlin: Springer.
[^355]: Schweitzer, F., Lao, K., & Family, F. (1997). Active random walkers simulate trunk trail formation by ants. BioSystems, 41, 153–166.
[^356]: Schweitzer, F., & Schimansky-Geier, L. (1994). Clustering of active walkers in a twocomponent system. Physica A, 206, 359–379.
[^357]: Sempo, G., Depickère, S., Amé, J.-M., Detrain, C., Halloy, J., & Deneubourg, J.-L. (2006). Integration of an autonomous artificial agent in an insect society: Experimental validation. In S. Nolfi, G. Baldassarre, R. Calabretta, J. C. T. Hallam, D. Marocco, J.-A. Meyer, O. Miglino, & D. Parisi (Eds.), From Animals to Animats 9: 9th International Conference on Simulation of Adaptive Behavior, SAB 2006, Rome, Italy, September 25–29, 2006. Proceedings (pp. 703–712). Berlin/Heidelberg: Springer. ISBN 978-3-540-38615-5. https://doi.org/10.1007/ 11840541_58
[^358]: Seyfried, J., Szymanski, M., Bender, N., Estaña, R., Thiel, M., & Wörn, H. (2005). The ISWARM project: Intelligent small world autonomous robots for micro-manipulation. In E. Sahin & W. M. Spears (Eds.), ¸ Swarm Robotics Workshop: State-of-the-Art Survey (pp. 70– 83). Berlin: Springer.
[^359]: Sharkey, A. J. C. (2007). Swarm robotics and minimalism. Connection Science, 19(3), 245– 260.
[^360]: Sood, V., & Redner, S. (2005). Voter model on heterogeneous graphs. Physical Review Letters, 94(17), 178701.
[^361]: Soysal, O., & ¸ Sahin, E. (2007). A macroscopic model for self-organized aggregation in swarm robotic systems. In E. ¸ Sahin, W. M. Spears, & A. F. T. Winfield (Eds.), Swarm Robotics - Second SAB 2006 International Workshop. Lecture notes in computer science (Vol. 4433, pp. 27–42). Berlin: Springer.
[^362]: Springel, V., White, S. D. M., Jenkins, A., Frenk, C. S., Yoshida, N., Gao, L., et al. (2005). Simulations of the formation, evolution and clustering of galaxies and quasars. Nature, 435, 629–636. http://www.nature.com/nature/journal/v435/n7042/full/nature03597.html 
[^363]: Stephan, A. (1999). Emergenz: Von der Unvorhersagbarkeit zur Selbstorganisation. Dresden, Munich: Dresden University Press.
[^364]: Stepney, S., Polack, F., & Turner, H. (2006). Engineering emergence. In CEC 2006: 11th IEEE International Conference on Engineering of Complex Computer Systems, Stanford, CA, USA, Los Alamitos, CA, August 2006. New York: IEEE Press.
[^365]: Støy, K., & Nagpal, R. (2004). Self-repair through scale independent self-reconfiguration. In 2004 IEEE/RSJ International Conference on Intelligent Robots and Systems, 2004 (IROS 2004). Proceedings (Vol. 2, pp. 2062–2067). NewYork: IEEE.
[^366]: Strogatz, S. H. (2001). Exploring complex networks. Nature, 410(6825), 268–276. http:// www.nature.com/nature/journal/v410/n6825/abs/410268a0.html 
[^367]: Strogatz, S. H., Abrams, D. M., McRobie, A., Eckhardt, B., & Ott, E. (2005). Theoretical mechanics: Crowd synchrony on the Millennium Bridge. Nature, 438(7064), 43–44.
[^368]: Sugawara, K., Kazama, T., & Watanabe, T. (2004). Foraging behavior of interacting robots with virtual pheromone. In Proceedings of 2004 IEEE/RSJ International Conference on Intelligent Robots and Systems, Los Alamitos, CA (pp. 3074–3079). New York: IEEE Press.
[^369]: Sugawara, K., & Sno, M. (1997). Cooperative acceleration of task performance: Foraging behavior of interacting multi-robots system. Physica D, 100, 343–354.
[^370]: Sutton, R. S., & Barto, A. G. (1998). Reinforcement learning: An introduction. Cambridge, MA: MIT Press.
[^371]: Sznajd-Weron, K., & Sznajd, J. (2000). Opinion evolution in closed community. International Journal of Modern Physics C, 11(06), 1157–1165.
[^372]: Szopek, M., Schmickl, T., Thenius, R., Radspieler, G., & Crailsheim, K. (2013). Dynamics of collective decision making of honeybees in complex temperature fields. PLoS One, 8(10), e76250. https://doi.org/10.1371/journal.pone.0076250. http://dx.doi.org/10.1371%2Fjournal. pone.0076250
[^373]: Tabony, J., & Job, D. (1992). Gravitational symmetry breaking in microtubular dissipative structures. Proceedings of the National Academy of Sciences of the United States of America, 89(15), 6948–6952.
[^374]: Tarapore, D., Christensen, A. L., & Timmis, J. (2017). Generic, scalable and decentralized fault detection for robot swarms. PLoS One, 12(8), 1–29. https://doi.org/10.1371/journal. pone.0182058
[^375]: Tarapore, D., Lima, P. U., Carneiro, J., & Christensen, A. L. (2015). To err is robotic, to tolerate immunological: Fault detection in multirobot systems. Bioinspiration & Biomimetics, 10(1), 016014.
[^376]: (Nagpal, R., et al.) The Kilobot Project, Self-Organizing Systems Research Group. (2013). Website. https://ssr.seas.harvard.edu/kilobots 
[^377]: Theraulaz, G., & Bonabeau, E. (1995). Coordination in distributed building. Science, 269, 686–688.
[^378]: Theraulaz, G., & Bonabeau, E. (1995). Modelling the collective building of complex architectures in social insects with lattice swarms. Journal of Theoretical Biology, 177, 381– 400.
[^379]: Theraulaz, G., Bonabeau, E., Nicolis, S. C., Solé, R. V., Fourcassié, V., Blanco, S., et al. (2002). Spatial patterns in ant colonies. Proceedings of the National Academy of Sciences of the United States of America, 99(15), 9645–9649.
[^380]: Thompson, D. W. (1917). On growth and form: The complete revised edition. Cambridge: Cambridge University Press.
[^381]: Thrun, S., Burgard, W., & Fox, D. (2005). Probabilistic robotics. Cambridge, MA: MIT Press.
[^382]: Toner, J., & Tu, Y. (1998). Flocks, herds, and schools: A quantitative theory of flocking. Physical Review E, 58(4), 4828–4858.
[^383]: Trianni, V. (2008). Evolutionary swarm robotics - Evolving self-organising behaviours in groups of autonomous robots. Studies in computational intelligence (Vol. 108). Berlin: Springer.
[^384]: Trianni, V., Groß, R., Labella, T. H., ¸ Sahin, E., & Dorigo, M. (2003). Evolving aggregation behaviors in a swarm of robots. In W. Banzhaf, J. Ziegler, T. Christaller, P. Dittrich, & J. T. Kim (Eds.), Advances in Artificial Life (ECAL 2003). Lecture notes in artificial intelligence (Vol. 2801, pp. 865–874). Berlin: Springer.
[^385]: Trianni, V., Ijsselmuiden, J., & Haken, R. (2016). The SAGA concept: Swarm robotics for agricultural applications. Technical report. http://laral.istc.cnr.it/saga/wp-content/uploads/ 2016/09/saga-dars2016.pdf 
[^386]: Trianni, V., Labella, T. H., & Dorigo, M. (2004). Evolution of direct communication for a swarm-bot performing hole avoidance. In M. Dorigo, M. Birattari, C. Blum, L. M. Gambardella, F. Mondada, & T. Stützle (Eds.), Ant colony optimization and swarm intelligence (ANTS 2004). Lecture notes in computer science (Vol. 3172, pp. 130–141). Berlin: Springer.
[^387]: Tuci, E., Groß, R., Trianni, V., Mondada, F., Bonani, M., & Dorigo, M. (2006). Cooperation through self-assembly in multi-robot systems. ACM Transactions on Autonomous and Adaptive Systems (TAAS), 1(2), 115–150.
[^388]: Turgut, A., Çelikkanat, H., Gökçe, F., & ¸ Sahin, E. (2008). Self-organized flocking in mobile robot swarms. Swarm Intelligence, 2(2), 97–120. http://dx.doi.org/10.1007/s11721- 008-0016-2
[^389]: Turing, A. M. (1952). The chemical basis of morphogenesis. Philosophical Transactions of the Royal Society of London. Series B, Biological Sciences, B237(641), 37–72.
[^390]: Twu, P., Mostofi, Y., & Egerstedt, M. (2014). A measure of heterogeneity in multi-agent systems. In American Control Conference (pp. 3972–3977).
[^391]: Tyrrell, A., Auer, G., & Bettstetter, C. (2006). Fireflies as role models for synchronization in ad hoc networks. In Proceedings of the 1st International Conference on Bio-inspired Models of Network, Information and Computing Systems. New York: ACM.
[^392]: Valentini, G. (2017). Achieving consensus in robot swarms: Design and analysis of strategies for the best-of-n problem. Berlin: Springer. ISBN 978-3-319-53608-8. https://doi.org/10. 1007/978-3-319-53609-5
[^393]: Valentini, G., Brambilla, D., Hamann, H., & Dorigo, M. (2016). Collective perception of environmental features in a robot swarm. In 10th International Conference on Swarm Intelligence, ANTS 2016. Lecture notes in computer science (Vol. 9882, pp. 65–76). Berlin: Springer.
[^394]: Valentini, G., Ferrante, E., & Dorigo, M. (2017). The best-of-n problem in robot swarms: Formalization, state of the art, and novel perspectives. Frontiers in Robotics and AI, 4, 9. ISSN 2296-9144. http://journal.frontiersin.org/article/10.3389/frobt.2017.00009
[^395]: Valentini, G., Ferrante, E., Hamann, H., & Dorigo, M. (2016). Collective decision with 100 Kilobots: Speed vs accuracy in binary discrimination problems. Journal of Autonomous Agents and Multi-Agent Systems, 30(3), 553–580. http://dx.doi.org/10.1007/s10458-015- 9323-3
[^396]: Valentini, G., Hamann, H., & Dorigo, M. (2014). Self-organized collective decision making: The weighted voter model. In A. Lomuscio, P. Scerri, A. Bazzan, & M. Huhns (Eds.), Proceedings of the 13th International Conference on Autonomous Agents and Multiagent Systems (AAMAS 2014). IFAAMAS.
[^397]: Valentini, G., Hamann, H., & Dorigo, M. (2015). Self-organized collective decisions in a robot swarm. In AAAI-15 Video Proceedings. Palo Alto, CA: AAAI Press. https://www.youtube. com/watch?v=5lz_HnOLBW4.
[^398]: Valentini, G., Hamann, H., & Dorigo, M. (2015). Efficient decision-making in a selforganizing robot swarm: On the speed versus accuracy trade-off. In R. Bordini, E. Elkind, G. Weiss, & P. Yolum (Eds.), Proceedings of the 14th International Conference on Autonomous Agents and Multiagent Systems (AAMAS 2015) (pp. 1305–1314). IFAAMAS. http://dl.acm.org/citation.cfm id=2773319.
[^399]: Vestartas, P., Heinrich, M. K., Zwierzycki, M., Leon, D. A., Cheheltan, A., La Magna, R., & Ayres, P. (2018). Design tools and workflows for braided structures. In K. De Rycke, C. Gengnagel, O. Baverel, J. Burry, C. Mueller, M. M. Nguyen, P. Rahm, & M. R. Thomsen (Eds.), Humanizing Digital Reality: Design Modelling Symposium Paris 2017 (pp. 671–681). Singapore: Springer. ISBN 978-981-10-6611-5. https://doi.org/10.1007/978-981-10-6611-5_ 55
[^400]: Vicsek, T., Czirók, A., Ben-Jacob, E., Cohen, I., & Shochet, O. (1995). Novel type of phase transition in a system of self-driven particles. Physical Review Letters, 6(75), 1226–1229.
[^401]: Vicsek, T., & Zafeiris, A. (2012). Collective motion. Physics Reports, 517(3–4), 71–140.
[^402]: Vigelius, M., Meyer, B., & Pascoe, G. (2014). Multiscale modelling and analysis of collective decision making in swarm robotics. PLoS One, 9(11), 1–19. https://doi.org/10.1371/journal. pone.0111542
[^403]: Vinkovic, D., & Kirman, A. (2006). A physical analogue of the Schelling model. ´ Proceedings of the National Academy of Sciences of the United States of America, 103(51), 19261–19265.
[^404]: von Frisch, K. (1974). Animal architecture. San Diego, CA: Harcourt.
[^405]: Voorhees, P. W. (1985). The theory of Ostwald ripening. Journal of Statistical Physics, 38(1), 231–252.
[^406]: Wahby, M., Weinhold, A., & Hamann, H. (2016). Revisiting BEECLUST: Aggregation of swarm robots with adaptiveness to different light settings. In Proceedings of the 9th EAI International Conference on Bio-inspired Information and Communications Technologies (BICT 2015), pp. 272–279. ICST. http://dx.doi.org/10.4108/eai.3-12-2015.2262877
[^407]: Watts, D. J., & Strogatz, S. H. (1998). Collective dynamics of ‘small-world’ networks. Nature, 393(6684), 440–442.
[^408]: Webb, B. (2001). Can robots make good models of biological behaviour? Behavioral and Brain Sciences, 24, 1033–1050.
[^409]: Webb, B. (2002). Robots in invertebrate neuroscience. Nature, 417, 359–363.
[^410]: Webb, B., & Scutt, T. (2000). A simple latency-dependent spiking-neuron model of cricket phonotaxis. Biological Cybernetics, 82, 247–269.
[^411]: Weinberg, S. (1995). Reductionism redux. The New York Review of Books, 42(15), 5.
[^412]: Wells, H., Wells, P. H., & Cook, P. (1990). The importance of overwinter aggregation for reproductive success of monarch butterflies (danaus plexippus l.). Journal of Theoretical Biology, 147(1), 115–131. ISSN 0022-5193. http://dx.doi.org/10.1016/S0022-5193(05)80255-3.
[^413]: Werfel, J., Petersen, K., & Nagpal, R. (2014). Designing collective behavior in a termiteinspired robot construction team. Science, 343(6172), 754–758. http://dx.doi.org/10.1126/ science.1245842
[^414]: Whiteson, S., Kohl, N., Miikkulainen, R., & Stone, P. (2003). Evolving keepaway soccer players through task decomposition. In Genetic and Evolutionary Computation-GECCO 2003 (pp. 20–212). Berlin: Springer.
[^415]: Whiteson, S., & Stone, P. (2006). Evolutionary function approximation for reinforcement learning. Journal of Machine Learning Research, 7, 877–917.
[^416]: Wilson, S., Gameros, R., Sheely, M., Lin, M., Dover, K., Gevorkyan, R., et al. (2016). Pheeno, a versatile swarm robotic research and education platform. IEEE Robotics and Automation Letters, 1(2), 884–891.
[^417]: Wilson, M., Melhuish, C., Sendova-Franks, A. B., & Scholes, S. (2004). Algorithms for building annular structures with minimalist robots inspired by brood sorting in ant colonies. Autonomous Robots, 17, 115–136.
[^418]: Wilson, S., Pavlic, T. P., Kumar, G. P., Buffin, A., Pratt, S. C., & Berman, S. (2014). Design of ant-inspired stochastic control policies for collective transport by robotic swarms. Swarm Intelligence, 8(4), 303–327.
[^419]: Winfield, A. F. T., Harper, C. J., & Nembrini, J. (2004). Towards dependable swarms and a new discipline of swarm engineering. In International Workshop on Swarm Robotics (pp. 126–142). Berlin: Springer.
[^420]: Winfield, A. F. T., Sa, J., Fernández-Gago, M.-C., Dixon, C., & Fisher, M. (2005). On formal specification of emergent behaviours in swarm robotic systems. International Journal of Advanced Robotic Systems, 2(4), 363–370. https://doi.org/10.5772/5769
[^421]: Witten, T. A. Jr, & Sander, L. M. (1981). Diffusion-limited aggregation, a kinetic critical phenomenon. Physical Review Letters, 47(19), 1400–1403. https://doi.org/10.1103/PhysRevLett. 47.1400
[^422]: Wittlinger, M., Wehner, R., & Wolf, H. (2006). The ant odometer: Stepping on stilts and stumps. Science, 312(5782), 1965–1967.
[^423]: Wolf, H. (2011). Odometry and insect navigation. Journal of Experimental Biology, 214(10), 1629–1641. ISSN 0022-0949. https://doi.org/10.1242/jeb.038570.
[^424]: Wolpert, L. (1996). One hundred years of positional information. Trends in Genetics, 12(9), 359–364. ISSN 0168-9525. http://view.ncbi.nlm.nih.gov/pubmed/8855666
[^425]: Yamaguchi, H., Arai, T., & Beni, G. (2001). A distributed control scheme for multiple robotic vehicles to make group formations. Robotics and Autonomous systems, 36(4), 125–147.
[^426]: Yamins, D. (2005). Towards a theory of “local to global” in distributed multi-agent systems. In Proceedings of the Fourth International Joint Conference on Autonomous Agents and Multiagent Systems (AAMAS’05) (pp. 183–190).
[^427]: Yamins, D. (2007). A Theory of Local-to-Global Algorithms for One-Dimensional Spatial Multi-Agent Systems. PhD thesis, Harvard University, November 2007.
[^428]: Yamins, D., & Nagpal, R. (2008). Automated global-to-local programming in 1-D spatial multi-agent systems. In L. Padgham, D. C. Parkes, J. P. Müller, & S. Parsons (Eds.), Proceedings of 7th International Conference on Autonomous Agents and Multiagent Systems (AAMAS 2008), Estoril, Portugal, May 2008.
[^429]: Yang, C. N. (1952). The spontaneous magnetization of a two-dimensional Ising model. Physical Review, 85(5), 808–816. http://dx.doi.org/10.1103/PhysRev.85.808
[^430]: Yasuda, T., & Ohkura, K. (2008). A reinforcement learning technique with an adaptive action generator for a multi-robot system. In The Tenth International Conference on Simulation of Adaptive Behavior (SAB’08). Lecture notes in artificial intelligence, July 2008 (Vol. 5040, pp. 250–259). Berlin: Springer.
[^431]: Yates, C. A., Erban, R., Escudero, C., Couzin, I. D., Buhl, J., Kevrekidis, I. G., et al. (2009). Inherent noise can facilitate coherence in collective swarm motion. Proceedings of the National Academy of Sciences of the United States of America, 106(14), 5464–5469. https:// doi.org/10.1073/pnas.0811195106. http://www.pnas.org/content/106/14/5464.abstract 
[^432]: Zahadat, P., Christensen, D. J., Katebi, S. D., & Støy, K. (2010). Sensor-coupled fractal gene regulatory networks for locomotion control of a modular snake robot. In Proceedings of the 10th International Symposium on Distributed Autonomous Robotic Systems (DARS) (pp. 517– 530).
[^433]: Zahadat, P., Hahshold, S., Thenius, R., Crailsheim, K., & Schmickl, T. (2015). From honeybees to robots and back: Division of labor based on partitioning social inhibition. Bioinspiration & Biomimetics, 10(6), 066005. https://doi.org/10.1088/1748-3190/10/6/066005
[^434]: Zahadat, P., Hofstadler, D. N., & Schmickl, T. (2017). Vascular morphogenesis controller: A generative model for developing morphology of artificial structures. In Proceedings of the Genetic and Evolutionary Computation Conference, GECCO’17, New York, NY, USA (pp. 163–170). New York: ACM. ISBN 978-1-4503-4920-8. http://doi.acm.org/10.1145/3071178. 3071247
[^435]: Zahadat, P., & Schmickl, T. (2016). Division of labor in a swarm of autonomous underwater robots by improved partitioning social inhibition. Adaptive Behavior, 24(2), 87–101. https:// doi.org/10.1177/1059712316633028
[^436]: Zhou, G., He, T., Krishnamurthy, S., & Stankovic, J. A. (2004). Impact of radio irregularity on wireless sensor networks. In Proceedings of the 2nd International Conference on Mobile Systems, Applications, and Services (pp. 125–138). New York: ACM.

