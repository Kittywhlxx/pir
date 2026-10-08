# Q1 接纳证据：VLDB（PVLDB Vol.17–19）与 SIGMOD（PACMMOD Vol.2–4）2024–2026 年以密码组件为机制的应用型论文

> 检索环境说明（影响可核验性）：本次会话的网络代理封锁了 dblp.org / dblp.uni-trier.de / vldb.org / dl.acm.org / 2024–2026.sigmod.org / arxiv.org / eprint.iacr.org / api.semanticscholar.org / api.openalex.org / rmarcus.info(dbscholar) 等全部一手目录站点（WebFetch 返回 EGRESS_BLOCKED，curl 返回 CONNECT 403）。
> 可用来源仅两类：(1) WebSearch 结果中引用的一手页面片段（vldb.org PDF、dl.acm.org 条目、ePrint、作者主页等）；(2) 社区维护的会议论文清单仓库 `RealZST/csconf-papers`（自述数据源为 DBLP 元数据，经 raw.githubusercontent.com 可读）——该仓库仅有 2025、2026 两个年份目录，**没有 2024 目录**，因此 PVLDB Vol.17 与 PACMMOD Vol.2 的覆盖只能依靠 WebSearch，属于不完整覆盖（见各节 Gaps）。
> 所有"题目 / 卷期 / DOI"字段均来自上述可核来源并附 URL；凡原文未读到的"信任模型 / 证明 / 实验类型"字段一律标 [待核]，不做填补。

---

## KQ1：各 venue-年份录用了哪些以 PIR / SSE / ORAM / HE / MPC / TEE+oblivious 为机制、以应用问题为主语的论文？

### Takeaway
2025–2026 两年（PVLDB Vol.18–19、PACMMOD Vol.3–4）的标题级筛查是完整的（基于 csconf-papers 的 DBLP 镜像清单，分别 483 / 445 / 251 / 354 篇），共甄别出 **约 20 篇**符合"密码组件 + 应用主语"的正式录用论文，外加 4 篇 demo/tutorial；其中与候选家族 A/B/C 直接相关的核心论文为 **Femur（SIGMOD'25）、Towards Practical Oblivious Map（VLDB'25）、OasisDB（VLDB'25）、Doc*（VLDB'25）、OBIR-tree（SIGMOD'25）、Grove/Index-free Adjacency Oblivious Graph（VLDB'26）、Workload-Aware Encrypted Index（VLDB'26）、NSHEDB（SIGMOD'26）、Loom（SIGMOD'26）、HADES（VLDB'25）**。2024 年（PVLDB Vol.17 / PACMMOD Vol.2）只能部分覆盖，确认到 SWAT、SecuDB、SecretFlow-SCQL、FedSQ（PVLDB 17）与 Object-oriented Unified Encrypted Memory Management、QueryShield(demo)（SIGMOD'24）。

### Cited Findings

#### A. PVLDB Vol.17（VLDB 2024）—— 覆盖不完整（无目录页可读；仅 WebSearch 命中）

