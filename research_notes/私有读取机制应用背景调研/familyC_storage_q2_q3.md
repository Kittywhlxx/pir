# 应用家族 C：托管的加密对象存储（文档 / 邮件索引 / KV）的私密点查 —— Q2 问题存在性 与 Q3 占位检查

> **证据可核性说明（必读）**：本环境的出站代理封锁了 arxiv.org、eprint.iacr.org、usenix.org、ndss-symposium.org、vldb.org、dl.acm.org、ieeexplore、springer、semanticscholar、作者主页等几乎全部论文托管域（均返回 CONNECT 403），仅 raw.githubusercontent.com 可达。因此：
> - 标注 **[原文直读]** 的引句来自本人直接抓取的一手文本（仅两条：Pantheon 官方仓库 README、vORAM 官方 `obliv` 仓库 README）。
> - 其余引句均来自搜索引擎对论文原文页面（PDF/摘要页）的逐句摘录，标注 **[待核：搜索摘录]**。它们附有原文 URL，报告撰写者/裁决者需按 URL 打开原文核对页码与措辞；本文**不**为这些句子补充页码/章节号（除非摘录中明确给出）。
> - 本文不下"新颖性"结论，只记录可核事实与缺口。

---

## Q2 问题存在性：存储社区论文是否明确认定「读哪个对象敏感 + 对象大小可变 + 内容外包且加密」的场景

### Takeaway
三点分别在 oblivious storage / 加密 KV / SSE 攻击文献中有明确原句：(i) "encryption alone is not sufficient … access patterns can leak sensitive information"（ObliviStore、Path ORAM、Snoopy）；(ii) 大小可变且大小本身泄漏/填充昂贵（Jia–Agarwal–Khandelwal USENIX Sec 2024 "Length Leakage"、Blackstone–Kamara–Moataz NDSS 2020 volume attacks、Weighted ORAM Eurocrypt 2023、vORAM/DivORAM 动机句）；(iii) 内容外包给不受信服务器且加密（ObliviStore、TaoStore、Snoopy、Pancake）。**但三点同时出现在同一篇论文、并且以"按长度档分组泄漏"为契约的，本次只找到 Jia et al. 2024（bin-packing 到 max-size bin 后跑 Pancake）**——其泄漏粒度与单服务器性见 Q3。

### Cited Findings

