---
theme: prussianblue
contents:
  - Preliminary
  - Introduction
  - Model
  - Evaluation
  - Defense
  - Conclusion
title: USENIX Security Symposium
---

# USENIX Security Symposium

<br>

#### **Poisoning Attacks to Local Differential Privacy Protocols for Key-Value Data**

Yongji Wu, Xiaoyu Cao, Jinyuan Jia, [Neil Zhenqiang Gong](https://gonglab.pratt.duke.edu/)

### Duke University

<div class="pt-12">
  <span> 
    <i>Xiaodong Shi</i>
  </span>
</div>

<!--
- 我要汇报的是来自22年USENIX Security会议集中关于differential privacy领域收录的一篇论文，
- 主要讲了本地差分隐私在处理键值数据时面临的安全性问题, 诸如PrivKVM PCKV-UE和PCKV-GRR， 研究了针对这些协议的数据投毒攻击，
- 加入若干少量的恶意用户，通过添加伪造的数据，使得中心服务器的统计结果出现偏差，
- 作者来自duke大学gong zhenqiang团队。
-->

---
id: 1
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />


<br>
<br>
<br>

# Data Statistics Collection

<img src="data_collection.png" class="mx-auto" />

<!--
- 在传统的数据统计收集方式中,当查询发送到云服务器时,云服务器直接收集用户数据,计算查询统计信息,并回答查询。在这种场景下,数据收集器或传感器服务器需要被信任.
- 各种数据泄露事件突显了依靠数据收集者(例如Equifax)来保护用户私人数据的困难
-->

---
id: 1
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>


# Untrusted Server

<img src="untrusted.png" class="mx-auto" width="570" />

<!--
- 然而,现今用户希望保持其敏感数据的私密性,且服务器可能不可信任,因此研究人员提出了本地差分隐私协议来保护用户隐私。其主要想法是用户在将数据发送到服务器之前通过添加一些噪音来扰乱其数据。使用噪声数据,服务器仍然可以准确计算所需的数据统计信息,同时每个用户的数据无法精确恢复。 
- 现在,许多互联网服务依赖于用户的数据。然而,服务器从用户收集原始数据会给用户的隐私带来重大挑战。
然而,LDP 的安全性在很大程度上还没有探索。
-->

---
id: 1
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>

# Data Poisoning Attacks

<img src="attack.png" class="mx-auto" />

<!--
- 然而,在这项工作中,我们表明LDP协议容易受到数据污染攻击。
- 具体来说,攻击者可以将假用户注入协议,而普通用户遵循协议,假用户实际上可以篡改他们发送到服务器的数据。服务器将计算攻击者希望的带有污染数据的数据统计信息。
- 在这项工作中,我们考虑了LDP的两个最常见应用,即频率估计和散热器识别的数据污染影响。
- 我们考虑LDP协议的两个基本任务,即频率估计和重要项目识别。在
> 频率估计中,数据收集器(也称为中心服务器)旨在估计n个用户中每个项目的频率,而重要项目识别旨在识别n个用户中频率最大的前k个项目。项目的频率定义为拥有该项目的用户比例。 
-->

---
id: 1
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>

# Frequency Estimation
- **Encode: item $v \to$ encoded value $x \in D$**
- **Perturb: randomly perurbs $x \to y$ in $D$**
- **Aggregate: estimate frequency from $y$**

<v-click>

<mimage layout="r" url="pem.png" width="200" />

<br>

# Heavy Hitter Identification
- **Find the most frequent $k$ items**
- **Prefix Extending Method**
  > - *Users are divided into groups*
  > - *Iteratively find portions of frequent values*
  > - *Each group uses OLH*

</v-click>

<!--
#### Frequency Estimation: 
在频率估计中,我们假设每个用户持有一个项目,所需的数据统计信息是每个项目的频率。一般来说,频率估计证明代码执行三个关键步骤来估计频率:
- 首先,用户对项目进行编码以评估编码空间;
- 第二,用户随机扰乱编码值到另一个值;
- 第三,服务器汇总扰乱的编码值以估计项目频率。
> 正式地说,如果满足此条件,则协议满足epsilon LDP。这保证无法从其扰动编码值精确恢复个人项
频率估计的LDP协议由三个关键步骤组成:编码、扰动和聚合。编码步骤将每个用户的项目编码为某个数字值。我们用D表示编码值的空间。扰动步骤在D空间内随机扰动该值,并将扰动后的值发送到中心服务器。在聚合步骤中,中心服务器使用来自所有用户的扰动值估计项目频率。为简单起见,我们用PE(v)表示项目v的扰动编码值。粗略地说,如果任意两个项目被扰动到相同的值的概率很接近,则该协议满足LDP
#### Heavy Hitter Identification: 
接下来,我们将讨论LDP的另一个应用,即重要项目识别。我们在这里要找到的数据统计信息是最频繁的关键项目。我们考虑一种最先进的协议,称为前缀扩展方法(PEM)。
- 在PEM中,用户被划分为组,PEM在每个组内使用oil内部找到频繁值的一部分。
- 重要项目识别的目标是识别n个用户中频率最高的前k个项目。
- 一个直接简单的解决方案是首先使用频率估计协议估计每个项目的频率,然后选择频率最大的k个项目
-->

---
id: 1
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />


# Poisoning Attacks to LDP

- **Goal:** **Promote a set of target items $T$**
  > - *Frequency estimation: increase estimated frequency*
  > - *Heavy hitter identification: promote to be heavy hitters*
<v-click>

- **Backgroud Knowledge:**
  > - *LDP protocol*

</v-click>

<v-click>

- **Capability:**
  > - *Inject fake accounts*

</v-click>

<v-click>

<table>
  <td>
    <mimage layout="l" url="Twitter.png" width="130" />
  </td>
  <td>
    <mimage layout="c" url="fake.png" width="350" />
  </td>
  <td>
    <mimage layout="r" url="Yahoo.png" width="200" />
  </td>
</table>

</v-click>



<!--
- Goal: 接下来,我将介绍我们在这项工作中考虑的第一个模型。我们假定攻击者有一组目标项目要推广,例如,一家公司可能有兴趣使其产品更受欢迎。更具体地说,在频率估计中,攻击者的目标是增加目标项目的估计频率,而在重要项目识别中,攻击者的目标是促进目标项目被识别为top
- backgroud: 对于攻击者的背景知识,我们假设攻击者了解LDP协议。 
- capability: 对于攻击者的能力,我们假设攻击者可以在协议中注入假账号
之前的研究表明攻击者可以以低成本获取假账号 攻击者可以从地下市场的商家购买这些网络服务的假账号/受损账号,价格便宜。
> 例如,Hotmail账号价格在0.004至0.03美元之间;而经电话验证的Google账号价格根据商家在0.03至0.50美元之间。

- 总结：我们对频率估计和重要项目识别的LDP协议进行了第一项系统的数据污染攻击研究。
我们证明,无论是从理论上还是从经验上,我们的攻击可以有效地增加目标项目的估计频率或将其提升为重要项目。
我们探索了三种对策来防御我们的攻击。我们的经验结果凸显了针对我们攻击的新防御措施的需求。
-->

---
id: 2
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>

# LDP protocols exist for various data types.

- #### **Categorical data** 
> *Data that represents a fixed number of categories or choices.*

<div v-click="4">

<mimage layout="r" url="Radar.png" width="350" />

</div>

<div v-click="1">

- #### **Numerical data** 
> *Quantitative data that represents measurable quantities.*

</div>

<div v-click="2">

- #### **Multidimensional data** 
> *Data that contains multiple dimensions or attributes.*

</div>

<div v-click="3">

- #### **Key-value data** 
> *Data that contains pairs of keys and their associated values.*

</div>



<!--
- 分类数据:表示固定数量的类别或选择的数据。例如,性别、职业等。
- 数值数据:表示可测量数量的数据。例如,年龄、身高、温度等。数值数据可以是连续的或离散的。
- 多维数据:包含多个维度或属性的数据。例如,一个数据集具有年龄、性别、身高、体重等列。它为每个数据样本包含多个信息维度。
- 键值数据:包含键和与之关联的值的数据对。例如,{username:“John”,age:30}。键唯一标识值。这在数据库和缓存系统中很常见。
> 分类数据表示一组固定的选择,而数值数据表示可以测量的数量。多维数据和键值数据是描述复杂数据的两种方式。多维数据将多个属性组合在同一个表中,而键值数据使用键来关联不同的属性值。
-->

---
id: 2
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>

# Background

> ***Nowadays, many Internet services rely on users’ data. However, it poses significant challenges to users’ privacy for a server to collect raw data from users.***

<br>

- #### **Companies are collecting more and more data...**

<br>

- #### **Key-value data is pervasive data form, widely used in:**

<img src="KV_exmple.png" class="mx-auto" />

<!--
> 今天的公司正在收集越来越多的数据来改进他们的服务。特别是键值数据是一种常见形式,已经在各个领域看到了应用。
- 例如:推荐系统:使用用户的偏好和兴趣来建议新的产品和内容。通常使用键值对来存储用户、偏好和推荐。
物联网:使用传感器收集的大量数据来监控系统和环境。这些数据通常以键值对的形式存储,其中键标识传感器或测量,值是相应的读数。
- 使用分析:跟踪用户与产品/服务的交互,以便改进设计和体验。键值数据可用于存储每个用户会话的数据,并用于对会话进行分析和建模。
- 所以,键值数据的应用非常广泛,可用于建立用户档案、个性化推荐、监控和优化系统以及对用户体验进行分析。许多公司都依赖于它来积累和利用数据资产。
-->

---
id: 2
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>

# Key-Value Data Collection - RecSys

<table>
  <td>
    <mimage layout="l" url="edge.png" width="500" />
  </td>
  <td>
    <mimage layout="l" url="appstore.png" width="130" />
  </td>
</table>

<!--
> 推荐系统是一个很好的例子。如果像Apple这样的公司想收集我们手机上安装的应用及其使用频率信息,然后根据此信息在App Store中提供更好的推荐,这将是非常有价值的。
edge扩展商店和app store也可能希望收集关于应用的评级数据。因此,他们想知道我们如何评价我们使用的应用。这样,他们就可以了解哪些应用最受欢迎以及它们的评级。
#### 具体来说,Apple可能会收集以下类型的键值数据:
- 应用ID -> 安装次数:跟踪用户安装的每个应用及其安装次数。
- 应用ID -> 使用次数:跟踪用户打开每个应用的次数。
- 应用ID -> 评级:收集用户为每个应用打出的评级或评价。
- 用户ID -> 应用ID列表:跟踪每个用户安装的所有应用。
#### 通过分析此数据,Apple可以:
- 确定最受欢迎和使用频率最高的应用。
- 识别用户可能感兴趣的应用(基于其他用户的安装和使用情况)。
- 根据用户历史使用和评级数据向用户推荐新的应用。
- 识别评级较低的应用及其改进空间。
- 最终改进 App Store 的体验和推荐。
-->

---
id: 2
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>


# Popular Browsers

<img src="popular.png" class="mx-auto" />

<!--
假设一家公司想收集最受欢迎的浏览器,每个用户将有他频繁使用的浏览器作为他的数据。在将原始数据发送到服务器之前,用户将在本地扰动他的数据。其思想是,观察用户与服务器之间通信的对手无法自信地确定原始数据是什么。但是,某些统计信息可以稳定、准确地分析。例如,服务提供商仍然可以知道Chrome是最受欢迎的浏览器。
具体来说:
- 每个用户将报告他最常使用的1-3个浏览器(例如Chrome、Safari和Firefox)。这是他的原始或真实数据。
- 然后,用户将随机选择是否要将浏览器报告中的一个扰乱或更改为其他选项(例如Edge)。这将产生他扰动后要发送到服务器的列表。
- 服务器不能确定用户报告的哪个浏览器是原始数据,哪个是被随机扰乱的。但是,通过聚合来自许多用户的列表,它仍然可以确定较高频率的项目,比如Chrome很可能是最普遍报告的浏览器。
- 所以,尽管数据被扰乱了,服务提供商仍然可以得出相关的统计信息,比如最受欢迎的浏览器等。但同时也难以得出每个用户的具体偏好。这就达到了保护隐私的目的。
通过在发送到服务器之前向用户数据添加随机“噪声”,可以掩盖个人信息并防止对手确定原始数据。但是,如果噪声足够随机,则可从足够大的数据集中得出准确的统计信息。
-->

---
id: 2
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>

# Protocols for Key-Value Data Collection

<img src="ldp4kv.png" width="600" class="mx-auto" />

<br>

# Three key steps

|  **Sample**   | *A user randomly samples a key from the dictionary and constructs a KV pair based on the sampled key* |
| :-------: | :----------------------------------------------------------- |
|  **Perturb**  | *The user perturbs the constructed KV pair to obtain the message that should be sent to the server* |
| **Aggregate** | *The server estimates the frequency and mean value of each key via aggregating the messages from all users* |

<!--
#### 最新的三个针对kv的ldp协议有:PrivKVM、PCKV-UE和PCKV-GRR。这些都是最近几年开发的三种流行协议。
- PrivKVM:一种本地差分隐私协议,允许用户向服务器报告键值数据,同时保护单个密钥和值与用户的对应关系。它通过为每个键值对分配随机ID并报告ID-值映射,而不是直接报告原始的键值对,来实现这一点。
- PCKV-UE:一种两阶段协议,允许用户高效地向服务器报告大量键值数据,同时提供形式上验证的隐私保证。在第一个阶段,用户对数据集中每个键的出现进行编码。在第二阶段,用户只向服务器报告与选择的密钥相对应的编码值。
- PCKV-GRR:一种类似的两阶段协议,使用随机响应(而不是确切的编码)来隐藏密钥的出现。它可以提供更强的隐私保证,代价是准确性略低。
#### 这三种协议各有优点,可根据具体用例选择:
- PrivKVM提供最强的隐私保证,适用于最敏感的数据。但仅支持小规模数据集,并可能产生相当大的常数开销。
- PCKV-UE在隐私和实用性之间取得较好平衡,可高效地支持大规模数据集。但是,其隐私保证相对较弱。
- PCKV-GRR提供最强的隐私保证,但报告的信息可能较不准确。需要根据应用程序的需求权衡这两点。
#### 这三个LDP协议都是由三个关键步骤组成:
- 采样:用户从字典中随机采样一个键,并基于采样的键构造一个键值对。
- 扰动:用户扰乱构造的键值对以获得要发送到服务器的消息。
- 聚合:服务器通过聚合来自所有用户的消息来估计每个键的频率和平均值。
-->

---
id: 2
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>

# LDP is Vulnerable to Attacks

<img src="attack2ldp.png" class="mx-auto" width="750" />

<!--
- 由于分布式设置,LDP协议容易受到数据污染攻击,在这种攻击中,攻击者将假用户注入系统,并通过从假用户发送精心制作的数据来操纵服务器的分析结果。
- 尽管局部差分隐私的初衷是防止对手确定个人用户的数据,但它容易受到数据污染攻击。
- 因此,尽管对手无法确定个人用户的数据,但他可以将一些假用户引入系统,并通过精心制作的消息从假用户发送到服务器。
- 攻击者可以严重损害LDP协议的估计统计信息,即使只有少量假用户也是如此。结果可能会受到严重损害。
#### 具体来说,攻击者可以:
1. 识别LDP协议用于估计特定统计量(如频率或平均值)的机制。
2. 创建假用户并向协议报告经过精心设计的数据,以操纵该统计量的估计结果。
3. 即使假用户只占一小部分,也可以通过选择极端数据值来产生统计量估计的重大变化。
4. 这些变化的统计量估计可能会 mislead 依赖此信息的任何系统或过程。
5. 在某些情况下,攻击者可能利用此信息泄露机密数据或损害用户。
-->

---
id: 2
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>

# LDP Protocols for Key-Value Data

<br>


- #### **We have a dictionary of $d$ keys**

<br>

- #### **Each user has a ser of KV pairs $<k,v>$**

<br>


- #### **$v$ is normalized into [-1,1]**

<br>


- #### **We want to estimate the frequency and mean of each key**

<!-- 
> 在这种设置中,我们通常假设存在某个字典。如果用户有一组键值对,则将值归一化到一个范围内。我们想要估计每个键k的频率和平均值。
#### 具体来说:
1. 假设存在包含所有可能键的字典,我们将其称为键空间K。
2. 每个用户将具有来自K的一组键值对。每个值都将归一化到0到1之间的范围内(或其他固定范围)。
3. 用户然后会将其键值对集输入到LDP协议中。该协议将为每个键k产生一个报告,其中包含:
  - fˆk:键k的估计频率(出现次数)
  - ˆmk:键k的估计平均值(归一化值的总和除以频率)
4. 为了产生这些估计值,LDP协议会:
  - 要求每个用户随机选择并扰动要报告的键值对子集。这可以通过随机采样获得。
  - 用户扰动其选择的键值对,并将其发送到服务器作为“消息”。这些消息被设计为隐藏原始键和值。
  - 服务器通过聚合来自所有用户的消息来估计每个键的频率和平均值。这需要消除消息中的噪声。
5. 关键设计问题是:
  - 采样率和添加的噪声量之间的平衡。更高的噪声提供更好的隐私保证但产生更不准确的估计。
  - 聚合算法必须足够强大,即使在高噪声下也可以产生准确的结果。这可能需要大量用户数据。
  - 整体协议必须提供形式化的隐私保证。任何漏洞都可能被利用以损害隐私。
-->

---
id: 3
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>

# Threat Model

|     |     |
| --- | --- |
| <br>**Attacker's goal** | <br><kbd>**Promote frequency and mean estimation of some target keys**</kbd> |
| <br>**Attacker's knowledge** | <br><kbd>**LDP protocol, including the parameter settings**</kbd> |
| <br>**Attacker's capability** | <br><kbd>**Insert a small fraction of fake users Craft their messages**</kbd> |

<!--
#### 我们将介绍我们的威胁模型。
- 攻击者的目的是提高某个目标案例的频率和平均值估计。 
- 攻击者知道服务器使用的差分隐私机制的具体细节,包括参数设置和字典中键的总数等。
- 攻击者可以注入很少的假用户,最多1%。攻击者可以精心设计假用户发送到服务器的信息。 
-->

---
id: 3
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>
<br>

# Our Three Attacks

<br>

- #### **Baselines**

<br>

  > - *Random Message Attack (RMA)*
  > - *Random Key-Value Pair Attack (RKVA)*

<br>

- #### **Maximal Gain Attack (M2GA)**

<!--  
- 在这项工作中,我们提出了三种针对键值数据的本地差分隐私机制的攻击方式。(PrivKVM PCKV-UE PCKV-GRR) 
- 我们有两种基本攻击和一种基于优化的最大效益攻击。 
-->

---
id: 3
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>
<br>

# Random Message Attack (RMA)

<img src="rma.png" class="mx-auto" />

<!--  
- 在第一个基线攻击,随机消息攻击中,思路很简单。
- 攻击者将在消息域进行每次攻击。
- 因此,对每个假用户,攻击者会在整个消息域中随机选择一条消息发送到服务器。
-->

---
id: 3
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>
<br>

# Random Key-Value Pair Attack (RKVA)

<img src="rkva.png" class="mx-auto" />

<!--  
- 第二种形式的攻击是随机键值对攻击,攻击者针对原始数据空间发起攻击。
- 如果攻击者为每个假用户选择目标键域中的一个键,并将该键的值设置为1。
- 然后,假用户将根据机制扰动原始数据,然后将消息发送到服务器。
-->

---
id: 3
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>

# Maximal Gain Attack (M2GA)

- **Maximize the gains**

- **Solve the two-objective optimization problem:**



$$
\max _{\mathbb{Y}}\left[\begin{array}{l}
G_{f}(\mathbb{Y}) \\
G_{m}(\mathbb{Y})
\end{array}\right]
$$

<br>

| ${\mathbb{Y}}$: crafted messages for the fake users |
| :--: |
| $G_y$: grequency gain |
| $G_m$: mean gain |

<!--  
- 这里是我们基于优化的攻击。
- 该攻击的目的是最大限度地增加频率效益和平均值效益。
#### 过程很简单。
- 我们只需要将频率和最小平均值形式化为两个目标优化问题。
- 优化问题在为假用户设计的消息空间内。
- 基本上,我们需要解决假用户发送的最佳消息,以最大限度地提高频率。平均值估计可以大大减少。
-->

---
id: 4
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>

# Frequency gains of the three attacks

<br>

|      |                           **PrivKVM**                            |                           **PCKV-UE**                            |                           **PCKV-GRR**                           |
| :-- | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| **M2GA** | $\frac{\beta}{1+\beta}\left[1-f_{\mathbb{T}}+\frac{2-r}{e^{\varepsilon / 2}-1}\right]$ | $\frac{\beta \ell}{1+\beta}\left[2 r-f_{\mathbb{T}}+\frac{4 r}{e^{\varepsilon}-1}\right]$ | $\frac{\beta}{1+\beta}\left[\left(1-f_{\mathbb{T}}\right) \ell+\frac{2\left(d^{\prime}-r\right)}{e^{\varepsilon}-1}\right]$ |
| **RMA**  | $\frac{\beta}{1+\beta}\left[\frac{\left(e^{\varepsilon / 2}-2 d+1\right) r}{2\left(e^{\varepsilon / 2}-1\right) d}-f_{\mathbb{T}}\right]$ | $\frac{\beta \ell}{1+\beta}\left[\frac{4 e^{\varepsilon} r}{3\left(e^{\varepsilon}-1\right)}-f_{\mathbb{T}}\right]$ | $\frac{\beta\left(r-f_{\mathbb{T}}d^{\prime}\right) \ell}{\left(1+\beta\right)d^{\prime}}$ |
| **RKVA** | $\frac{\beta}{1+\beta}\left[1-f_{\mathbb{T}}+\frac{1-r}{e^{\varepsilon / 2}-1}\right]$ | $\frac{\beta \ell}{1+\beta}\left(1 - f_{\mathbb{T}}\right)$  | $\frac{\beta \ell}{1+\beta}\left(1 - f_{\mathbb{T}}\right)$  |

<!--
### 在评估方面,我们同时具有理论评估和实证评估。
#### 在理论评估中,我们可以理论上分析我们针对三种方法使用的三种攻击的出现频率以及平均收益。
- PrivKVM、PCKV-UE和PCKV-GRR三种攻击的频率增益。
- β = m/n为伪用户的比例
- fT =∑k∈T fk为目标键的真频率之和
- ε为隐私预算
- d为密钥总数
- L是填充长度，
- d'= d+l为填充字典大小
- r为目标键的个数。
-->

---
id: 4
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>

# Approximate mean gains of the three attacks

<br>

<div class="overflow-auto">

|          |                         **PrivKVM**                          |                         **PCKV-UE**                          |                         **PCKV-GRR**                         |
| :------- | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| **M2GA** | $\sum_{k \in \mathbb{T}} \frac{f_{k} m_{k} r+\frac{e^{\varepsilon_{2}+1}}{e^{\varepsilon}-1} \beta}{f_{k} r+\beta}-m_{k}$ | $\sum_{k \in \mathbb{T}} \frac{2 \beta \ell\left(e^{\varepsilon}+1\right)+\left(e^{\varepsilon}-1\right) f_{k} m_{k}}{2 \beta \ell\left(e^{\varepsilon}+1\right)+\left(e^{\varepsilon}-1\right) f_{k}}-m_{k}$ | $\sum_{k \in \mathbb{T}} \frac{\left(e^{\varepsilon}-1\right)\left(\beta \ell+f_{k} m_{k} r\right)+2 \beta d^{\prime}}{\beta\left[\left(e^{\varepsilon}-1\right) \ell+2\left(d^{\prime}-r\right)\right]+\left(e^{\varepsilon}-1\right) f_{k} r}-m_{k}$ |
| **RMA**  |   $\sum_{k\in\mathbb{T}}\frac{2f_km_kd}{2f_k d+\beta}-m_k$   | $\sum_{k\in\mathbb{T}}\frac{3(e^\mathcal{E}-1)f_k m_k}{3(e^\mathcal{E}-1)f_k+4e^\mathcal{E}\beta\ell}-m_k$ | $\sum_{k\in\mathbb{T}}\frac{f_km_k d'}{f_k d'+\beta\ell}-m_k$ |
| **RKVA** | $\sum_{k \in \mathbb{T}} \frac{f_{k} m_{k} r\left(e^{\varepsilon / 2}+1\right)+e^{\varepsilon / 2} \beta}{f_{k}\left(e^{\varepsilon / 2}+1\right)+e^{\varepsilon / 2} \beta}-m_{k}$ | $\sum_{k\in\mathbb{T}}\frac{f_km_kr+\beta\ell}{f_kr+\beta\ell}-m_k$ | $\sum_{k\in\mathbb{T}}\frac{f_km_kr+\beta\ell}{f_kr+\beta\ell}-m_k$ |

</div>

<!-- 
- PrivKVM、PCKV-UE和PCKV-GRR三种方法遭受三种攻击的近似平均收益。
- fk为键k的真频率，mk为k的真均值，ε2 = ε 2Niter为每一轮PrivKVM的隐私预算，Niter为轮数
-->

---
id: 4
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>

# Datasets
> *We evaluate our three attacks, i.e., M2GA, RMA, and RKVA, on a synthetic dataset and three real-world datasets.*

<br>

| Dataset      |  #users | #keys |  #records | 90th-percentile |
| :----------- | ------: | ----: | --------: | --------------: |
| Synthetic    | 100,000 |   100 |   100,000 |             1.0 |
| Clothing     | 105,508 | 5,850 |   192,198 |             3.0 |
| TakingData   |  60,822 |   320 | 1,327,468 |            34.0 |
| MovieLens-1M |     943 | 1,682 |   100,000 |           244.4 |

<!--  
#### 我们在一个合成数据集和三个真实数据集上评估了我们的三种攻击，即M2GA、RMA和RKVA。
- 合成数据集:我们创建一个合成数据集来评估我们的攻击。具体来说,我们生成了10000个用户和100个密钥。每个用户只有一个键值对。密钥和值遵循平均值为零的高斯分布,其中密钥的标准差为15,值的标准差为1。
- 服装数据集:这是一个产品尺寸推荐的服装适度数据集。它包含用户对不同产品的评分。我们将每个产品视为一个密钥,将每个评分视为一个值。请注意,每个用户可能有多个产品和评分对。
- TalkingData数据集:此数据集包含用户在其移动设备上下载的移动应用程序。具体来说,我们将每个移动应用程序类别视为一个密钥,将用户在一个类别中下载的应用程序数视为一个值。一个用户可能有多个键值对。
- MovieLens-1M数据集:此数据集包含用户对不同电影的评分。每个电影是一个密钥,每个评分是一个值。一个用户可能会评分多部电影。
> 我们将每个数据集中的值缩放,使其落在[-1,1]的范围内。
-->

---
id: 4
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>

# Theoretical Evaluation

- M2GA is the best-performing attack;

- The frequency gain of an attack increases as # of fake users increases;

- The smaller the true mean value is, the larger the (approximate) mean gain is.

<mimage layout="c" url="synthetic.png" width="650" />

<!-- 
- 图1展示了不同参数(β, ε, r)对合成数据集上的频率增益的影响,三行分别为PrivKVM、PCKV-UE以及PCKV-GRR
- 图2展示了不同参数(β, ε, r)对=合成数据集上的平均增益的影响，三行PrivKVM、PCKV-UE以及PCKV-GRR
> PrivKVM、PCKV-UE和PCKV-GRR三种方法在合成数据集上的频率收益和平均增益会根据参数β、ε和r的不同产生变化。

我们提出的基于优化的攻击方法表现最佳。
攻击的频率增益随着伪用户数的增加而增加,这与预期相符。
真实平均值越小,近似最小增益越大。
-->

---
id: 4
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>

# Empirical Evaluation

| ***Promoting*** **one target key in a rating dataset with PCKV-UE protocol** |
| :----------------------------------------------------------: |
|  <img src="dataset.png" width="600" class="mx-auto" />  |
|               **$\beta$: fraction of fake users**                |
| ***Takeaway:*** **huge frequency and mean gains, even with a small $\beta$** |

<!--  
#### 现在我们将论述我们的实证评估。
- 这里展示了在一个评级数据集上采用PCKV-UE协议推广单个目标密钥k的结果。我们可以这么说,x轴表示伪用户的比例。y轴表示频率收益和最小收益。也就是受限频率和最小值之间的差异。攻击前后。关键点是,我们可以说,即便只有很小比例的伪用户,比如1%,频率和平均值估计也会大大受损。
-->

---
id: 4
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>

# Empirical Evaluation - RecSys

<img src="empirical.png" class="float-right ml-5" />

**Promoting** 
> *10 target items in a recommender system*

**ASR:** *attack success rate* 
> *fraction of the 10 target items that are among the top-20 after attack*

**Takeaway:** 
> *recommendation result is greatly compromised, even with a small $\beta$*

<!--  
#### 我们也将我们的攻击方法应用于下游用例——推荐系统。
- 在这个案例中,我们在推荐系统中推广10个目标项目。
- 这里,y轴表示攻击成功率。
- beta: fraction of fake users
- 它被定义为10个目标项目中的一部分,如果攻击后它们被推荐系统推荐在前20名之内,那么我们认为它成功了。
- 因此,我们可以说,需要注意的一点是,我们选择的10个目标项目不是攻击前最流行的项目。
- 所以我们可以说,即使只有1%的伪用户,我们也可以完全破坏推荐结果,我们选择的目标项目将在攻击后全部推荐给用户。
-->

---
id: 5
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>

# One-class Classifier (OC) based Detection

- Treat each user's message as its features

- Assumption
  > *Server knows a fraction of genuine users*

<br>

<v-click>

# Anomaly Score (AS) based Detection

- Multiple rounds of communications are conducted in PrivKVM

- We can then check consistency of messages from a user across multiple rounds
- We assign an anomaly score to each user
- If the score is greater than anomaly threshold $\eta$, consider the user to be fake

</v-click>

<!-- 
#### 我们探索了两种方法来检测虚假用户作为防御我们的投毒攻击。对于这两种方法，我们假设服务器知道从每个用户发送的KV对。
- 对于基于单类分类器的检测，我们进一步假设服务器知道真正用户的比例λ
  > 检测虚假用户本质上是一个异常检测问题，我们的目标是将虚假用户与真实用户区分为离群值。因此，我们可以利用通常用于异常(离群值)检测的一类机器学习分类器来检测虚假用户。具体来说，我们将每个用户发送到服务器的消息视为其特性。在我们的实验中，我们使用隔离森林。隔离森林训练随机划分的树的集合以检测离群值。在训练之后，隔离林可以将用户分类为两个组。服务器将包括更多真实用户的组视为"genuine"组，而将另一个组视为"fake"组. "fake"组中的用户被视为假用户，不包括在聚合中。服务器只使用"genuine"组的用户发送的消息来估计频率和平均值。
- AS:我们注意到，在PrivKVM中进行了多轮通信，允许我们检查用户在不同轮发送的消息的一致性。在此基础上，提出了一种针对PrivKVM的伪用户检测方法。
  > 回想一下，在PrivKVM中，每个用户在每轮向服务器发送一个扰动的KV对和一个密钥的索引。因为键是随机从大字典中采样的，所以对于真正的用户来说，不太可能多次重复选择相同的键。但是，由于假用户每轮都会提升一个目标密钥，所以可能会在多轮中向服务器发送相同的密钥，特别是在目标密钥数量较少的情况下。基于这种直觉，我们给每个用户分配一个异常参数，我们将其定义为用户向服务器发送相同键索引的最大轮数。如果用户的异常参数不小于η(\eta称为异常阈值)，那么我们将用户标记为假用户。我们计算每个用户的异常参数，并在每一轮中检测出虚假用户。当某个用户在某一轮中被检测出是假用户时，我们在随后的几轮中排除该用户以进行均值估计。此外，我们根据用户在第一轮发送的消息，通过删除属于被检测到的假用户的消息，重新估计密钥的频率。
-->

---
id: 5
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>

# Experimental Results

| False positive rate | False negative rate |
| :-----------------: | :-----------------: |
|   <img src="positive.png"  class="mx-auto" />    |    <img src="negative.png"  class="mx-auto" />       |
| OC: One-class classifier  | AS: Anomaly score  |


<!--  
#### β， r， λ对TalkingData上针对M2GA检测假用户的FPR(第一行)和FNR(第二行)的影响
- 理想的防御应该具有接近零的假阳性率和接近零的假阴性率。
- 所以它永远不会将普通用户误判为假用户。
- 它也将准确地检测所有假用户。
- 防御或两个方法在某些场景下是有效的,但在其他场景下仍然相当有限。
- 举个例子,对于我们的一类分类器,当假用户的数量较少时,它的假阴性率会非常接近1,对吗?
- Anomaly Score (AS) based Detection方法只对PrivKVM起作用。
> 另一种防御方法是在本地化差分隐私协议中使用可验证的计算。例如，在收集键值对时，服务器可能会利用同态加密。然而，这样的方法会在用户端产生巨大的计算开销，降低用户体验。其他潜在的防御措施包括基于用户的额外信息检测虚假用户，例如，他们的社会联系或注册信息。然而，这些检测方法不适用时，所需的信息是不可得的。
-->

---
id: 6 
---

<img src="DMU.png" class="top-5 right-5 absolute" width="80" />

<br>
<br>
<br>
<br>

# Conclusion

<br>

- #### **Key-value LDP protocols are vulnerable to poisoning attacks**

<br>

- #### **An attacker can promote frequency/mean of any target items**

<br>

- #### **We highlight the need for strong defenses against such attacks**

<br>

> *Our defends help to degree, but there is more work to do.*

<!-- 
- 本文首次对键值数据的本地化差分隐私协议投毒攻击进行了系统研究。
- 证明了这类投毒攻击可以表示为一个双目标优化问题。
- 结果表明，攻击者可以提高所选目标键的估计频率和均值。
- 我们还探索了两种防御，它们在某些情况下是有效的，但在其他情况下是无效的。
- 未来一项有趣的工作是研究我们对攻击的防御。
-->