- **SWAT: A System-Wide Approach to Tunable Leakage Mitigation in Encrypted Data Stores**（Leqian Zheng, Lei Xu, Cong Wang, Sheng Wang, Yuke Hu, Zhan Qin, Feifei Li, Kui Ren）。PVLDB 17(10): 2445–2458, 2024, DOI 10.14778/3675034.3675038。机制：SGX enclave + Pancake 式频率平滑 + 自定义 oblivious 算子；工作负载为 key-value、range、dynamic。原句："provably mitigating system-wide leakage via a suite of algorithms with tunable privacy-efficiency trade-offs"；"Keys and equal-length values are protected by pseudorandom functions and authenticated encryption"。承重实验：C++ 客户端/服务器原型跑在 Intel SGXv2 上，合成数据集，报告"about 10.6× slower than an encryption-only data store ... and is 31.6× faster than a trivial zero-leakage solution"。— [vldb.org PDF](https://www.vldb.org/pvldb/vol17/p2445-zheng.pdf)；[arXiv 2306.16851](https://arxiv.org/abs/2306.16851)
- **SecuDB: An In-enclave Privacy-preserving and Tamper-resistant Relational Database**（Xinying Yang, Cong Yue, Wenhui Zhang, Yang Liu, Beng Chin Ooi, Jianjun Chen）。PVLDB 17(12): 3906–3919, 2024。机制：整个 RDBMS 跑在 Intel TDX 内，列/日志/统计掩码 + TEE 背书的 temporal table；**不做访问模式隐藏**。实验：吞吐相对明文达 84.7% / 94.7%，防篡改开销 <22.6%。— [vldb.org PDF](https://www.vldb.org/pvldb/vol17/p3906-yang.pdf)
- **SecretFlow-SCQL: A Secure Collaborative Query pLatform**（Wenjing Fang 等，Ant Group）。PVLDB 17(12): 3987–4000, 2024。机制：MPC（秘密分享）协同分析，重点是 oblivious sort；实验：1M 个 64-bit 值安全排序约 4.5 分钟，比 EMP 基线快 126×。— [vldb.org PDF](https://www.vldb.org/pvldb/vol17/p3987-fang.pdf)
- **FedSQ: A Secure System for Federated Vector Similarity Queries**（Zeqi Zhu, Zeheng Fan, Yuxiang Zeng, Yexuan Shi, Yi Xu, Mengmeng Zhou, Jin Dong）。PVLDB 17(12): 4441–4444, 2024, DOI 10.14778/3685800.3685895。4 页，"prototype system"，**疑为 demo track [待核]**。机制：MPC 做跨数据所有者的局部向量检索结果安全聚合。— [vldb.org PDF](https://www.vldb.org/pvldb/vol17/p4441-zhu.pdf)；[BUAA 条目](https://research.buaa.edu.cn/en/publications/fedsq-a-secure-system-for-federated-vector-similarity-queries/)
- 边界项（年份归属需注意）：**Cryptographically Secure Private Record Linkage Using Locality-Sensitive Hashing**（Wei, Kerschbaum）位于 vol17 p79（Vol.17 前几期于 2023 年 9–12 月出版），搜索结果中引用标为 2023 — [vldb.org PDF](https://www.vldb.org/pvldb/vol17/p79-kerschbaum.pdf) [年份待核]。
- 未在 Vol.17 中搜到任何以 PIR 为机制的论文；搜索中唯一的 PIR 命中是 Pantheon（Vol.16）。— [WebSearch 结果引用 vldb.org/pvldb/vol16/p643-ahmad.pdf](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)

#### B. PACMMOD Vol.2（SIGMOD 2024）—— 覆盖不完整

- **Object-oriented Unified Encrypted Memory Management for Heterogeneous Memory Architectures**（Mo Sha 等，Alibaba / UPenn / NUS）。被 Femur 参考文献列为 Proc. ACM Manag. Data 2(3), Article 155, 2024；SIGMOD 2024 程序表列于 "Session 7: Data Quality (2) + Security (1)"。机制与应用主语 [待核]（标题指向 TEE/加密内存管理，而非查询应用）。— [SIGMOD 2024 程序页（被代理封锁，仅由搜索摘要引用）](https://2024.sigmod.org/program_sigmod.shtml)
- **QueryShield: Cryptographically Secure Analytics in the Cloud**（Boston University / UC Santa Cruz）—— **demo**，MPC 分析。— [SIGMOD 2024 程序页](https://2024.sigmod.org/program_sigmod.shtml)
- 同场次出现但**不计入**（PACMMOD 卷期为 2023）：**Waffle: An Online Oblivious Datastore for Protecting Data Access Patterns**（Maiyya, Vemula, Agrawal, El Abbadi, Kerschbaum），ACM DL 日期 2023-12-12，DOI 10.1145/3626760，BibTeX series SIGMOD '23；ePrint 2023/1285 却标 "SIGMOD 2024"（来源冲突）。— [ePrint 2023/1285](https://eprint.iacr.org/2023/1285)；[作者 PDF](https://cs.uwaterloo.ca/~smaiyya/assets/papers/waffle.pdf)
- 同场次不计入（PACMMOD 1(4), Dec 2023）：**TEE-based General-purpose Computational Backend for Secure Delegated Data Processing**（Sha, Li, Wang, Li, Tan），Article 263。— [NUS PDF](https://www.comp.nus.edu.sg/~lijl/papers/tgcb-sigmod24.pdf)
- 同样不计入（PACMMOD 1(4), 2023）：**Hiding Access-pattern is Not Enough! Veil: A Storage and Communication Efficient Volume-Hiding Algorithm**。— [arXiv 2310.12491](https://arxiv.org/pdf/2310.12491)
- Session 15 "Data Security (2)" 已知成员 DProvDB、DP-starJ 为纯差分隐私，按约束排除；该场次其余论文搜索摘要截断未见 [待核]。— [SIGMOD 2024 程序页](https://2024.sigmod.org/program_sigmod.shtml)

#### C. PVLDB Vol.18（VLDB 2025）—— 标题级完整筛查（csconf-papers 清单 483 篇，含全卷）

- **Towards Practical Oblivious Map**（Xinle Cao, Weiqi Feng, Jian Liu, Jinjin Zhou, Wenjing Fang, Lei Wang, Quanqing Xu, Chuanhui Yang, Kui Ren）。PVLDB 18(3): 688–701, DOI 10.14778/3712221.3712235；ePrint 2024/1650。应用主语：客户端访问外包加密 KV 存储而不泄露访问模式（OMAP）；混合哈希表+搜索树，提出去摊销 ORAM "DAORAM"；复杂度从 OMIX++ 的 O(log n) 轮 / O(log² n) 带宽降至 O(log n / log log n)+O(log λ) 轮。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p688-liu.pdf)；[ePrint 2024/1650](https://eprint.iacr.org/2024/1650)；[ACM DL](https://dl.acm.org/doi/10.14778/3712221.3712235)
- **SPECIAL: Synopsis Assisted Secure Collaborative Analytics**（Chenghong Wang, Lina Qiu, Johes Bater, Yukui Luo）。PVLDB 18, p1035；MPC 协同分析。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p1035-wang.pdf)
- **Jodes: Efficient Oblivious Join in the Distributed Setting**（Yilei Wang, Xiangdong Zeng, Sheng Wang, Feifei Li，Alibaba Cloud）。PVLDB 18(5): 1291–1304, DOI 10.14778/3718057.3718060。TEE + oblivious，多服务器，"reveals only the input sizes and output size"；16 服务器实验最高 6× 提升。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p1291-wang.pdf)；[arXiv 2501.09334](https://arxiv.org/pdf/2501.09334)
- **HADES: Range-Filtered Private Aggregation on Public Data**（Xiaoyuan Liu, Ni Trieu, Trinabh Gupta, Ishtiyaque Ahmad, Dawn Song）。PVLDB 18(7): 2159–2171, DOI 10.14778/3734839.3734852。原句："a fully homomorphic encryption (FHE) based private aggregation system for public data that supports point, range predicates, and boolean combinations"；对比先前方案"either require additional setups (e.g., non-colluding servers, hardware enclaves) or are inefficient"；端到端 TPC-H 相比一轮 FHE 方案提升 204×–6574×。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p2159-liu.pdf)；[DOI](https://doi.org/10.14778/3734839.3734852)
- **Alchemy: A Query Optimization Framework for Oblivious SQL**（Donghyun Sohn, Kelly Jiang, Nicolas Hammer, Jennie Rogers）。PVLDB 18, p3021。机制 [待核]（该组前作 RESCU-SQL / smcql 为 MPC）。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p3021-sohn.pdf)
- **Access Control for Information-Theoretically Secure Data**（系统名 Doc*；Yin Li, Sharad Mehrotra, Shantanu Sharma, Komal Kumari）。PVLDB 18(10): 3243–3255, DOI 10.14778/3748191.3748192（arXiv 版题为 "Access Control for Information-Theoretically Secure Key-Document Stores"，ePrint 2025/1457 题为 "DOC★: ..."）。应用主语："secure outsourcing key-value stores where values correspond to documents that are indexed and accessed using keys"；机制 Shamir 秘密分享（信息论安全）；隐藏数据、用户访问权限与输出大小；威胁模型原句："a minority of the servers can also be malicious ... malicious servers may collude amongst themselves and/or with the clients"；实验：231.5 ms over 5,000 keywords across 500,000 files。代码 github.com/SecretDeB/DocStar-VLDB-2025。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p3243-sharma.pdf)；[DOI](https://doi.org/10.14778/3748191.3748192)；[arXiv 2507.10730](https://arxiv.org/abs/2507.10730)；[ePrint 2025/1457](https://eprint.iacr.org/2025/1457.pdf)
- **GORAM: Graph-oriented ORAM for Efficient Ego-centric Queries on Federated Graphs**（Xiaoyu Fan, Kun Chen, Jiping Yu, Xiaowei Zhu, Yunyi Chen, Huanchen Zhang, Wei Xu）。PVLDB 18(10): 3601–3614, DOI 10.14778/3748191.3748218；arXiv 2410.02234。机制：MPC 上的 ORAM 风格索引（代码仓库名 GORAM-ABY3，暗示 3 方 ABY3 [待核]）；"no information about the graphs or the querying keys is exposed"；作者自称"first instance of processing billion-scale graphs with practical performance on MPC"；第三方摘要称十亿边图上延迟 58 ms–35.7 s（未核）。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p3601-fan.pdf)；[arXiv 2410.02234](https://arxiv.org/abs/2410.02234v1)
- **OasisDB: An Oblivious and Scalable System for Relational Data**（Haseeb Ahmed, Nachiket Rao, Abdelkarim Kati, Florian Kerschbaum, Sujaya Maiyya）。PVLDB 18(11): 4478–4491, DOI 10.14778/3749646.3749707；ePrint 2025/1263。应用主语：在 oblivious KV-store（PathORAM、Waffle）之上构建隐藏访问模式与 volume 模式的关系型系统；支持点查、范围、join、聚合、有限更新；Epinions 数据集上随机器数线性扩展；对比 CockroachDB（明文）、ObliDB、Obliviator。代码 github.com/dsg-uwaterloo/ObliSQL。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p4478-ahmed.pdf)；[ePrint 2025/1263](https://eprint.iacr.org/2025/1263)；[GitHub README](https://github.com/dsg-uwaterloo/ObliSQL)
- **LETIndex: A Secure Learned Index with TEE**（Shuting Cao, Zeping Niu, Guoliang Li）。PVLDB 18(12), p5403。机制 TEE；是否 oblivious [待核]。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p5403-li.pdf)
- **Sectric: Towards Accurate, Privacy-preserving and Efficient Triangle Counting**（Minze Xu 等）。PVLDB 18, p3382。机制 [待核]。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p3382-xu.pdf)
- **PrivAGM: Secure Construction of Differentially Private Directed Attributed Graph Models on Decentralized Social Graphs**（Songlei Wang, Yifeng Zheng, Xiaohua Jia, Haibo Hu）。PVLDB 18, p4682。机制 [待核]（标题暗示 MPC+DP）。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p4682-zheng.pdf)
- **FedVSE: A Privacy-Preserving and Efficient Vector Search Engine for Federated Databases**（Zeheng Fan, Yuxiang Zeng, Zhuanglin Zheng, Yongxin Tong）—— **demo**，PVLDB 18(12): 5371–5374, DOI 10.14778/3750601.3750674。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p5371-tong.pdf)；[ACM DL](https://dl.acm.org/doi/10.14778/3750601.3750674)
- **Demonstration of Reflex: How SMPC Query Execution can be sped up through Efficient and Flexible Intermediate Result Size Trimming**（Long Gu, Shaza Zeitouni, Carsten Binnig, Zsolt István）—— **demo**，PVLDB 18, p5311。— [vldb.org PDF](https://www.vldb.org/pvldb/vol18/p5311-gu.pdf)
- 清单来源：[csconf-papers papers/2025/VLDB.md（raw）](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2025/VLDB.md)（页首注明"483 papers · updated 2026-10-01"，并说明文件覆盖 PVLDB vol 18 全卷）。

#### D. PVLDB Vol.19（VLDB 2026，截至 2026-10-01 已发表部分）—— 标题级完整筛查（csconf-papers 清单 445 篇）

- **A Workload-Aware Encrypted Index for Efficient Privacy-Preserving Range Queries**（Dong Wang, Ningning Cui, Jianxin Li, Jianzhong Qi, Jianliang Xu, Hui Lu）。PVLDB 19(4): 740–752, DOI 10.14778/3785297.3785313。机制：XOR 加密位图索引 + 基于 PRF 的轻量比较协议（P³RQ-Bitmap）、自适应分区的加密树索引（P³RQ-WBTree）；声称查询至少快 83×。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p740-li.pdf)；[ACM DL](https://dl.acm.org/doi/10.14778/3785297.3785313)；[HKBU 条目](https://scholars.hkbu.edu.hk/en/publications/a-workload-aware-encrypted-index-for-efficient-privacy-preserving/)
- **Bifrost: A Much Simpler Secure Two-Party Data Join Protocol for Secure Data Analytics**（Shuyu Chen, Mingxun Zhou, Haoyu Niu, Guopeng Lin, Weili Han）。PVLDB 19(6): 1156–1169, DOI 10.14778/3797919.3797925。两方 MPC（ECDH-PSI + 两方 oblivious shuffle），"reveals only the size of the joined table"。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p1156-han.pdf)；[ACM DL](https://dl.acm.org/doi/10.14778/3797919.3797925)
- **Secure Multi-Party Sampling over Joins**（Qiyao Luo, Quanqing Xu, Chuanhui Yang）。PVLDB 19, p1455。MPC。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p1455-luo.pdf)
- **Enabling Index-free Adjacency in Oblivious Graph Processing with Delayed Duplications**（Weiqi Feng, Xinle Cao, Adam O'Neill, Chuanhui Yang；系统名 Grove）。PVLDB 19, p1826；ePrint 2025/2047（页面注明"full version ... to appear in VLDB 2026"，状态 Major revision）。应用主语：oblivious 图数据库支持 index-free adjacency（邻居查询、t-hop 查询）；对比 GraphOS 与 AHR 协议。代码：github.com/WeiqiNs/DAORAM。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p1826-cao.pdf)；[ePrint 2025/2047](https://eprint.iacr.org/2025/2047)
- **Secure Join Operations in Multi-Identifier Databases: Performance and Practicality**（Wen-Jie Lu 等）。PVLDB 19, p1854。机制 [待核]（作者群为 MPC/HE 方向）。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p1854-lu.pdf)
- **V3DB: Audit-on-Demand Zero-Knowledge Proofs for Verifiable Vector Search over Committed Snapshots**（Zipeng Qiu, Wenjie Qu, Jiaheng Zhang, Binhang Yuan）。PVLDB 19, p2604；arXiv 2603.03065。ZK 验证向量检索（完整性而非机密性）。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p2604-qiu.pdf)
- **Doppio: Communication-Efficient and Secure Multi-Party Shuffle Differential Privacy**（Wentao Dong, Yang Cao, Cong Wang, Wei-Bin Lee）。PVLDB 19, p113。DP + MPC shuffle。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p113-dong.pdf)
- **Efficient and Secure Range Counting over Distributed Geographic Data with Query Range Protection**（Haoxin Yang 等）。PVLDB 19, p3317。机制 [待核]。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p3317-yang.pdf)
- **Fides: Secure and Scalable Asynchronous DAG Consensus via Trusted Components**（p3302）与 **HarborMaster: Rollback Detection for Trusted Distributed Computing**（p2126）：TEE 系统论文，非查询应用。— [Fides PDF](https://www.vldb.org/pvldb/vol19/p3302-xie.pdf)；[HarborMaster PDF](https://www.vldb.org/pvldb/vol19/p2126-mishra.pdf)
- **Private Data Collections: Enabling Data Privacy in Permissioned Blockchains**（Natarajan 等，Hyperledger）。PVLDB 19, p4263。track [待核]（疑 industrial）。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p4263-natarajan.pdf)
- **Private LLM Inference with Homomorphic Encryption**（Lawrence Lim, Divyakant Agrawal, Amr El Abbadi）。PVLDB 19, p4927 —— **tutorial**（同题 PDF 亦见于 EDBT 2026 tutorial 路径）。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p4927-lim.pdf)；[openproceedings EDBT 2026](https://openproceedings.org/2026/conf/edbt/paper-T1.pdf)
- **Demonstration of Doc*: Access Control for Information-Theoretically Secure Data**（p4810）—— **demo**。— [vldb.org PDF](https://www.vldb.org/pvldb/vol19/p4810-sharma.pdf)
- 清单来源：[csconf-papers papers/2026/VLDB.md（raw）](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2026/VLDB.md)（"445 papers · updated 2026-10-01"）。

#### E. PACMMOD Vol.3（SIGMOD 2025）—— 标题级完整筛查（csconf-papers 清单 251 篇）

- **Femur: A Flexible Framework for Fast and Secure Querying from Public Key-Value Store**（Jiaoyi Zhang, Liqiang Peng, Mo Sha, Weiran Liu, Xiang Li, Sheng Wang, Feifei Li, Mingyu Gao, Huanchen Zhang；清华 + 阿里）。Proc. ACM Manag. Data 3(3) (SIGMOD), Article 162, DOI 10.1145/3725299；arXiv 2503.05376。应用主语：公开 KV 库上的私密点查；机制：PIR + 可调松弛（"distance-based indistinguishability"，灵感来自 LDP，离散 Laplace 噪声扩展位置区间）+ 学习索引（PGM）键到位置转换 + "a novel variable-range PIR scheme optimized for bandwidth-limited settings"；高带宽模式明文下载区间 O(s) 传输，低带宽模式 PIR 传输 O(1)/计算 O(s)。原文将 Pantheon 列为相关工作："Pantheon [7] and Constant-weight PIR [57] implement equality-checking operators and invoke traditional PIR schemes"。第三方摘要称吞吐提升最高 163.9×（未核）。— [ACM DL](https://dl.acm.org/doi/10.1145/3725299)；[arXiv 2503.05376](https://arxiv.org/abs/2503.05376v1)；[作者 PDF](https://people.iiis.tsinghua.edu.cn/~gaomy/pubs/femur.sigmod25.pdf)
- **OBIR-tree: An Efficient Oblivious Index for Spatial Keyword Queries on Secure Enclaves**（Zikai Ye, Xiangyu Wang, Zesen Liu, Dan Zhu, Jianfeng Ma，西电）。Proc. ACM Manag. Data 3(1) (SIGMOD), Article 58, DOI 10.1145/3709708；arXiv 2509.05755。应用主语：加密数据上的 oblivious top-k 空间关键词查询；机制：IR-tree + Path ORAM，扩展为 Intel SGX 内运行 + Real/Dummy block Tree；隐藏 search / access / volume pattern；效率比基线高 25×–723×。— [ACM DL（via 搜索）](https://dl.acm.org/doi/pdf/10.1145/3709708)；[西电新闻](https://ce.xidian.edu.cn/info/1323/10983.htm)
- **SHIELD: Encrypting Persistent Data of LSM-KVS from Monolithic to Disaggregated Storage**（Viraj Thakkar 等）。DOI 10.1145/3725354。机制：LSM KV 持久数据加密（静态加密，非访问模式隐藏）[待核]。— [ACM DL](https://dl.acm.org/doi/pdf/10.1145/3725354)
- **PoneglyphDB: Efficient Non-interactive Zero-Knowledge Proofs for Arbitrary SQL-Query Verification**（Binbin Gu, Juncheng Fang, Faisal Nawab）。3(1), DOI 10.1145/3709713。ZK 验证。— [ACM DL](https://dl.acm.org/doi/10.1145/3709713)
- **SecureXGB: A Secure and Efficient Multi-party Protocol for Vertical Federated XGBoost**（DOI 10.1145/3709723）；**Sequoia: An Accessible and Extensible Framework for Privacy-Preserving Machine Learning over Distributed Data**（DOI 10.1145/3709742）；**U-DPAP: Utility-aware Efficient Range Counting on Privacy-preserving Spatial Data Federation**（DOI 10.1145/3714333，机制 [待核]）。— [csconf-papers 2025/SIGMOD.md](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2025/SIGMOD.md)
- 其余命中（DProvDB 类、LDP 类、DP 合成数据类共 10+ 篇）为纯差分隐私，按约束排除。— 同上
- 清单来源：[csconf-papers papers/2025/SIGMOD.md（raw）](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2025/SIGMOD.md)（"251 papers · updated 2026-10-01"）。

#### F. PACMMOD Vol.4（SIGMOD 2026）—— 标题级完整筛查（csconf-papers 清单 354 篇）

- **NSHEDB: Noise-Sensitive Homomorphic Encrypted Database Query Engine**（Boram Jung, Hung-Wei Tseng, Yuliang Li）。Proc. ACM Manag. Data 4(1) (SIGMOD), Article 57, pp.1–26, DOI 10.1145/3786671，2026-04-07；arXiv 2602.24271。机制：BFV 字级 leveled HE，"without transciphering between different HE schemes (e.g., CKKS/BFV↔TFHE) or relying on trusted hardware"；支持等值、范围、聚合查询；作者自报 TPC-H 上相对 SOTA HE 数据库 20×–1370× 加速、73× 存储缩减。— [ACM DL](https://dl.acm.org/doi/abs/10.1145/3786671?af=R)；[csconf 列表](https://github.com/RealZST/csconf-papers/blob/main/papers/2026/SIGMOD.md)
- **Loom: Weaving PCS-Preserving and Searchable Cloud Backups for Secure Messaging**（Tiancheng Zhu, Jiabei Wang, Yongbin Zhou，南京理工）。DOI 10.1145/3802082；SIGMOD 2026 accepted papers Round 4；poster 2B.12。机制细节（SSE 具体构造、信任模型）[待核]。— [SIGMOD 2026 accepted list（被封锁，由搜索摘要引用）](https://2026.sigmod.org/sigmod_papers.shtml)；[ACM DL](https://dl.acm.org/doi/pdf/10.1145/3802082)
- **Sketch-based Secure Query Processing for Streaming Data**（Jianzhe Yu, Feng Han, Qi Dong, Qiyao Luo, Weiran Liu, Lin Qu, Ke Yi）。4(3), Article 233, DOI 10.1145/3802110。外包 MPC（数据以秘密分享形式由计算方持有），Count-Min / HLL / SpaceSaving / GK 草图，每次更新摊销 <1 ms。— [DOI](https://doi.org/10.1145/3802110)；[Ke Yi 主页](https://www.cse.ust.hk/~yike/research.htm)
- **Scalable Privacy-Preserving Shortest Path Distance Computation via 2-Hop Labeling in MPC**（Huizhong Wang, Yuanyuan Zeng, Kun Chen, Wei Dong, Chenhao Ma）。DOI 10.1145/3786695。MPC。— [csconf 列表](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2026/SIGMOD.md)
- **Rollbaccine: Herd Immunity against Storage Rollback Attacks in TEEs**（DOI 10.1145/3786693）：TEE 存储系统，非查询应用。— 同上
- **Differentially Oblivious Multi-way Join**（Zhiang Wu, Wei Dong, Xiao Hu，DOI 10.1145/3802039）：差分 oblivious（DP 松弛的 oblivious），边界项。— 同上
- **Privacy-preserving and Verifiable Causal Prescriptive Analytics**（DOI 10.1145/3769815）、**Reliable and Private Utility Signaling for Data Markets**（DOI 10.1145/3769821）：机制 [待核]。— 同上
- 清单来源：[csconf-papers papers/2026/SIGMOD.md（raw）](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2026/SIGMOD.md)（"354 papers · updated 2026-10-01"）。

### Inferences
- 2025–2026 两个 venue 上"密码组件 + 应用主语"论文的机制分布：MPC 协同分析类最多（SCQL、SPECIAL、Jodes、Alchemy、Bifrost、Sampling over Joins、Sketch、2-Hop MPC、GORAM），其次是 ORAM/oblivious 单服务器存储类（Oblivious Map、OasisDB、OBIR-tree、Grove），HE 类（HADES、NSHEDB），PIR 类仅 Femur 一篇，SSE 类为 Workload-Aware Encrypted Index 与 Loom。
- 以"单服务器、客户端持密钥、隐藏访问位置"为设定且主语是存储/点查的论文（Oblivious Map、OasisDB、Femur、NSHEDB、OBIR-tree、Grove）在两年内连续出现于两个 venue，说明这类问题设定在 DB 社区有持续录用记录（这是对录用事实的归纳，不是对可投性的结论）。

### Gaps
- PVLDB Vol.17（2024）与 PACMMOD Vol.2（2024）**没有可读的完整目录**：csconf-papers 无 2024 目录（[tree 页面](https://github.com/RealZST/csconf-papers/tree/main/papers)仅显示 2025、2026），dblp / vldb.org / dl.acm.org / 2024.sigmod.org 均被代理封锁。2024 年的清单仅为 WebSearch 命中，极可能遗漏（例如 PACMMOD Vol.2 其余期次、PVLDB Vol.17 的 SSE / ORAM 论文）。
- csconf-papers 清单是 DBLP 元数据的社区镜像，不是一手来源；本笔记把它当作"标题级目录"使用，各论文的卷期/DOI 只有在 WebSearch 另有一手页面引用时才写入，其余只给 PDF 链接。
- 标题级筛查依赖关键词（private/privacy/encrypted/oblivious/homomorphic/secure/enclave/TEE/PIR/searchable/MPC/federated/retrieval/ORAM 等）；标题中不含这些词的论文（如仅以系统名命名）可能漏掉。
- PVLDB Vol.18 清单的 raw 文件前 100k 字符与后 28k 字符分两次读取，Vol.19 同样分两次；两段拼接处各有一条被截断的标题未能检查。

---

## KQ2：每篇论文的字段表（应用问题 / 角色与信任模型 / 承重实验 / 形式化证明 / 家族映射）

### Takeaway
在已读到原文句子的范围内，只有 Femur、HADES、Doc*、SWAT、OasisDB、Oblivious Map、Jodes、Bifrost、NSHEDB、OBIR-tree 的信任模型可以部分核实；"是否含形式化定理证明"一项除 SWAT（"provably"）、Femur（定义 + 离散 Laplace 定理）、Doc*（信息论安全 + 恶意少数）、Bifrost（仅泄露连接表大小）外均 [待核]。家族映射：C 类（托管加密 KV/对象点查）证据最密集（Oblivious Map、OasisDB、Femur、NSHEDB、SWAT、Loom），B 类（关键词→变长列表 / 邻接表）有 Doc*、OBIR-tree、Grove、GORAM，A 类（私密向量检索/RAG）**在两个 venue 上未找到单服务器密码学方案**，只有联邦 MPC（FedSQ、FedVSE demo）和 ZK 可验证（V3DB）。

### Cited Findings

| # | 题目（venue, 年） | 应用问题一句话 | 角色与信任模型 | 承重实验类型 | 形式化证明 | 家族 |
|---|---|---|---|---|---|---|
| 1 | Femur (SIGMOD 2025, PACMMOD 3(3)) | 公开 KV 库上的私密键查询，允许用户按带宽/隐私需求调节 | 客户端 + 单个公开 KV 服务器；服务器学到的是被噪声扩展后的位置区间；原文把"full PIR security"视为可选上限；服务器 honest-but-curious 还是 malicious [待核]；客户端持 PIR 私钥 [待核] | 吞吐（第三方称最高 163.9×）、带宽/延迟随网络带宽变化、与 PIR 基线对比（Pantheon 被引用，是否作为实验基线 [待核]） | 有形式化定义"𝜖-dist indistinguishability"与"adding noise according to a discrete Laplace distribution satisfies distance-based indistinguishability"的定理陈述；证明是否在正文/附录 [待核] | **C**（公开/托管 KV 点查；注意库是公开而非加密） — [arXiv](https://arxiv.org/abs/2503.05376v1) [ACM](https://dl.acm.org/doi/10.1145/3725299) |
| 2 | Towards Practical Oblivious Map (VLDB 2025, 18(3)) | 客户端在外包加密 KV 存储上做键查询而不泄露访问模式 | 客户端 + 单个不可信服务器（ORAM 标准模型）[待核原文措辞]；客户端持加密密钥 [待核] | 轮数/带宽复杂度 + 实现对比 OMIX++ 等（具体指标 [待核]） | 复杂度定理有；安全证明 [待核] | **C** — [ePrint](https://eprint.iacr.org/2024/1650) |
| 3 | OasisDB (VLDB 2025, 18(11)) | 在 oblivious KV-store 上支撑关系型点查/范围/join/聚合并可横向扩展 | 客户端（多用户）+ 代理层/批处理层/解析层 + oblivious KV（PathORAM / Waffle）；对手为被动持久观察者 [待核原文]；隐藏 access 与 volume pattern | 吞吐/延迟随机器数扩展（Epinions），对比 CockroachDB、ObliDB、Obliviator | [待核] | **C** — [vldb PDF](https://www.vldb.org/pvldb/vol18/p4478-ahmed.pdf) [GitHub](https://github.com/dsg-uwaterloo/ObliSQL) |
| 4 | Doc* / Access Control for Information-Theoretically Secure Data (VLDB 2025, 18(10)) | 外包 key-document 存储上的关键词检索 + 基于密钥的访问控制 | 多服务器 Shamir 秘密分享；"a minority of the servers can also be malicious ... may collude amongst themselves and/or with the clients"（即容忍少数合谋，非严格不合谋）；客户端持秘密分享重构能力 [待核] | 延迟：231.5 ms over 5,000 keywords across 500,000 files；隐藏输出大小 | 信息论安全（"unconditional or information-theoretic security"）；定理/证明位置 [待核] | **B + C**（关键词→文档列表；托管文档存储） — [vldb PDF](https://www.vldb.org/pvldb/vol18/p3243-sharma.pdf) [arXiv](https://arxiv.org/abs/2507.10730) |
| 5 | OBIR-tree (SIGMOD 2025, PACMMOD 3(1)) | 加密数据上 oblivious top-k 空间关键词查询 | 客户端 + 单服务器（Path ORAM）；扩展版服务器带 SGX enclave 以减少往返；隐藏 search/access/volume pattern | 搜索效率比基线 25×–723× | [待核] | **B**（关键词查询 + oblivious 索引） — [ACM](https://dl.acm.org/doi/pdf/10.1145/3709708) |
| 6 | Grove / Index-free Adjacency in Oblivious Graph Processing (VLDB 2026, 19) | oblivious 图数据库上的邻居 / t-hop 查询，支持 index-free adjacency | 客户端/服务器部署（DAORAM 仓库描述）[待核]；对比 GraphOS 并"provides stronger security guarantees than the AHR protocol" | 邻居查询与 t-hop 查询性能 vs GraphOS | [待核] | **B**（邻接表/变长邻居列表的 oblivious 读取） — [ePrint](https://eprint.iacr.org/2025/2047) |
| 7 | GORAM (VLDB 2025, 18(10)) | 联邦图（互不信任持有方）上的 ego-centric 查询 | MPC 多方（仓库名 GORAM-ABY3 → 3 方 [待核]）；"no information about the graphs or the querying keys is exposed" | 十亿边图延迟（第三方摘要 58 ms–35.7 s，未核） | [待核] | B-邻近（邻居列表），但为多方联邦而非单服务器 — [arXiv](https://arxiv.org/abs/2410.02234v1) |
| 8 | HADES (VLDB 2025, 18(7)) | 公开数据上带点/范围/布尔谓词的私密聚合 | 客户端 + 单服务器 FHE；明确不依赖"non-colluding servers, hardware enclaves"；客户端持 FHE 私钥 [待核] | 端到端 TPC-H 查询相对一轮 FHE 方案 204×–6574× | [待核] | 无直接映射（公开数据私密查询，Pantheon 同系作者） — [vldb PDF](https://www.vldb.org/pvldb/vol18/p2159-liu.pdf) |
| 9 | NSHEDB (SIGMOD 2026, PACMMOD 4(1)) | 同态加密数据库上的等值/范围/聚合查询引擎 | 客户端持 HE 私钥 + 单服务器纯 HE 计算（"without ... relying on trusted hardware"）[信任模型细节待核] | TPC-H 延迟与存储（自报 20×–1370×、73×） | [待核] | **C**（托管加密 DB 点查/范围） — [ACM](https://dl.acm.org/doi/abs/10.1145/3786671?af=R) |
| 10 | SWAT (VLDB 2024, 17(10)) | 加密 KV/范围/动态工作负载的系统级可调泄露缓解 | 客户端→代理→云端 SGX enclave；放宽"trusted client proxy"假设；"Keys and equal-length values" | 延迟/吞吐 vs encryption-only 与 zero-leakage（10.6× 慢 / 31.6× 快），合成数据 | 自称"provably mitigating"，每类工作负载有独立安全定义；定理/证明位置 [待核] | **C**（托管加密 KV 点查/范围） — [vldb PDF](https://www.vldb.org/pvldb/vol17/p2445-zheng.pdf) |
| 11 | Loom (SIGMOD 2026, PACMMOD 4) | 安全消息云备份的可搜索且 PCS 保持的加密 | [待核] | [待核] | [待核] | **C**（加密消息/邮件类存储检索）[待核] — [ACM](https://dl.acm.org/doi/pdf/10.1145/3802082) |
| 12 | Workload-Aware Encrypted Index (VLDB 2026, 19(4)) | 外包加密数据上的隐私保护范围查询索引 | [待核]（PRF 比较协议暗示单服务器 SSE 式模型） | 查询时间 ≥83× 优于 SOTA | "theoretical analysis and experiments" [待核] | B/C 邻近（加密范围索引） — [ACM](https://dl.acm.org/doi/10.14778/3785297.3785313) |
| 13 | Jodes (VLDB 2025, 18(5)) | 分布式 TEE 上的 oblivious equi-join | 多台 TEE 服务器 + 网络对手；"reveals only the input sizes and output size" | 16 服务器性能最高 6× | [待核] | 无 — [vldb PDF](https://www.vldb.org/pvldb/vol18/p1291-wang.pdf) |
| 14 | Bifrost (VLDB 2026, 19(6)) | 两方纵向分区数据的安全 join | 两方 MPC，输出秘密分享；"reveals only the size of the joined table" | 相对 iPrivJoin 2.54–22.32× 加速、通信降 84–89%（PVLDB 版）；arXiv 版数字不同（4.41–12.96×、64–80%）——来源冲突 | [待核] | 无 — [vldb PDF](https://www.vldb.org/pvldb/vol19/p1156-han.pdf) |
| 15 | SecretFlow-SCQL (VLDB 2024, 17(12)) | 多方安全协同查询平台 | MPC 多方秘密分享 | 安全排序 1M 值 4.5 min，126× vs EMP | [待核] | 无 — [vldb PDF](https://www.vldb.org/pvldb/vol17/p3987-fang.pdf) |
| 16 | SecuDB (VLDB 2024, 17(12)) | TEE 内整库运行的隐私保护与防篡改 RDBMS | 单服务器 Intel TDX；不隐藏访问模式 | 吞吐比例 84.7%/94.7%，防篡改 <22.6% | [待核] | 无 — [vldb PDF](https://www.vldb.org/pvldb/vol17/p3906-yang.pdf) |
| 17 | Sketch-based Secure Query Processing for Streaming Data (SIGMOD 2026, 4(3)) | 外包 MPC 上的流式草图查询 | 多计算方持秘密分享 | 每更新 <1 ms 摊销 | 有"provable accuracy guarantees"（精度而非安全）[待核] | 无 — [DOI](https://doi.org/10.1145/3802110) |
| 18 | FedSQ (VLDB 2024, 17(12), 4 页, track 待核) / FedVSE (VLDB 2025 demo) | 联邦向量相似查询 | 多数据所有者 + MPC 聚合 | [待核] | [待核] | A-邻近（联邦而非单服务器） — [FedSQ](https://www.vldb.org/pvldb/vol17/p4441-zhu.pdf) [FedVSE](https://dl.acm.org/doi/10.14778/3750601.3750674) |
| 19 | V3DB (VLDB 2026, 19) | 可验证向量检索（ZK） | 服务器可能作恶，客户端验证；不涉及查询机密性 [待核] | [待核] | ZK 证明系统 | A-邻近（完整性而非隐私） — [vldb PDF](https://www.vldb.org/pvldb/vol19/p2604-qiu.pdf) |

- 变长记录处理方式：SWAT 原文明确限定"equal-length values"（[vldb PDF](https://www.vldb.org/pvldb/vol17/p2445-zheng.pdf)）；Doc* 处理的是关键词→文件集合且隐藏输出大小（[arXiv](https://arxiv.org/abs/2507.10730)）；其余论文对变长值/填充的处理 [待核]。

### Inferences
- 表中可核实的单服务器方案（Femur、Oblivious Map、OasisDB、NSHEDB、SWAT、OBIR-tree）均以"延迟/吞吐 + 与明文或上一代密码方案的倍率"作为承重实验，没有一篇以"效果指标"（如检索召回）为主；A 类问题（RAG 取回）若投这两个 venue，没有同类先例可对标。
- Femur 与 HADES 都延续 Pantheon 的"公开数据私密查询"设定（Femur 引用 Pantheon；HADES 由 Pantheon 作者 Ahmad/Gupta 合著），说明 Pantheon 问题线在 2025 年于 SIGMOD 与 VLDB 各有一篇后继。

### Gaps
- 由于 PDF 与 arXiv/ePrint 全文站点被封锁，"角色数 / 是否要求不合谋 / 客户端持何密钥 / 定理与证明位置"大多数未能核到原句，统一标 [待核]。
- Loom、Workload-Aware Encrypted Index、Alchemy、LETIndex、Sectric、PrivAGM、U-DPAP、Secure Join in Multi-Identifier DB、Range Counting over Geographic Data 的机制只能从标题/摘要片段判断。

---

## KQ3：Pantheon（VLDB 2023）是否有 2024–2026 后续；VLDB/SIGMOD 2024–2026 上的加密向量数据库 / private ANN / oblivious vector search

### Takeaway
未找到名为 "Pirex" 的论文；Pantheon 原作者在 VLDB/SIGMOD 2024–2026 上的后继是 **HADES（VLDB 2025，Ahmad & Gupta 合著，FHE 私密聚合于公开数据）**，而同问题设定（公开 KV 私密点查）的他人后继是 **Femur（SIGMOD 2025，引用 Pantheon）**；其余 Pantheon 后继（PIRacle、Practical Keyword PIR、FEDPIR、SecOutPIR、Peer2PIR）均在非 DB venue。两个 venue 上**未找到单服务器的加密向量数据库 / private ANN / oblivious vector search 正式论文**，相关的只有联邦 MPC（FedSQ、FedVSE demo）、ZK 可验证（V3DB）与 HE-LLM 推理 tutorial。

### Cited Findings
- Pantheon 本体：PVLDB 16(4): 643–656，ACM 日期 2022-12，VLDB 2023 会议；"Pantheon achieves a 93X improvement for server-side latency over a state-of-the-art solution"。— [vldb.org PDF](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)；[ACM DL](https://dl.acm.org/doi/10.14778/3574245.3574251)；[ePrint 2023/1331](https://eprint.iacr.org/2023/1331)
- 同作者 VLDB 2023 tutorial：Private Information Retrieval in Large Scale Public Data Repositories（PVLDB 16(12)）。— [ACM DL](https://dl.acm.org/doi/10.14778/3611540.3611572)
- "Pirex"：两轮 WebSearch 均未命中任何同名论文；搜索工具明确回答"I couldn't find a VLDB 2024 or 2025 paper called 'Pirex'"。— [搜索命中的 Pantheon GitHub](https://github.com/ishtiyaque/Pantheon)
- HADES（VLDB 2025）由 Trinabh Gupta、Ishtiyaque Ahmad 合著，是 Pantheon 作者在 VLDB 的直接后继论文（FHE 私密聚合，而非 KV 点查）。— [DOI](https://doi.org/10.14778/3734839.3734852)
- Femur（SIGMOD 2025）引用 Pantheon 作为相关工作："Pantheon [7] and Constant-weight PIR [57] implement equality-checking operators and invoke traditional PIR schemes for homomorphic operations"。— [作者 PDF（被封锁，由搜索引用）](https://people.iiis.tsinghua.edu.cn/~huanchen/publications/femur-sigmod25.pdf)
- 非 DB venue 的 Pantheon 后继：PIRacle: A Fast and Scalable Private Information Retrieval System for Key-Value Stores（IEEE Transactions on Computers, 2026）；Practical Keyword Private Information Retrieval from Key-to-Index Mappings（USENIX Security 2025，"15∼178× reduction in communication and 1.1∼2.4× runtime improvement"）；FEDPIR（多值键 KV 的单服务器关键词 PIR，XJTU，venue [待核]）；Peer2PIR（arXiv 2405.17307）；SecOutPIR（Cybersecurity 期刊 2026）。— [USENIX PDF](https://www.usenix.org/system/files/conference/usenixsecurity25/sec25cycle1-prepub-1224-hao.pdf)；[XJTU 条目](https://scholar.xjtu.edu.cn/en/publications/fast-private-retrieval-on-key-value-store-with-multiple-values-pe/)；[Peer2PIR](https://arxiv.org/pdf/2405.17307)
- Ahmad 的 Google Scholar 2025 条目中只有一篇 IEEE BigData 2025（LLM-based SQL equivalence），无 PIR 方向的 VLDB/SIGMOD 2024–2026 论文。— [Google Scholar 条目（搜索引用）](https://scholar.google.com.hk/citations?user=wdQie4MAAAAJ&hl=ja)
- 加密向量检索：PVLDB Vol.18 / Vol.19 与 PACMMOD Vol.3 / Vol.4 的标题级筛查中，含 "vector" 且含密码/隐私关键词的只有 FedVSE（demo, MPC 联邦）、FedBridge（联邦，非密码）、V3DB（ZK 可验证）。— [csconf 2025/VLDB](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2025/VLDB.md)；[csconf 2026/VLDB](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2026/VLDB.md)
- 2024 的 FedSQ 同为联邦 MPC 向量相似查询（4 页）。— [vldb.org PDF](https://www.vldb.org/pvldb/vol17/p4441-zhu.pdf)
- 私密 RAG / oblivious semantic search 方向仅见预印本，无 VLDB/SIGMOD 录用证据：VeriANN（ePrint 2026/923，两服务器不合谋 DPF-PIR + LSH + 认证混淆电路）、PRAG（arXiv 2604.26525，CKKS 同态检索）、Spruce（arXiv 2609.03376）、Hoss: Fast Oblivious Semantic Search with Heterogeneous GPU-CPU-TEE Architecture（arXiv 2609.04522）、Quantization Enables Private Dense Retrieval（arXiv 2609.36376）。— [ePrint 2026/923](https://eprint.iacr.org/2026/923)；[PRAG](https://arxiv.org/abs/2604.26525)；[Hoss（pith.science）](https://pith.science/paper/2609.04522)
- Private ANN 的经典先例在 S&P 2022（Private Approximate Nearest Neighbor Search with Sublinear Communication），非 DB venue。— [搜索引用](https://arxiv.org/pdf/2603.21466)

### Inferences
- 家族 A（私密 RAG / 向量取回）在 VLDB/SIGMOD 2024–2026 没有"单服务器密码学"先例，只有联邦/可验证邻近工作；家族 C（托管加密 KV 点查）与 B（关键词/邻接表 oblivious 读取）各有多篇录用记录。
- Pantheon 的问题线在 DB 社区保持活跃（Femur、HADES），但两篇后继都偏向"公开数据 + 私密查询"而非"加密托管存储"。

### Gaps
- Ahmad / Gupta 的 DBLP 页面（dblp.org/pid/184/6643、dblp.org/pid/20/7539）被封锁，无法逐条确认他们 2024–2026 的全部发表。
- 无法读取 Femur 实验节，不能确认 Pantheon 是否为其实验基线。
- FEDPIR 的 venue 与年份未能确认。

---

## KQ4：某个 venue-年份"未找到"的明确声明与检索方式

### Takeaway
没有任何一个 venue-年份完全"未找到"；但 2024 年两个 venue 的覆盖是**不完整的部分命中**，而 PIR 机制在 PVLDB Vol.17 与 PACMMOD Vol.2–4 中除 Femur 外均未找到。

### Cited Findings
- 检索方式一览：
  - DBLP 搜索 API（`dblp.org/search/publ/api?q=<kw> Proc. VLDB Endow.` 与 `... Proc. ACM Manag. Data`，kw = private/privacy/encrypted/oblivious/secure/homomorphic/searchable/trusted execution/multi-party/confidential）：**全部被代理封锁**（curl CONNECT 403；WebFetch EGRESS_BLOCKED），镜像 dblp.uni-trier.de / dblp1.uni-trier.de 同样封锁。
  - vldb.org/pvldb/volumes/17、18、19 目录页：封锁。
  - dl.acm.org/toc/pacmmod 与 2024/2025/2026.sigmod.org 目录页：封锁。
  - api.semanticscholar.org、api.openalex.org：封锁。
  - 替代：`raw.githubusercontent.com/RealZST/csconf-papers/main/papers/{2025,2026}/{VLDB,SIGMOD}.md`（可读；2024 目录不存在，返回 404）— [tree 页](https://github.com/RealZST/csconf-papers/tree/main/papers)
  - WebSearch（extended 模式）共 20 余次，覆盖：各 venue-年份 + 关键词组合、Pantheon/Pirex 后继、private ANN/encrypted vector、各核心论文的信任模型与实验。
- PVLDB Vol.17（2024）中以 PIR 为机制的论文：**未找到**（搜索工具明确回答 "Your 'private information retrieval' keyword didn't surface any Volume 17 paper"）。— [搜索引用 Pantheon vol16](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)
- PACMMOD Vol.2（SIGMOD 2024）中以 PIR/SSE/ORAM/HE 为机制且主语为应用的正式论文：**未找到**（仅找到 TEE 加密内存管理一篇与 MPC demo 一篇；Waffle/TGCB/Veil 均为 PACMMOD Vol.1 2023）。— [SIGMOD 2024 程序页（封锁，搜索引用）](https://2024.sigmod.org/program_sigmod.shtml)
- PVLDB Vol.18/19 与 PACMMOD Vol.3/4 中"单服务器私密向量检索 / oblivious ANN"正式论文：**未找到**（标题级完整筛查）。— [csconf 2026/SIGMOD](https://raw.githubusercontent.com/RealZST/csconf-papers/main/papers/2026/SIGMOD.md)

### Inferences
- 2024 年的"未找到"是覆盖不足导致的弱证据，不能据此断言该年没有相关论文；2025–2026 年的"未找到"（单服务器私密向量检索）是标题级完整筛查后的较强证据，但仍受关键词筛选限制。

### Gaps
- 若后续会话网络可达 dblp.org 或 vldb.org，应优先补查：PVLDB Vol.17 全卷（含 2023-09 至 2024-08 各期）与 PACMMOD Vol.2 全部 6 期的标题/摘要，关键词同上。
- 本笔记未对任何论文下"新颖性 / 可投性"结论；家族映射列只是按论文自述应用问题做的对应。