#### (i) 访问哪个对象/键是敏感的（access-pattern 泄漏的具体危害句）
- ObliviStore (Stefanov & Shi, IEEE S&P 2013) 引言：**"It is well-known that encryption alone is not sufficient for ensuring data privacy, since data access patterns can also leak a considerable amount of sensitive information."** 并引用 Islam et al. 的结果：access patterns 可通过统计推断泄漏 **"up to 80% of the search queries made to an encrypted email repository"**。[待核：搜索摘录] — [ObliviStore PDF (cs.umd.edu)](https://www.cs.umd.edu/~elaine/docs/ObliviStore.pdf)；[NDSS 分布式版](https://www.ndss-symposium.org/wp-content/uploads/2017/09/oblivistore-high-performance-oblivious-distributed-cloud-data-store.pdf)
  - 注：此句直接把"加密邮件库"作为 access-pattern 泄漏危害的例子，对家族 C 的"邮件索引"子场景有直接支撑价值。
- ObliviStore 摘要：**"we must prevent information leakage not only through access patterns, but also through timing of I/O events."**（时序也是泄漏面）[待核：搜索摘录] — 同上
- Path ORAM (Stefanov et al., CCS 2013 / JACM 2018)：**"data encryption alone is often not enough to protect users' privacy in outsourced storage applications."** ORAM 定义为 **"a cryptographic construction that allows a client to access encrypted data residing on an untrusted storage server, while completely hiding the access patterns to storage."** [待核：搜索摘录] — [PathORAM JACM PDF](https://people.csail.mit.edu/devadas/pubs/PathORAMJACM.pdf)
- Snoopy (Dauterman, Fang, Demertzis, Crooks, Popa, SOSP 2021) 引言：**"Encryption cannot fully protect this data: how the user accesses data can leak sensitive information to the cloud. For example, the frequency with which a doctor accesses a medication database might reveal a patient's diagnosis."** [待核：搜索摘录] — [Snoopy ePrint 2021/1280](https://eprint.iacr.org/2021/1280.pdf)；[ACM DL](https://dl.acm.org/doi/10.1145/3477132.3483562)
- Snoopy 对公开泄漏的自我声明：**"the attacker can learn public information such as the number of requests sent by each client."** [待核：搜索摘录] — [Snoopy PDF (nacrooks.github.io)](https://nacrooks.github.io/bibliography/publications/2021-sosp-snoopy.pdf)
  - 注：与本机制的公开泄漏项"请求次数与时序"属同类表述。
- Pantheon (Ahmad, Agrawal, El Abbadi, Gupta, PVLDB 16(4), VLDB 2023) 官方仓库 README **[原文直读]**：**"Consider a cloud server that owns a key-value store and provides a private query service to its clients. The key-value store in this case is _public_ and a client cannot encrypt or modify it to ensure privacy. Pantheon is a system that allows its clients to retrieve the value corresponding to a key from such a key-value store privately, i.e. without letting anyone else in the world (even the server) to know which key the client is interested in."** — [github.com/ishtiyaque/Pantheon README](https://raw.githubusercontent.com/ishtiyaque/Pantheon/main/README.md)
- Pantheon 论文 §2.2 威胁模型（搜索摘录转述）：被动敌手 **可 "see the content of the key-value store and monitor and store all queries and responses from clients"、"monitor and analyze any operation the server performs and mount side-channel attacks on the server"、"monitor, log, and analyze all network traffic"**；目标为 "query privacy and result integrity"。[待核：搜索摘录；章节号 2.2 来自摘录] — [Pantheon ePrint 2023/1331](https://eprint.iacr.org/2023/1331.pdf)；[VLDB PDF](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)
- Pancake (Grubbs, Khandelwal, Lacharité, Brown, Li, Agarwal, Ristenpart, USENIX Sec 2020)：攻击只需 **"a passive persistent adversary"**，即 **"one that observes access patterns but does not actively performs accesses"**；形式模型中敌手 **"both knows the distribution and obtains the transcript of (encrypted) queries and responses"**。[待核：搜索摘录] — [Pancake ePrint 2020/1501](https://eprint.iacr.org/2020/1501.pdf)；[Cornell 镜像](https://www.cs.cornell.edu/~ragarwal/pubs/pancake.pdf)
- SSE/邮件：Cash, Grubbs, Perry, Ristenpart (CCS 2015) 以 Enron 邮件集为语料，提出 leakage-abuse attacks；count attack 基于 "access and size patterns leakage"（后者为综述转述）。[待核：搜索摘录] — [ePrint 2016/718](https://eprint.iacr.org/2016/718)

#### (ii) 对象/值大小可变，且大小本身泄漏信息或使填充成本高
- Jia, Agarwal, Khandelwal, "Length Leakage in Oblivious Data Access Mechanisms" (USENIX Security 2024, pp. 6417–6434)：
  - 动机：真实生产数据中 **object sizes 可相差 "several orders of magnitude"**，故 length leakage 重要。[待核：搜索摘录] — [ePrint 2024/863](https://eprint.iacr.org/2024/863.pdf)
  - 摘要：**"explores the problem of preventing length leakage in oblivious data access mechanisms with passive persistent adversaries"**；**"designing mechanisms that prevent both length leakage and access pattern leakage requires navigating additional difficulties"**，形成 storage / bandwidth / leakage 三方权衡。[待核：搜索摘录] — [USENIX 页面](https://www.usenix.org/conference/usenixsecurity24/presentation/jia-grace)；[ACM DL](https://dl.acm.org/doi/10.5555/3698900.3699259)
  - 填充到最大：padding-based scheme 叠加 ORAM/Pancake 可达 bandwidth-optimal，**只泄漏 "the number of objects and the largest object size"**，且任何只泄漏这两项的方案 **必然承受与最大对象尺寸挂钩的最小带宽开销**。[待核：搜索摘录] — [USENIX PDF](https://www.usenix.org/system/files/usenixsecurity24-jia-grace.pdf)
  - 论文还指出 packing-based 设计 **"leak information about the sum of plaintext object sizes"**。[待核：搜索摘录] — [ePrint 2024/863](https://eprint.iacr.org/2024/863.pdf)
  - 其 USENIX 演讲 slides：oblivious stores **"are still vulnerable to length leakage attacks"**，基线安全模型 **"assumes the stored values have the same size"**。[待核：搜索摘录] — [slides](https://usenix.org/system/files/usenixsecurity24_slides-jia_grace.pdf)
- Blackstone, Kamara, Moataz, "Revisiting Leakage Abuse Attacks" (NDSS 2020)：四个新攻击中 **三个只用 document-size 泄漏**，因此 **"also work against ORAM-based solutions"**；另有两个 **volumetric injection attacks**；定义 "volume pattern" = 匹配文档的大小集合，"total volume pattern" = 其和；实验基于 Enron。[待核：搜索摘录] — [ePrint 2019/1175](https://eprint.iacr.org/2019/1175)；[NDSS PDF](https://www.ndss-symposium.org/wp-content/uploads/2020/02/23103-paper.pdf)
- ShieldDB (arXiv 2003.06103)：**"ORAM does not hide the size of query results unless non-trivial padding is added"**（转述）。[待核：搜索摘录] — [arXiv 2003.06103](https://arxiv.org/pdf/2003.06103)
- Weighted ORAM (Assouline & Minaud, Eurocrypt 2023)：朴素做法把文件切成定长块 **迫使填充到块大小的整数倍，在文件多为小文件时代价高；缩小块以减填充则抬高 ORAM 开销（开销随块数增长）**（转述，摘录标注 sentences 6-5–6-12）。[待核：搜索摘录] — [IACR archive PDF](https://www.iacr.org/archive/eurocrypt2023/14004431/14004431.pdf)
- vORAM (Roche, Aviv, Choi, IEEE S&P 2016)：自称 **"the first ORAM to allow varying block sizes without trivial padding"**。[待核：搜索摘录] — [arXiv 1505.07391](https://arxiv.org/pdf/1505.07391)
- DivORAM (Liu et al., Information Sciences 447, 2018)：**"most ORAM schemes support only storing blocks of the same size"**，变长数据通常被填充到统一长度，膨胀存储与带宽（后半为转述）。[待核：搜索摘录] — [Middlesex repository](https://repository.mdx.ac.uk/item/87v99)
- Tiptoe (SOSP 2023) 对变长 URL 的处理：**"SimplePIR serves data to the client only in relatively large chunks—roughly 40 KiB with our parameter settings. The client must always fetch at least one of these chunks from the server."**；作者 **"group URLs into batches and compress them, dropping any URLs that are more than 500 characters in length"**；嵌入层 **"we pad the height of the matrix M to the size of the largest cluster."** [待核：搜索摘录] — [Tiptoe ePrint 2023/1438](https://eprint.iacr.org/2023/1438.pdf)；[PDOS PDF](https://pdos.csail.mit.edu/papers/tiptoe:sosp23.pdf)
- Nakamori, Motomura, Fujiki, "Densely-Packed Single-Server PIR for Variable-Length Data" (Zenodo 记录标注 Euro S&P, 2025-07-01)：摘要称现有变长记录单服务器 PIR 优化 **"either add significant padding overhead or require complex key management"**。[待核：搜索摘录] — [Zenodo 16740584](https://zenodo.org/records/16740584)

#### (iii) 内容外包给不受信服务器且加密
- TaoStore (Sahin, Zakhary, El Abbadi, Lin, Tessaro, IEEE S&P 2016)：**"We consider oblivious storage systems hiding both the contents of the data as well as access patterns from an untrusted cloud provider."** 场景：**"multiple users from a trusted group (e.g., corporate employees) asynchronously access and edit potentially overlapping data sets through a trusted proxy mediating client-cloud communication."** [待核：搜索摘录] — [TaoStore PDF (UCSB)](https://sites.cs.ucsb.edu/~rachel.lin/papers/TaoStore-CameraReady.pdf)
- Obladi (Crooks et al., OSDI 2018)：trusted proxy 向 **"an untrusted storage server"** 发读写；服务器 honest-but-curious，可观察并控制 proxy 流量时序。[待核：搜索摘录] — [arXiv 1809.10559](https://arxiv.org/pdf/1809.10559)；[USENIX PDF](https://www.usenix.org/system/files/osdi18-crooks.pdf)
- vORAM 官方 `obliv` 包 README **[原文直读]**：**"In practice, the backend storage is a fixed-size list of fixed size blocks, all of which are encrypted."** 以及 **"anyone who sees the *access pattern* (the sequence of read/write requests to the backend server) will provably not learn anything about what data is being read or written, only some idea of *how much* total data is being stored and accessed."** — [github.com/dsroche/obliv README.rst](https://raw.githubusercontent.com/dsroche/obliv/master/README.rst)
- Pancake：**"Batch imposes bandwidth overhead exactly B× over a KV store that just uses encryption and leaks access patterns."**（对比基线即"仅加密的 KV 存储"）[待核：搜索摘录] — [Pancake ePrint 2020/1501](https://eprint.iacr.org/2020/1501.pdf)
- 反例（非加密）：Pantheon 的 KV 存储是**公开明文**（README [原文直读]："The key-value store in this case is _public_ and a client cannot encrypt or modify it"），不满足 (iii)。

### Inferences
- (i)+(iii) 在 ORAM/oblivious storage 文献里是标准开场白（ObliviStore、Path ORAM、TaoStore、Snoopy 均有近似原句），且 ObliviStore 明确以"加密邮件库"为例，可直接支撑家族 C 的文档/邮件子场景。
- (ii) 在 2019–2024 由攻击方（Blackstone et al. volume attacks）与防御方（Jia et al. 2024 Length Leakage）双向确认：仅隐藏访问模式不够，长度/体积本身是独立泄漏面；而消除长度泄漏的代价由最大对象尺寸决定。
- 推断（非原文）：Jia et al. 2024 的"bin-pack 到 max-object-size 的 bins 后跑 Pancake"与本机制的"按 2^k 容量档分组为信封"同属"把变长对象归并到等尺寸容器、把泄漏契约降到容器粒度"的思路，但其容器是单一最大尺寸而非多档（见 Q3 Gaps，需核原文）。

### Gaps
- 所有论文原文 PDF 在本环境均不可达，Q2 引句（除两条 README）未能逐字核对页码/章节；需裁决者按 URL 核对。
- 未找到任何一篇论文在**同一段落**同时陈述 (i)(ii)(iii) 三点并把"按长度档分组"作为既定泄漏契约；最接近的是 Jia et al. 2024（三点皆涉及），但其是否明确写出"size-bucket 级泄漏"需核原文。
- 未能检索 Shroud、ObliFS、OBFUSCURO 的原句（预算与网络限制）。

---

## Q3 占位检查：已有系统是否在该场景实现"私密点查变长对象"，并同时满足「变长记录 + 泄漏契约到分组/长度档粒度 + 单服务器」

### Takeaway
逐项核查的 9 个系统中，**没有一个在可核证据上同时满足三条**：HHG 2013 满足变长但是多服务器 IT-PIR 且契约为"不泄漏块数"而非长度档；vORAM 变长+单服务器但契约为"仅总量"且依赖几何分布假设；Pantheon 单服务器但每次运行固定 value 大小且存储为公开明文；Pancake 单服务器但假设等长值；Jia et al. 2024 变长+单服务器，契约为"对象数 + 最大对象尺寸"或 bin 级（是否多档 [待核]）；Nakamori et al. 2025 变长+单服务器，泄漏描述 [待核]。

### Cited Findings（逐系统）

**(1) Pantheon — "Private Retrieval from Public Key-Value Store"，PVLDB 16(4): 643–656，VLDB 2023（Ahmad, Agrawal, El Abbadi, Gupta）**
- 记录模型：官方 README **[原文直读]** 的 CLI 参数：**"-k The size of each key in bits. -s The size of each value in Bytes."**，示例 **"each key is `64-bits` and each value is `256 Bytes`"** → 每次部署一个固定 value 大小。— [README](https://raw.githubusercontent.com/ishtiyaque/Pantheon/main/README.md)
- 论文设计目标（摘录）：**Pantheon "should support arbitrarily large values, because hashing a value does not serve the purpose in most cases"**；并要求 **"support at least 256-bit keys"**（键经抗碰撞哈希）；三个规模参数为 key size、value size、n。基准 **"each key is 32 bits and each value is 256 bytes long"**。[待核：搜索摘录] — [ePrint 2023/1331](https://eprint.iacr.org/2023/1331.pdf)
- 单服务器：README 标题 "Single-server deployment" [原文直读]；论文 §2.2 被动敌手可看全库、全流量与侧信道 [待核：搜索摘录]。
- 泄漏描述：README [原文直读] "without letting anyone else in the world (even the server) to know which key the client is interested in"；摘要：server 或任何敌手 **不得获知 key 或 value 的任何信息**。[待核：搜索摘录]
- 存储性质：**公开明文**（README [原文直读]）。
- 后续 Femur (SIGMOD 2025, arXiv 2503.05376) 评论：含 Pantheon 在内的全安全方案 **"face rapid growth in computational cost caused by the larger key-value pair length"**；Femur 自述 **"each key-value pair is stored in plaintext with a uniform length to prevent attackers from learning more"**（摘录截断）。[待核：搜索摘录] — [arXiv 2503.05376](https://arxiv.org/pdf/2503.05376)
- **三条判定**：变长 ✗（单一 value size 参数；"arbitrarily large" 是容量要求非变长契约 [待核]）；长度档契约 ✗（未见）；单服务器 ✓。另：存储为公开明文而非加密外包，与家族 C 的 (iii) 不符。

**(2) Henry, Huang, Goldberg — "One (Block) Size Fits All: PIR and SPIR with Variable-Length Records via Multi-Block Queries"，NDSS 2013**
- 记录模型：变长记录被切成定长 b-bit 块，用户一次查询取回多块：**"the user fetches several b-bit blocks in a single query without revealing either the number of blocks fetched or the index of any block"**；**"Multi-block PIR queries naturally lead to PIR queries over variable-length records without padding or other efficiency-harming workarounds."** [待核：搜索摘录] — [cypherpunks.ca PDF](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)；[NDSS 页面](https://www.ndss-symposium.org/ndss2013/ndss-2013-programme/one-block-size-fits-all-pir-and-spir-variable-length-records-multi-block-queries/)
- 服务器数：扩展 **Goldberg's robust IT-PIR**（信息论多服务器）；机制为以 ramp scheme 替换 Shamir 共享，**"trades off some Byzantine robustness to improve throughput without affecting privacy"**。[待核：搜索摘录]
- 泄漏契约：隐藏取回块数与块索引（即隐藏记录长度），而非"长度档"。
- **三条判定**：变长 ✓；长度档契约 ✗（契约更强：块数也隐藏，但依赖多服务器）；单服务器 ✗（IT-PIR 多服务器）。

**(3) vORAM / HIRB — Roche, Aviv, Choi, "A Practical Oblivious Map Data Structure with Secure Deletion and History Independence"，IEEE S&P 2016（arXiv 1505.07391；ePrint 2015/1126）**
- 记录模型：基于 non-recursive Path ORAM，桶大小固定，**每桶可容纳任意多个变长块或块片段**，块可跨同一路径的多个桶；**"the first ORAM to allow varying block sizes without trivial padding"**。[待核：搜索摘录] — [arXiv 1505.07391](https://arxiv.org/pdf/1505.07391)
- 安全条件：obliviousness 与小 stash 成立 **"provided block sizes follow a geometric distribution"**（摘要）；定理假设块长有尾界（期望 B，超过 mB 的概率 < 0.5^m），桶 ≥ 20B 时 stash 超 RB 概率 < 28·(0.883)^R。[待核：搜索摘录]
- 泄漏描述（官方包 README **[原文直读]**）：观察者 **"only [learns] some idea of *how much* total data is being stored and accessed"**；后端为 **"a fixed-size list of fixed size blocks, all of which are encrypted"**。— [obliv README.rst](https://raw.githubusercontent.com/dsroche/obliv/master/README.rst)
- 单服务器：单一不受信后端（README：远程 sftp 服务器或本地目录）[原文直读]。
- 第三方评价：Practical Volume-Based Attacks (arXiv 2008.06627) 称 vORAM 对结果体积 **只能"to some extent"隐藏**，观察到的路径取回数可估计结果数。[待核：搜索摘录] — [arXiv 2008.06627](https://arxiv.org/pdf/2008.06627)
- **三条判定**：变长 ✓；长度档契约 ✗（契约为"总量"级且依赖几何分布假设，非分组/档位）；单服务器 ✓。

**(4) Pancake — Grubbs et al., "Pancake: Frequency Smoothing for Encrypted Data Stores"，USENIX Security 2020**
- 记录模型/值大小：本次检索**未取得**论文中关于值长度的原句；仅有间接证据：Jia et al. 2024 slides 称 oblivious stores 的基线安全模型 **"assumes the stored values have the same size"**，且该文把 Pancake 作为需叠加 padding/bin-packing 才能处理长度泄漏的底层机制。[待核：搜索摘录] — [slides](https://usenix.org/system/files/usenixsecurity24_slides-jia_grace.pdf)；[ePrint 2024/863](https://eprint.iacr.org/2024/863.pdf)
- 单服务器 / 加密：被动持久敌手观察加密 KV 存储的 transcript（见 Q2 (i)）。[待核：搜索摘录]
- 泄漏契约：安全目标为 transcript 与 **"a sequence of uniformly distributed accesses to random bit strings"** 不可区分（频率平滑，非 PIR/ORAM 级隐藏）。[待核：搜索摘录] — [ePrint 2020/1501](https://eprint.iacr.org/2020/1501.pdf)
- **三条判定**：变长 ✗（间接证据指向等长值 [待核原文 §2]）；长度档契约 ✗；单服务器 ✓。

**(5a) Jia, Agarwal, Khandelwal — "Length Leakage in Oblivious Data Access Mechanisms"，USENIX Security 2024**
- 记录模型：变长对象；两类构造：(a) padding 到最大对象尺寸后跑 ORAM/Pancake，**泄漏 "the number of objects and the largest object size"**；(b) 广义构造 **"bin-packs objects into bins of the maximum object size and then runs a Pancake-based oblivious access mechanism over those bins"**，以更多存储换更低带宽、"a bit more leakage"。[待核：搜索摘录] — [USENIX PDF](https://www.usenix.org/system/files/usenixsecurity24-jia-grace.pdf)
- 泄漏粒度：(a) 对象数 + 最大尺寸；(b) bin 级（bins 均为最大对象尺寸，即单一容量档，而非多档 2^k）[待核：此判断基于摘录，需核原文是否有多尺寸 bins]；另指出 packing 设计泄漏 **"the sum of plaintext object sizes"**。
- 单服务器：passive persistent adversary 的加密 KV 存储（Pancake 设定）[待核]。
- **三条判定**：变长 ✓；长度档契约 ？（bin 级但 bin 为单一最大尺寸 [待核]）；单服务器 ✓。**这是本次检索中与"变长 + 分组级契约 + 单服务器"最接近的已发表工作，裁决前必须读原文 §(构造与泄漏定义)。**

**(5b) Nakamori, Motomura, Fujiki — "Densely-Packed Single-Server PIR for Variable-Length Data"，Zenodo 记录标注 Euro S&P，2025-07-01**
- 摘要：现有变长单服务器 PIR **"either add significant padding overhead or require complex key management"**；提出协议 **"packs variable-length entries as efficiently as symmetric-key schemes but without any additional key management"**，并保证每次查询至多返回一条记录；实验中比 padding 方案在不同长度分布下更稳定，计入密钥传输时最快 1.8×。[待核：搜索摘录] — [Zenodo 16740584](https://zenodo.org/records/16740584)
- 泄漏描述：**未取得**（摘要未述；全文不可达）。
- **三条判定**：变长 ✓；长度档契约 [待核]；单服务器 ✓（题名）。**裁决前必读全文。**

**(5c) Tiptoe — Henzinger, Dauterman, Corrigan-Gibbs, Zeldovich, "Private Web Search with Tiptoe"，SOSP 2023**
- 变长处理：URL 批量压缩（平均 22 bytes/URL），**丢弃 >500 字符的 URL**；SimplePIR 以约 40 KiB chunk 为单位返回；嵌入矩阵按最大 cluster 填充。[待核：搜索摘录] — [ePrint 2023/1438](https://eprint.iacr.org/2023/1438.pdf)
- **三条判定**：变长 ✓（应用层批压缩+丢弃+填充）；长度档契约 ✗（填到最大）；单服务器 ✓。

**(5d) Respire — Burton, Menon, Wu, "Respire: High-Rate PIR for Databases with Small Records"，CCS 2024（ePrint 2024/1165）**
- 记录模型：**"we focus on the setting where each database record is 256 bytes"**（定长小记录）；batch 模式用 cuckoo hashing 把索引分到 bucket，**每 bucket 作为固定大小数据库查询**（B ≥ 3T/2，每 bucket ≤ K ≤ 2N/T，K 为 2 的幂）。[待核：搜索摘录] — [ePrint 2024/1165](https://eprint.iacr.org/2024/1165.pdf)；[UT PDF](https://www.cs.utexas.edu/~dwu4/papers/Respire.pdf)
- **三条判定**：变长 ✗（定长记录；bucket 是为批查询而非长度分档）；长度档契约 ✗；单服务器 ✓。

**(5e) DivORAM — Liu, Huang, Li, Cheng, Shen，Information Sciences 447: 1–11, 2018**
- 变长块 ORAM，服务器侧用 Damgård–Jurik 同态；声称较 Ring ORAM 省 30% 带宽、较 HIRB 省 40%。泄漏分析**未取得**。[待核：搜索摘录] — [Middlesex repository](https://repository.mdx.ac.uk/item/87v99)
- **三条判定**：变长 ✓；长度档契约 [待核]；单服务器 ✓（推断自服务器同态计算，[待核]）。

**(5f) Weighted ORAM — Assouline & Minaud，Eurocrypt 2023**
- 每块可有各自大小，受最大块大小 B 与最大总权重 N 约束；**"parameters depend only on those bounds, not on the individual size distribution"**（转述）。[待核：搜索摘录] — [IACR news 20293](https://iacr.org/news/item/20293)；[Eurocrypt PDF](https://www.iacr.org/archive/eurocrypt2023/14004431/14004431.pdf)
- **三条判定**：变长 ✓；长度档契约 ✗/[待核]（契约以 (B, N) 上界表述）；单服务器 ✓（ORAM 单服务器，[待核]）。

**(6) 其他 oblivious storage 系统（定长块，列为对照）**
- ObliviStore (S&P 2013)：评估 **"block size of 4KB"**，多 ORAM 节点 + 不受信云存储；[待核：搜索摘录]。变长 ✗。
- TaoStore (S&P 2016)：trusted proxy + 不受信云，tree-based ORAM 定长块；[待核：搜索摘录]。变长 ✗。
- Obladi (OSDI 2018)：trusted proxy + 不受信存储；变长 ✗（未见变长表述）。
- Snoopy (SOSP 2021)：TEE（enclave）+ 分片 subORAM；公开泄漏含每客户端请求数；变长 ✗（未见变长表述）；服务器模型为 TEE 多机。[待核：搜索摘录]

### 汇总判定表（基于可核摘录；全部标 [待核] 直至原文核对）

| 系统 | venue/年 | 变长记录 | 泄漏契约粒度 | 单服务器 | 同时满足三条？ |
|---|---|---|---|---|---|
| Pantheon | VLDB 2023 | ✗（固定 value size 参数） | 完全隐藏 key/value（存储为公开明文） | ✓ | 否：缺变长；且非加密外包 |
| HHG 2013 | NDSS 2013 | ✓（定长块多块查询） | 隐藏块数与索引（强于长度档） | ✗（IT-PIR 多服务器） | 否：缺单服务器 |
| vORAM/HIRB | S&P 2016 | ✓ | 仅总量（README 原句）；需几何分布假设 | ✓ | 否：契约非分组/档位 |
| Pancake | USENIX Sec 2020 | ✗（间接证据：等长值）[待核] | 频率平滑；长度泄漏未处理 | ✓ | 否：缺变长 |
| Jia et al. Length Leakage | USENIX Sec 2024 | ✓ | 对象数+最大尺寸 / bin 级（单一最大尺寸 bin）[待核] | ✓ | 最接近；是否"多档"[待核] |
| Nakamori et al. Densely-Packed PIR | Euro S&P 2025 [待核] | ✓ | [待核] | ✓ | [待核] |
| Tiptoe | SOSP 2023 | ✓（压缩+丢弃+填充到最大） | 填到最大 | ✓ | 否：契约非档位 |
| Respire | CCS 2024 | ✗（256 B 定长） | — | ✓ | 否 |
| DivORAM | Inf. Sci. 2018 | ✓ | [待核] | ✓[待核] | [待核] |
| Weighted ORAM | Eurocrypt 2023 | ✓ | 以 (B_max, N) 上界表述 | ✓[待核] | 否/[待核] |
| ObliviStore / TaoStore / Obladi / Snoopy | 2013–2021 | ✗ | 定长块 ORAM | 多节点/proxy/TEE | 否 |

### Inferences
- 在可核证据范围内，"变长记录 + 单服务器"的交集只有 vORAM 系（含 DivORAM、Weighted ORAM）、Jia et al. 2024、Nakamori et al. 2025、Tiptoe；其中把泄漏契约写到"分组/长度档"粒度的，摘录中只有 Jia et al. 2024 的 bin 级描述，但其 bin 似为单一最大尺寸，与"2^k 多档"不同（需核原文）。
- Pantheon 的"arbitrarily large values"是对单次部署值尺寸上限的要求，不是对同库内值长度异构的处理；且其 KV 为公开明文，与家族 C 的加密外包前提不同。

### Gaps
- **Nakamori et al. 2025 与 Jia et al. 2024 的全文均未读到**：前者的泄漏模型、后者 bins 是否支持多尺寸档，是裁决"占位"与否的关键，必须由裁决者按 URL 核对。
- Pancake 论文中关于值长度的原句未取得（仅有 Jia et al. 的间接表述）。
- DivORAM、Weighted ORAM 的泄漏分析原句未取得。
- 未能完成 Pantheon / HHG 2013 在 Semantic Scholar / Google Scholar 上 2024–2026 的引用链排查（域被封）；仅通过搜索偶得 Femur (SIGMOD 2025)、Peer2PIR (arXiv 2405.17307)、LetoPIR (ePrint 2026/1814)、Pirouette (PoPETs 2026) 等引用者名字，未核其是否处理变长。
- Signal/MIT private contact discovery、Shroud、ObliFS 未检索。
- 所有带 [待核：搜索摘录] 的引句页码/章节号缺失。
