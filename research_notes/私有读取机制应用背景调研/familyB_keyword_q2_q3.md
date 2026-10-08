# 应用家族 B「私密关键词查找：取回倒排表 / 邻接表等变长列表」—— Q2 问题存在性 与 Q3 占位检查

> 取证说明（重要）：本次会话的网络出口策略封锁了 eprint.iacr.org、iacr.org、usenix.org、ndss-symposium.org、arxiv.org、petsymposium.org、dl.acm.org、springer、semanticscholar、uwaterloo/cypherpunks 等全部论文 PDF 站点（CONNECT 403），仅 GitHub（raw.githubusercontent.com）可达。因此下面的英文"原句"**绝大多数是通过搜索引擎的页面摘录（quoted snippets）转引**，未能在 PDF 内逐页核对；凡未在 PDF 中直接核对的句子，一律标注 **[转引自搜索摘录，页码/章节待核]**。只有 Pantheon README 与 GraphOS README 两处是直接从 GitHub 原文件读取的（标注 **[直接核对]**）。报告撰写者引用时请优先按给出的 URL 自行打开 PDF 复核页码。

---

## Q2 问题存在性：私密关键词查找社区是否明确认定了「读哪条记录敏感 + 记录长度可变（volume）+ 内容外包加密」的场景

### Takeaway
三点均有该社区自己的论文原句支撑：(i) 查询哪个 label/关键词是敏感的（query/access pattern），(ii) 每个 label 对应的值元组长度可变、且 volume 泄漏会被 Kellaris et al. (CCS'16) 等攻击利用——这是 volume-hiding 一族（Kamara–Moataz Eurocrypt'19、Patel et al. CCS'19、Amjad et al. PoPETs'23）的开篇动机句，(iii) 多映射/索引是加密后外包到不可信云服务器的（"cloud-hosted data structures"、"encrypted multi-map"）。keyword PIR 一族（SparsePIR 等）则**显式假设表示长度固定**，Pantheon 声称应支持任意大的 value 但实现按统一 value size 运行；HHG (NDSS'13) 是唯一明确把「变长记录」作为 PIR 问题陈述的论文，但它是多服务器 IT-PIR。

### Cited Findings

**(ii)+(iii) volume 泄漏可致攻击 / 值元组变长 / 外包加密 —— volume-hiding SSE 奠基与后续论文原句**

- Patel–Persiano–Yeo–Yung (CCS 2019, "Mitigating Leakage in Secure Cloud-Hosted Data Structures: Volume-Hiding for Multi-Maps via Hashing") 开篇："Volume leakage has recently been identified as a major threat to the security of cryptographic cloud-based data structures by Kellaris et al. [CCS'16] (see also the attacks in Grubbs et al. [CCS'18] and Lacharité et al. [S&P'18])." [转引自搜索摘录，章节待核：应为 Abstract/§1] — [ePrint 2019/1292](https://eprint.iacr.org/2019/1292.pdf)；[ACM DL](https://dl.acm.org/doi/10.1145/3319535.3354213)
- 同文对 volume-hiding 的定义（搜索摘录转述+部分原句）："A volume-hiding leakage function keeps the number of values for any single key from being revealed, and only the maximum volume of a key is leaked." 以及 Setup 泄漏 "L_Setup = dsize, which is the leakage function that reveals the number n of total values in the multi-map." [转引自搜索摘录，章节待核] — [ePrint 2019/1292](https://eprint.iacr.org/2019/1292.pdf)
- 同文标题本身即把场景定为「安全的云托管数据结构」（"Secure Cloud-Hosted Data Structures"），即内容外包到云服务器 — [ePrint 2019/1292](https://eprint.iacr.org/2019/1292)
- Kamara–Moataz (Eurocrypt 2019, "Computationally Volume-Hiding Structured Encryption"；ePrint 2018/978 原题 "Encrypted Multi-Maps with Computationally-Secure Leakage") 关于朴素填充："the first approach is to pad the tuples of the plaintext multi-map MM to their maximum response length t = max_ℓ #MM[ℓ] and encrypt the padded multi-map with any standard multi-map encryption scheme. It is easy to see that this hides the response lengths. Unfortunately, it also induce a non-trivial storage overhead." [转引自搜索摘录，章节待核：应为 §1 Introduction] — [IACR archive PDF](https://www.iacr.org/archive/eurocrypt2019/114760319/114760319.pdf)；[ePrint 2018/978](https://eprint.iacr.org/2018/978.pdf)
- Kamara–Moataz 贡献句："the first volume-hiding STE schemes that do not rely on naive padding; that is, padding all tuples to the same length." 以及 "leveraging computational assumptions, not just for encryption but, more interestingly, to hide the volumes themselves." 两种构造 "VLH and AVLH, achieve different trade-offs between storage efficiency, query efficiency and lossiness." [转引自搜索摘录] — [IACR archive PDF](https://www.iacr.org/archive/eurocrypt2019/114760319/114760319.pdf)
- Kamara–Moataz 对攻击现实性的保留句（报告时应如实呈现）："while there has been some progress on designing leakage attacks against STE, these attacks remain mostly of theoretical interest due to the strong assumptions they rely on." [转引自搜索摘录] — [IACR archive PDF](https://www.iacr.org/archive/eurocrypt2019/114760319/114760319.pdf)
- Amjad–Patel–Persiano–Yeo–Yung (PoPETs 2023(1), pp. 417–436, "Dynamic Volume-Hiding Encrypted Multi-Maps with Applications to Searchable Encryption") 把「变长响应 → volume 攻击」说得最直白："EMMs allow queries on labels and return the associated value tuple. As responses are variable-length, EMMs are subject to volume leakage attacks introduced by Kellaris et al. [CCS'16]. To prevent these attacks, volume-hiding EMMs were introduced by Kamara and Moataz [Eurocrypt'19] that hide the label volumes (i.e., the value tuple lengths)." [转引自搜索摘录，应为 Abstract/§1] — [PoPETs PDF](https://www.petsymposium.org/popets/2023/popets-2023-0025.pdf)；[ePrint 2021/765](https://eprint.iacr.org/2021/765.pdf)
- 同文列出其认为"必要"的三项泄漏：MM size n、maximum volume ℓ、label equality leakage（两次操作是否针对同一 label），并论证 "hiding the maximum volume ℓ would require Ω(n) communication for any reasonable error probability." [转引自搜索摘录，章节待核] — [ePrint 2021/765](https://eprint.iacr.org/2021/765.pdf)
- Ando–George (PoPETs 2022(4), pp. 44–65, "On the Cost of Suppressing Volume for Encrypted Multi-maps") 下界结论："the first lower bounds on STE schemes for multi-maps that leak at most the query equality pattern. Surprisingly, they find that in many cases, such STE schemes cannot be more storage-efficient than naively padding to the maximum length." [转引自搜索摘录，Abstract] — [PoPETs 页面](https://petsymposium.org/popets/2022/popets-2022-0098.php)；[PDF](https://www.petsymposium.org/popets/2022/popets-2022-0098.pdf)
- Wang–Sun–Li–Qi–Chen (CCS 2022, "Practical Volume-Hiding Encrypted Multi-Maps with Optimal Overhead and Beyond", XorMM)："the client in our scheme receives only ℓ matching results while not suffering from data loss, where ℓ is the maximum volume of all keys"；"the storage cost of XorMM is approximately 1.23n, where n is the total number of key/value pairs." [转引自搜索摘录] — [ACM DL](https://dl.acm.org/doi/10.1145/3548606.3559345)；[作者站 PDF](https://web.xidian.edu.cn/jfwang/files/69046b3630297.pdf)
- 2024 年仍有论文把 query/access/volume 三种 pattern 一并作为目标：Boldyreva–Tang (SCN 2024, "Encrypted Multi-map that Hides Query, Access, and Volume Patterns") "utilizes a position-based ORAM and an encrypted dictionary." [转引自搜索摘录] — [ePrint 2024/2091](https://eprint.iacr.org/2024/2091)

**(i) 查询哪个关键词/顶点是敏感的（query privacy / access pattern）**

- SEAL (USENIX Security 2020)：其两个旋钮之一直接以"隐藏 access pattern 的比特数"为单位——"The adjustable ORAM, ADJ-ORAM-α, hides only α bits of the access pattern by partitioning the accessed N-sized array into N/2^α regions of 2^α size each and by applying an individual standard ORAM per region." [转引自搜索摘录，章节待核：应为 §1/§3] — [USENIX prepub PDF](https://www.usenix.org/system/files/sec20fall_demertzis_prepub.pdf)；[ePrint 2019/811](https://eprint.iacr.org/2019/811.pdf)
- Amjad et al. 2023 将 "label equality leakage (whether two operations are for the same label)" 列为必要泄漏之一，说明查询 label 本身被视为需保护对象 [转引自搜索摘录] — [ePrint 2021/765](https://eprint.iacr.org/2021/765.pdf)
- 加密图侧：SecGraph (arXiv 2403.19531) 评价 GraphSE²："GraphSE2 addresses the protected graph query problem by leveraging a SSE scheme called Oblivious Cross-Tags protocol (OXTs). However, GraphSE2 inherits the leakage from the original OXT protocol and inevitably leaks access patterns." [转引自搜索摘录] — [arXiv 2403.19531](https://arxiv.org/pdf/2403.19531)
- 加密图侧对"顶点度数 = volume"的明确表述（出处待核，来自搜索摘录，疑为 PathGES 或 "Structural Leakage in Graph Encryption" 一文）："for a (lab, valSet) pair in M_1, |valSet| represents the degree of the vertex corresponding to the label lab, which characterizes the number of edges incident to it." [转引自搜索摘录，出处待核] — 候选：[PathGES ePrint 2024/845](https://eprint.iacr.org/2024/845.pdf)；[arXiv 2608.13981](https://arxiv.org/html/2608.13981)
- GraphOS (PVLDB 16(13), 2023) 对已有图加密方案的批评："Opaqe trivially reveals the number of neighbours of each vertex"（并称去除该泄漏需要非常昂贵的填充）[转引自搜索摘录] — [VLDB PDF](https://www.vldb.org/pvldb/vol16/p4324-chamani.pdf)
- 列出云端可学到"顶点邻居数"的句子："The exact vertices of an edge, the labels of an edge, and the number of neighbors of a vertex." [转引自搜索摘录，出处待核：arXiv 2503.10171 或 2405.19259] — [arXiv 2503.10171](https://arxiv.org/html/2503.10171)；[arXiv 2405.19259](https://arxiv.org/pdf/2405.19259)

**keyword PIR 论文中关于 value 大小的原句**

- SparsePIR (Patel–Seo–Yeo, USENIX Security 2023, "Don't be Dense: Efficient Keyword PIR for Sparse Databases", Distinguished Paper)："We assume the representation length is fixed for all (k, v)." [转引自搜索摘录，章节待核] 评测用 "one million 256-byte entries"，响应从 42 KB 降至 21 KB [转引自搜索摘录] — [USENIX PDF](https://www.usenix.org/system/files/usenixsecurity23-patel.pdf)；[ePrint 2023/466](https://eprint.iacr.org/2023/466.pdf)
- Pantheon (Ahmad–Agrawal–El Abbadi–Gupta, PVLDB 16(4), pp. 643–656)："Pantheon should support arbitrarily large values, because hashing a value does not serve the purpose in most cases." [转引自搜索摘录，章节待核] — [VLDB PDF](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)；[ePrint 2023/1331](https://eprint.iacr.org/2023/1331.pdf)
- Pantheon 官方实现 README **[直接核对]**："The key-value store in this case is _public_ and a client cannot encrypt or modify it to ensure privacy."；命令行参数 "-s The size of each value in Bytes." 示例 "each key is `64-bits` and each value is `256 Bytes`" — [GitHub README](https://github.com/ishtiyaque/Pantheon/blob/main/README.md)
- Ali et al. (USENIX Security 2021, "Communication–Computation Trade-offs in PIR")："Finally, we introduce new ways to handle PIR over sparse databases (keyword PIR), based on different hashing techniques." [转引自搜索摘录，Abstract] — [USENIX PDF](https://www.usenix.org/system/files/sec21-ali.pdf)；[ePrint 2019/1483](https://eprint.iacr.org/2019/1483.pdf)
- HHG (Henry–Huang–Goldberg, NDSS 2013)："Multi-block PIR queries naturally lead to PIR queries over variable-length records without padding or other efficiency-harming workarounds." 和 "By decoupling the PIR block size from the lengths of individual database records, we are free to fix the block size to its communication-optimal value" [转引自搜索摘录，Abstract] — [cypherpunks PDF](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)；[NDSS 页面](https://www.ndss-symposium.org/ndss2013/ndss-2013-programme/one-block-size-fits-all-pir-and-spir-variable-length-records-multi-block-queries/)
- HHG 同组 IEEE S&P 2012 海报（"Symmetric PIR"）对变长记录问题的陈述："one cannot generally rely on all records in a database being of a fixed length, nor on the number of records being somehow related to their lengths"；并指出填充+多次查询的代价："it becomes necessary for all users to submit the maximum possible number of queries needed for any record to avoid leaking information." [转引自搜索摘录] — [S&P 2012 poster PDF](https://www.ieee-security.org/TC/SP2012/posters/Symmetric%20PIR.pdf)

### Inferences
- (i)(ii)(iii) 三点在 volume-hiding EMM 一族内是**同时**成立的问题陈述：EMM = 加密后外包的多映射；查询 label 需隐藏（label equality 被视为泄漏）；值元组变长且 volume 被视为可攻击的泄漏。该社区把"volume"定义为每个 label 的值元组长度，与本任务的"倒排表长度"同义。
- keyword PIR 一族（Ali et al. 2021、SparsePIR 2023）把问题定义为**定长**条目的稀疏键空间检索；变长 value 不是其问题陈述的一部分（SparsePIR 明确假设表示长度固定）。Pantheon 口头上要求支持任意大 value，但未见其对变长 value 泄漏的讨论（见 Gaps）。
- 唯一把"变长记录"写进 PIR 问题陈述的是 HHG 2013，但其背景是 Goldberg 的信息论多服务器 PIR。

### Gaps
- 所有 PDF 均因出口策略不可达，未能核对页码/章节/表号；原句以搜索引擎摘录为准，需报告撰写者复核。
- SEAL、Kamara–Moataz、Patel et al. 的**正式泄漏函数**（如 SEAL 定理中的 L 表达式、dprfMM 的 L_Query）未取得原句。
- Pantheon 正文是否对变长 value 作填充/分桶处理：未找到任何句子。
- "Ren et al. Efficient volume-hiding (S&P 2021?)" 未检索到对应论文；"Wang–Chow CCS 2021" 未检索到 volume-hiding 论文——检索到的 Wang–Chow 作品是 PoPETs 2022 的 "Forward and Backward-Secure Range-Searchable Symmetric Encryption"（非 volume-hiding，[待核]），而 CCS 上的 volume-hiding 工作是 Wang–Sun–Li–Qi–Chen CCS 2022 (XorMM)。另有 IFIP SEC 2022 "Efficient Volume-Hiding Encrypted Multi-Maps with Support for Conjunctive Queries"（OXTMM，作者待核）— [Springer](https://link.springer.com/chapter/10.1007/978-3-031-06975-8_27)。

---

## Q3 占位检查：已有哪些系统用 PIR / ORAM / SSE 占据「私密取回变长列表」位置；是否同时满足「变长记录 + 泄漏契约到分组/长度档粒度 + 单服务器」

### Takeaway
最接近的占位者是 **SEAL (USENIX Sec 2020)**：它对每个关键词列表按 x 的幂次填充（泄漏仅为 ⌈log_x N⌉+1 个可观测长度档）、单服务器、SSE+可调 ORAM——三条形式上都沾边，但其机制是加密索引+服务器端分区 ORAM（并非 PIR），且仍泄漏 α-粒度的访问模式与查询等价模式。volume-hiding EMM 一族（KM'19、dprfMM、Amjad'23、XorMM）是单服务器+变长，但其契约是**严格 volume-hiding**（只泄漏 n 与 max ℓ），没有长度档粒度，且查询代价为 max 体积 ℓ。HHG'13 是变长+分块但为**多服务器 IT-PIR**。keyword PIR（Ali'21、SparsePIR'23、Pantheon'23）是单服务器但**定长**。加密图系统（GraphSE²/PeGraph）泄漏度数；GraphOS 用 SGX 双重 oblivious 结构隐藏度数；BlindGES 出现"两个分组大小 l1, l2"的两档泄漏 [待核]。

### Cited Findings（逐系统）

**(1) SEAL — Demertzis, Papadopoulos, Papamanthou, Shintre, USENIX Security 2020**
- 架构句："SEAL is based on two other 'adjustable' primitives, an adjustable ORAM, parameterized by a value α and an adjustable padding algorithm, parameterized by a value x." [转引自搜索摘录] — [USENIX prepub](https://www.usenix.org/system/files/sec20fall_demertzis_prepub.pdf)；[ePrint 2019/811](https://eprint.iacr.org/2019/811.pdf)
- 填充/长度档粒度原句："The adjustable padding algorithm, ADJ-PADDING-x, reduces the volume pattern leakage by padding every list to the closest power of x, leading to a dataset with at most log_x N distinct sizes."（预印本表述为 "⌈log_x N⌉+1" 个可观测大小）；流程为"对每个关键词找到能容纳其列表的最小 x 的幂并填充到该大小，再把整个数据集填充到总大小 x·N" [后半为搜索摘录转述，待核] — 同上
- ORAM 粒度原句："The adjustable ORAM, ADJ-ORAM-α, hides only α bits of the access pattern by partitioning the accessed N-sized array into N/2^α regions of 2^α size each and by applying an individual standard ORAM per region." [转引自搜索摘录] — 同上
- 动机句（搜索摘录转述）：现有 SE 方案泄漏查询结果大小；最简单的修复是把所有关键词列表填充到同一大小 N（最坏情况填充），作者认为代价过高 [转述，待核原句] — 同上
- 权衡句："Larger values for α and x yield slower but more secure SEAL." 示例：隐藏 3 比特访问模式并把列表填充到 4 的幂，相对普通 SE 约 32× 减速 [转引自搜索摘录，数字待核] — 同上
- 记录模型：变长（按关键词列表）；泄漏契约：长度档 = x 的幂次档（分桶级）+ α 比特粒度的访问模式；单服务器：SE 的云服务器模型 [单服务器原句待核；由 SE/ORAM 客户端-服务器模型推断]
- **三条检查**：变长 ✔；长度档粒度 ✔（power-of-x 档）；单服务器 ✔[待核原句]。**但**：机制为 SSE 加密索引 + 服务器侧 2^α 区域 ORAM（非 PIR），且仍泄漏区域级访问模式与查询等价。

**(2) volume-hiding EMM 一族**
- Kamara–Moataz, Eurocrypt 2019："the first volume-hiding STE schemes that do not rely on naive padding"；VLH 有损（"can return false negatives"，可调），AVLH 无损 [转引自搜索摘录] — [IACR archive PDF](https://www.iacr.org/archive/eurocrypt2019/114760319/114760319.pdf)。记录模型：变长值元组；泄漏契约：严格 volume-hiding（隐藏各 label 体积），非分档；单服务器 ✔。**三条检查**：变长 ✔；长度档粒度 ✘（契约为"隐藏体积"而非"泄漏到档"）；单服务器 ✔。
- Patel et al. (dprfMM), CCS 2019："dprfMM, that is the first volume-hiding encrypted multi-map whose storage and query complexity are both asymptotically optimal."；"The query communication and storage complexity of dprfMM is 2ℓ and 2(1+α)n respectively, where 0<α<1 is a small constant."（ℓ = max volume）；Setup 泄漏 "L_Setup = dsize ... reveals the number n of total values"；构造为带 stash 的 cuckoo hashing [转引自搜索摘录] — [ePrint 2019/1292](https://eprint.iacr.org/2019/1292.pdf)。另有差分隐私变体 dpMM："query cost is the volume of the queried key plus an additional logarithmic factor" [转述，待核]。**三条检查**：变长 ✔；长度档粒度 ✘（只泄漏 n 与 max ℓ；每次查询代价固定为 2ℓ）；单服务器 ✔。
- Amjad et al., PoPETs 2023："the first fully dynamic volume-hiding EMMs that are both asymptotically and concretely efficient"，"simultaneously forward and backward private"；必要泄漏 = n、ℓ、label equality [转引自搜索摘录] — [PoPETs PDF](https://www.petsymposium.org/popets/2023/popets-2023-0025.pdf)。**三条检查**：同 dprfMM（分档 ✘）。
- Wang–Sun–Li–Qi–Chen (XorMM), CCS 2022：客户端 "receives only ℓ matching results"，存储 ≈1.23n [转引自搜索摘录] — [ACM DL](https://dl.acm.org/doi/10.1145/3548606.3559345)。**三条检查**：分档 ✘（严格 volume-hiding，代价 max ℓ）。
- Ando–George, PoPETs 2022（下界）："in many cases, such STE schemes cannot be more storage-efficient than naively padding to the maximum length." [转引自搜索摘录] — [PoPETs](https://petsymposium.org/popets/2022/popets-2022-0098.php)。此为对"只泄漏查询等价"契约的代价证明，不是系统。
- Boldyreva–Tang, SCN 2024（ORAM+加密字典，隐藏 query/access/volume）[转引自搜索摘录] — [ePrint 2024/2091](https://eprint.iacr.org/2024/2091)。单服务器 ORAM；契约为全隐藏，分档 ✘ [细节待核]。
- 其他后续（仅标题级，未读）：d-DSE (USENIX Sec 2024) — [prepub](https://www.usenix.org/system/files/sec24fall-prepub-699-liu-dongli.pdf)；Veil (arXiv 2310.12491 / ACM 3626759)；Flash/BF²CMM (IEEE TIFS 2025)；"Beyond Access Pattern: Efficient Volume-Hiding Multi-Range Queries" (TIFS 20, 2025, p.2509)。

**(3) HHG — Henry, Huang, Goldberg, NDSS 2013 "One (Block) Size Fits All"**
- 变长记录模型原句："Multi-block PIR queries naturally lead to PIR queries over variable-length records without padding or other efficiency-harming workarounds."；"By decoupling the PIR block size from the lengths of individual database records, we are free to fix the block size to its communication-optimal value" [转引自搜索摘录，Abstract] — [cypherpunks PDF](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
- 多服务器属性："The new multi-block IT-PIR trades off some Byzantine robustness to improve throughput without affecting user privacy."；方法建立在 "Goldberg's information-theoretic PIR scheme" 之上（Goldberg 2007 IT-PIR 为 ℓ 服务器、t-private 的多服务器方案） [转引自搜索摘录；"ℓ servers" 原句待核] — 同上；[NDSS 页面](https://www.ndss-symposium.org/ndss2013/ndss-2013-programme/one-block-size-fits-all-pir-and-spir-variable-length-records-multi-block-queries/)
- 泄漏/块数描述（搜索摘录转述）：用户在单次查询中取回多个 b 比特块，"without revealing either the number of blocks fetched or the index of any block" [转引，待核原句]；海报版说明传统做法需所有用户提交"the maximum possible number of queries needed for any record to avoid leaking information" — [S&P 2012 poster](https://www.ieee-security.org/TC/SP2012/posters/Symmetric%20PIR.pdf)
- 四个 SPIR 构造支持变长记录，三个可配零知识证明用于定价/访问控制 [转引自搜索摘录]
- **三条检查**：变长 ✔（分块+多块查询，隐藏块数）；长度档粒度：其目标是隐藏块数（即隐藏长度），非分档 [待核正文是否有分档讨论]；单服务器 ✘（IT-PIR，多服务器）。

**(4) keyword PIR / sparse PIR 系统**
- Ali et al., USENIX Sec 2021：keyword PIR 通过 cuckoo hashing 把稀疏键映射到稠密域，"guarantees at most one element per bucket at the cost of a constant number of extra client queries" [转述，待核]；单服务器（SealPIR/MulPIR 系）[转引自搜索摘录] — [USENIX PDF](https://www.usenix.org/system/files/sec21-ali.pdf)。value 大小：未见变长讨论。**三条检查**：变长 ✘（未支持/未讨论）；分档 ✘；单服务器 ✔。
- SparsePIR, USENIX Sec 2023："We assume the representation length is fixed for all (k, v)."；编码大小 (1+ε)n vs cuckoo 的 (2+ε)n；单服务器 [转引自搜索摘录] — [USENIX PDF](https://www.usenix.org/system/files/usenixsecurity23-patel.pdf)。**三条检查**：变长 ✘（显式固定）；分档 ✘；单服务器 ✔。
- Pantheon, PVLDB 2023："Pantheon should support arbitrarily large values, because hashing a value does not serve the purpose in most cases." [转引自搜索摘录]；实现以统一 value size 运行（"-s The size of each value in Bytes." **[直接核对]**）— [VLDB PDF](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)；[GitHub README](https://github.com/ishtiyaque/Pantheon/blob/main/README.md)。**三条检查**：变长：声明支持大 value 但每次运行统一尺寸，是否变长/如何泄漏长度 [待核]；分档 ✘（未见）；单服务器 ✔。
- 其他 keyword PIR（标题级）：Constant-weight PIR (USENIX Sec 2022, Mahdavi) — [PDF](https://www.usenix.org/system/files/sec22fall_mahdavi.pdf)；ChalametPIR "Call Me By My Name" (CCS 2024) — [ACM](https://dl.acm.org/doi/10.1145/3658644.3670271)；"Practical Keyword PIR from Key-to-Index Mappings" (ePrint 2025/210)；KPIR-C (ePrint 2025/1952)。value 变长性均未核。

**(5) 加密图邻接表查询系统**
- GraphSE² (Lai, Yuan, Liu 等, ACM AsiaCCS 2019)："GraphSE² provides an encrypted structural data model to facilitate parallel and encrypted graph data access." [转引自搜索摘录] — [Semantic Scholar](https://www.semanticscholar.org/paper/7904b3446775ed8c79f4f94001a16b706989c462)；[arXiv 1905.04501](https://arxiv.org/abs/1905.04501)。泄漏（他文评价）："inherits the leakage from the original OXT protocol and inevitably leaks access patterns." — [SecGraph arXiv 2403.19531](https://arxiv.org/pdf/2403.19531)。**三条检查**：变长 ✔（邻接表）；分档 ✘（OXT 泄漏结果大小/访问模式）；单服务器 ✔。
- PeGraph：用 TSet（加密多映射）+ XSet 支持合取；SecGraph 批评其 "suffers from expensive computation and result pattern leakages" [转引自搜索摘录] — [arXiv 2403.19531](https://arxiv.org/pdf/2403.19531)。**三条检查**：分档 ✘。
- SecGDB (FC 2017)："Padding carefully designed fake entries to each original adjacency list during the generation of the encrypted graph. Hence, some leakage information (i.e., the accurate number of edges in the original graph and the adjacency information of each vertex) can be mitigated." [转引自搜索摘录] — [FC17 PDF](https://fc17.ifca.ai/preproceedings/paper_65-2.pdf)。填充粒度未核。
- GraphOS (Chamani, Demertzis, Papadopoulos, Papamanthou, Jalili, PVLDB 16(13):4324–4338, 2023)：批评 "Opaqe trivially reveals the number of neighbours of each vertex"；服务器 "initially learns only the number of vertices and edges" [转引自搜索摘录] — [VLDB PDF](https://www.vldb.org/pvldb/vol16/p4324-chamani.pdf)。实现为 SGX 上的 "doubly oblivious data structures"（Oblix++/Omix++）**[直接核对 README]** — [GitHub](https://github.com/jgharehchamani/graphos)。**三条检查**：变长 ✔；分档 ✘（目标为全隐藏）；单服务器 ✔（但依赖 TEE，非 PIR）。
- "A Privacy-Preserving Graph Encryption Scheme Based on Oblivious RAM" (arXiv 2405.19259, 2024)：ORAM + SGX 降低 access/query pattern 泄漏 [转引自搜索摘录] — [arXiv](https://arxiv.org/pdf/2405.19259)。细节未核。
- BlindGES（疑为 arXiv 2503.10171 "Verifiable, Efficient and Confidentiality-Preserving Graph Search with Transparency"）：有两档分组的表述——"There are only two values for the size of all ciphertexts, which correspond to the two grouping sizes l_1 and l_2 in the segmentation strategy of BlindGES." [转引自搜索摘录，出处待核] — [arXiv 2503.10171](https://arxiv.org/html/2503.10171)。**这是加密图侧最接近"按长度档分组、泄漏档位"的表述**，但仅两档，且是否单服务器/是否 PIR 未核。
- 关于填充到最大的代价："Padding intermediate results to the maximum size would eliminate this leakage but with prohibitive performance downgrade (quadratic in the graph size)." [转引自搜索摘录，出处待核：GraphOS 或 PathGES] — [PathGES ePrint 2024/845](https://eprint.iacr.org/2024/845.pdf)
- 2026 年综述式攻防论文 "Structural Leakage in Graph Encryption: Attacks and Defenses" (arXiv 2608.13981) 存在，内容未读 — [arXiv](https://arxiv.org/html/2608.13981)

### 三条件汇总表（✔/✘/待核）

| 系统 | venue/年 | 记录模型 | 变长处理 | 泄漏契约粒度 | 单服务器 | 三条同时满足？ |
|---|---|---|---|---|---|---|
| SEAL | USENIX Sec 2020 | 关键词→列表，变长 | 每列表填充到最近 x 的幂；总量填充到 x·N | 长度档（≤⌈log_x N⌉+1 档）+ α 比特访问模式 | ✔[原句待核] | 形式上 ✔✔✔，但机制为 SSE+分区 ORAM，非 PIR；仍泄漏区域级访问模式 |
| Kamara–Moataz VLH/AVLH | Eurocrypt 2019 | EMM，变长 | 计算性隐藏体积（VLH 有损） | 严格 volume-hiding | ✔ | ✘ 缺"分档粒度"（契约更强，代价不同） |
| dprfMM / dpMM | CCS 2019 | EMM，变长 | cuckoo hashing + stash | 泄漏 n 与 max ℓ；查询 2ℓ | ✔ | ✘ 缺分档 |
| Amjad et al. 动态 VH | PoPETs 2023 | 动态 EMM | — | n、ℓ、label equality | ✔ | ✘ 缺分档 |
| XorMM | CCS 2022 | EMM | XOR 过滤器 | 严格 VH，代价 ℓ | ✔ | ✘ 缺分档 |
| HHG multi-block | NDSS 2013 | 变长记录分块 | 多块查询隐藏块数 | 隐藏块数/长度 | ✘ 多服务器 IT-PIR | ✘ 缺单服务器 |
| Ali et al. | USENIX Sec 2021 | 定长条目 | cuckoo 稀疏→稠密 | 标准 PIR（无泄漏）| ✔ | ✘ 缺变长 |
| SparsePIR | USENIX Sec 2023 | "representation length is fixed" | 线性编码 | 标准 PIR | ✔ | ✘ 缺变长 |
| Pantheon | PVLDB 2023 | 公开 KV，统一 value size/运行 | [待核] | 标准 PIR | ✔ | 变长 [待核]；分档 ✘ |
| GraphSE² / PeGraph | AsiaCCS 2019 / [待核] | 邻接表，变长 | 无（OXT） | 泄漏访问/结果模式（度数） | ✔ | ✘ 缺分档契约 |
| GraphOS | PVLDB 2023 | 邻接表 | SGX 双重 oblivious | 仅 |V|,|E| | ✔（TEE） | ✘ 非 PIR/非分档 |
| BlindGES | arXiv 2025 [待核] | 邻接表 | 两档分组 l1,l2 | 两档 | [待核] | [待核] |

### Inferences
- 在「变长 + 长度档粒度契约 + 单服务器」三条上，SEAL 的 ADJ-PADDING-x 与本任务"按 2^k 容量档分组信封"的粒度**同构**（x=2 即为 2^k 档）；差别在 SEAL 用服务器侧分区 ORAM 处理访问模式并额外泄漏 α 粒度访问模式，而非对公开信封做 PIR。报告撰写者应把 SEAL 作为最近邻占位者如实呈现（不下新颖性结论）。
- volume-hiding EMM 一族选择了比"分档"更强的契约（只泄漏 n、max ℓ），其代价是每次查询传输 max ℓ（dprfMM 为 2ℓ），Ando–George 证明这类契约在许多情形下存储不能优于填充到最大。
- keyword PIR 一族的定长假设意味着它们未占据"变长列表"位置；若要用它们取变长列表需在其上叠加分块/分档策略，而这一叠加在所查文献中未见被写成泄漏契约。

### Gaps
- 未能直接读取任何一篇目标论文 PDF（出口封锁），所有章节/页码/表号缺失；SEAL 的正式泄漏定理、dprfMM 的 L_Query、HHG 的 ℓ-server 模型句、GraphSE²/PeGraph 的 L_S/L_Q 定义均需复核。
- PeGraph 的 venue/年份未核。
- Pantheon 正文是否讨论变长 value 的长度泄漏：未找到。
- "Ren et al. (S&P 2021?)" 与 "Wang–Chow CCS 2021 volume-hiding" 未找到对应论文（见 Q2 Gaps）。
- 2022–2026 的 SEAL 后续（adjustable leakage 系列）未专门检索；仅间接看到 d-DSE (USENIX Sec 2024) 与 S-Leak 等引用 SEAL。
- BlindGES 的出处、是否单服务器、l1/l2 分组的泄漏定义均待核。
