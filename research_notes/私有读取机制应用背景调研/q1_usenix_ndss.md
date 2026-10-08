# Q1：USENIX Security 与 NDSS（2024–2026）接纳证据——以应用问题为主语的 PIR / SSE / ORAM / HE 论文

> 方法与覆盖说明（请先读）：本会话的网络出口代理封锁了 dblp.org、dblp.dagstuhl.de、usenix.org、ndss-symposium.org、eprint.iacr.org、arxiv.org、api.openalex.org、api.semanticscholar.org 等全部文献索引与官网（WebFetch 返回 EGRESS_BLOCKED）。因此**无法逐年翻阅完整录用列表**；以下清单全部来自 WebSearch 返回的、锚定在 usenix.org / ndss-symposium.org / eprint.iacr.org 官方 URL 上的摘要片段。结论：这是一份"抽样核验"清单而非穷举清单；凡未在片段中直接读到的字段一律标 [待核]。
>
> 年份/venue 归属判定依据：论文 PDF 位于 `usenix.org/system/files/usenixsecurityYY-*.pdf` 或 `usenix.org/conference/usenixsecurityYY/presentation/*`；NDSS 论文位于 `ndss-symposium.org/wp-content/uploads/YYYY-*-paper.pdf` 或 `ndss-symposium.org/ndss-paper/*`。带 `prepub` 的 USENIX 文件名表示预印本（同一录用论文）。

## 关键问题 1：两 venue 2024–2026 中以应用问题为主语、机制为 PIR/SSE/ORAM/HE/DPF 的论文

### Takeaway
在能核到的范围内，USENIX Security 2024–2026 与 NDSS 2024–2026 录用的"应用主语"密码检索论文集中在四个应用方向：(i) 元数据私密消息传递（OMR 系列：PerfOMR'24、SophOMR'26、InstantOMR'26、Oblivious Signaling'26，均为单服务器 HE）；(ii) 多用户/系统级加密文档搜索（MUSES'24、d-DSE'24、I/O-Efficient DSE'24、Unus pro omnibus NDSS'24、DDR-SSE'26）；(iii) ORAM 托管存储（Treebeard'25、V-ORAM'25、MVP-ORAM NDSS'26）；(iv) 私密推荐 / 私密布尔检索（Nudge'26 三方 MPC；BKPIR NDSS'26 keyword PIR，自述目标应用为 private web search 与 patent retrieval）。**没有找到任何一篇以"私密 RAG / LLM 从共享知识库取回 passage"为主语的两 venue 正会论文**（该方向目前只见 arXiv 预印本）。候选家族 B（关键词查找取回变长列表）在 BKPIR（many-to-many keyword→values）与 d-DSE（volume leakage）上有最直接的锚点；家族 C（托管加密对象存储点查）在 Treebeard、MVP-ORAM、DDR-SSE、MUSES 上有锚点；家族 A 在这两 venue 上**无直接先例**。

### Cited Findings

#### 1.1 USENIX Security 2024（第 33 届，2024-08-14~16，Philadelphia）

**[U24-1] PerfOMR: Oblivious Message Retrieval with Reduced Communication and Computation**（Zeyu Liu, Eran Tromer, Yunhao Wang）
- 应用问题：接收者元数据私密的消息投递/取回（服务器为收件人检索其消息而不知道哪些消息属于谁）。原句："OMR works in the following model: in the systems, there are senders who send messages to the recipients without revealing who the recipients are." — [USENIX PDF](https://www.usenix.org/system/files/usenixsecurity24-liu-zeyu.pdf)
- 信任模型：单服务器 + 密码学假设（Ring-LWE）。原句："The adversary may control, or collude with, all the parties in the systems, except for the sender and recipient(s) of the message(s) whose privacy is to be protected."；"Both of the constructions are based on standard lattice hardness assumption (Ring-LWE)". 论文对比两服务器不合谋方案时指出 OMR "realizes it via cryptographic assumptions on a single server"。— [USENIX PDF](https://www.usenix.org/system/files/usenixsecurity24-liu-zeyu.pdf)；[presentation page](https://www.usenix.org/conference/usenixsecurity24/presentation/liu-zeyu)
- 客户端密钥：收件人持 HE 解密密钥（检索电路由服务器同态求值）[待核具体密钥结构]。
- 承重实验：检索电路同态求值时间（"15x faster than the prior work"）、通信与计算开销。— [presentation page](https://www.usenix.org/conference/usenixsecurity24/presentation/liu-zeyu)
- 形式化证明：[待核]（摘要称基于标准格假设，未读到定理陈述）。
- 家族映射：无（消息投递，不是按位置读取记录）；但其"单服务器 + 服务器学不到谁取了什么"的信任模型与我们一致。

**[U24-2] MUSES: Efficient Multi-User Searchable Encrypted Database**（Tung Le, Rouzbeh Behnia, Jorge Guajardo, Thang Hoang；SEC'24 Article 145, pp. 2581–2598）
- 应用问题：多写者/单读者的外包加密数据库关键词搜索，并隐藏 search / result / volume pattern。— [USENIX presentation](https://www.usenix.org/conference/usenixsecurity24/presentation/le)；[PDF](https://www.usenix.org/system/files/usenixsecurity24-le.pdf)
- 机制：DPF + Distributed PRF + Oblivious Linear Group Action；搜索阶段用 DPF-PIR "privately retrieves K encrypted columns of the index"。— [PDF](https://www.usenix.org/system/files/usenixsecurity24-le.pdf)
- 信任模型：**多服务器（分布式服务器）**，"if at least one server is honest, the attacker cannot learn the index requested"；读者为 honest，持公私钥对；写者各自拥有独立数据库，读者与写者 setup 后不直接通信。— [slides](https://www.usenix.org/system/files/usenixsecurity24_slides-le.pdf)
- 承重实验：关键词搜索延迟（"two orders of magnitude faster than the prior state of the art"）与用户带宽（"up to 12× lower user bandwidth cost"）。— [presentation](https://www.usenix.org/conference/usenixsecurity24/presentation/le)
- 形式化证明：[待核]。
- 家族映射：B/C（关键词→加密索引列；多写者托管加密数据库）。注意：非单服务器。

**[U24-3] d-DSE: Distinct Dynamic Searchable Encryption Resisting Volume Leakage in Encrypted Databases**（Dongli Liu, Wei Wang, Peng Xu, Laurence T. Yang, Bo Luo, Kaitai Liang；pp. 2563–2580）
- 应用问题：加密数据库中动态关键词检索的 volume（结果数量）泄露抵抗。— [USENIX presentation](https://www.usenix.org/conference/usenixsecurity24/presentation/liu-dongli)；[slides](https://www.usenix.org/system/files/usenixsecurity24_slides-liu-dongli.pdf)
- 机制：SSE（实例 BF-SRE，基于 Symmetric Revocable Encryption）；新安全概念 "Distinct with Volume-Hiding (DwVH)" + forward/backward privacy。
- 信任模型：单服务器 SSE（客户端持对称密钥）[待核原句]。
- 承重实验：Crime / Wikipedia / Enron 数据集，与 MITRA*、AURA 及填充策略 SEAL、ShieldDB 对比；通信成本降低 "6.36 to 53.14x for search queries"。— [arXiv 版](https://arxiv.org/pdf/2403.01182)；[appendix](https://www.usenix.org/system/files/usenixsecurity24-appendix-liu-dongli.pdf)
- 形式化证明：有。Theorem 1（BF-SRE adaptively secure w.r.t. leakage function），Theorem 2（DwVH security），game-hop 证明。— [arXiv 版](https://arxiv.org/pdf/2403.01182)
- 家族映射：B（关键词→变长结果集合；通过 distinct search 而非填充来处理 volume）。

**[U24-4] I/O-Efficient Dynamic Searchable Encryption meets Forward & Backward Privacy**（Mondal 等；USENIX Sec'24 fall cycle 预印本）
- 应用问题：磁盘 I/O 友好的动态 SSE。改造 NDSS'20 构造为 I/O-preserving，并给出 "oblivious-merge protocol merges two equal-sized databases without revealing any information"。— [prepub PDF](https://www.usenix.org/system/files/sec24fall-prepub-487-mondal.pdf)；[final PDF](https://www.usenix.org/system/files/usenixsecurity24-mondal.pdf)
- 信任模型/实验/证明：[待核]。家族映射：B/C。

**[U24-5] Secure Account Recovery for a Privacy-Preserving Web**（Little 等）
- 仅在正文中将 PIR 列为私密账户恢复的一种可选机制（与通用 MPC 并列），非 PIR 主语论文。— [PDF](https://www.usenix.org/system/files/usenixsecurity24-little.pdf)。家族映射：无。记录仅为完整性。

**[U24-6] Hermes: Efficient and Secure Multi-Writer Encrypted Database**（Hoang 组）
- 仅在 ResearchGate 片段出现，venue [待核]（搜索未在 usenix.org 返回该题目）。— [ResearchGate](https://www.researchgate.net/publication/391425553_Hermes_Efficient_and_Secure_Multi-Writer_Encrypted_Database)

#### 1.2 USENIX Security 2025（第 34 届，2025-08-13~15，Seattle）

**[U25-1] Treebeard: A Scalable and Fault Tolerant ORAM Datastore**（Setayesh 等）
- 应用问题：可扩展、容错的 ORAM 键值存储，防访问模式攻击。原句："the first scalable and fault tolerant Oblivious RAM (ORAM) based datastore designed to protect applications from access pattern attacks"；"We model Treebeard as an encrypted key-value store that supports single object Get and Put requests."— [PDF](https://www.usenix.org/system/files/usenixsecurity25-setayesh.pdf)
- 信任模型：可信代理（proxy）被切分到多台机器 + 不可信存储；安全性 "hinges on its ability to read and evict the same paths identified by executing sequential Ring ORAM"。客户端密钥 [待核]。
- 承重实验：吞吐/延迟/扩展性/容错 [具体数字待核]。形式化证明：[待核]。
- 家族映射：C（托管加密 KV 点查；定长 block）。

**[U25-2] V-ORAM: A Versatile and Adaptive ORAM Framework with Service Transformation for Dynamic Workloads**（Bo Zhang, Helei Cui, Xingliang Yuan, Zhiwen Yu, Bin Guo）
- 应用问题：外包 ORAM 服务在动态工作负载下的服务类型切换与成本优化（含 "searchable encryption" 作为应用之一）。— [presentation](https://www.usenix.org/conference/usenixsecurity25/presentation/zhang-bo-voram)；[PDF](https://www.usenix.org/system/files/usenixsecurity25-zhang-bo-voram.pdf)
- 信任模型：honest client / semi-honest server（单服务器）。— [slides](https://www.usenix.org/system/files/sec25_slides_zhang-bo-voram.pdf)
- 承重实验：处理时间与通信（"saves up to 10^4.12x"）、货币成本（"up to 33.1%"）。
- 形式化证明：有（"we formalize the security of V-ORAM, and prove that V-ORAM holds the security of ORAMs, including the process of service transformation"）。— [slides](https://www.usenix.org/system/files/sec25_slides_zhang-bo-voram.pdf)
- 家族映射：C。

**[U25-3] H2O2RAM: A High-Performance Hierarchical Doubly Oblivious RAM**（Zheng 等）
- TEE 内的分层 doubly-oblivious RAM；含 oblivious stashless Cuckoo hash table。— [PDF](https://www.usenix.org/system/files/usenixsecurity25-zheng.pdf)。偏原语（见第 2 节）。

**[U25-4] FABLE: Batched Evaluation on Confidential Lookup Tables in 2PC**（Su 等）
- 将 batch PIR 技术改造用于 2PC 中的机密查表。— [PDF](https://www.usenix.org/system/files/usenixsecurity25-su-zhengyuan.pdf)；[presentation](https://www.usenix.org/conference/usenixsecurity25/presentation/su-zhengyuan)。两方模型；家族映射：无。

**[U25-5] Breaking the Layer Barrier: Remodeling Private Transformer Inference with Hybrid CKKS and MPC**
- HE+MPC 私密 Transformer 推理（非检索）。venue：34th USENIX Security 2025（来自搜索摘要，URL [待核]）。家族映射：无。

**[U25-6] ALERT**（SSE 泄露风险实时估计的 ML 框架）— 出现在 USENIX Sec'25 Cycle 1 列表。— [cycle1 list](https://www.usenix.org/conference/usenixsecurity25/cycle1-accepted-papers)。非构造论文。

（USENIX'25 的 PIR 原语论文见第 2 节：Keyword PIR from Key-to-Index Mappings、Distributional PIR、Flexway O-Sort。）

#### 1.3 USENIX Security 2026（第 35 届，2026-08-12~14，Baltimore；**只核到 Cycle 1 录用列表**）

**[U26-1] Nudge: A Private Recommendations Engine**（Alexandra Henzinger, Emma Dauterman, Henry Corrigan-Gibbs, Dan Boneh）
- 应用问题：私密推荐——用户对大目录评分/取回内容，服务器不学习用户评分与取回项。— [presentation](https://www.usenix.org/conference/usenixsecurity26/presentation/henzinger)；[PDF](https://www.usenix.org/system/files/usenixsecurity26-henzinger.pdf)；[ePrint 2026/179](https://eprint.iacr.org/2026/179)
- 信任模型：**三服务器 3PC（复制秘密分享）**。原句："User privacy holds against an adversary that compromises the entire secret state of one server."；"User privacy also does not protect against two or more servers who collude."；"Nudge does not hide metadata: the servers learn when each user sends its messages."— [prepub PDF](https://www.usenix.org/system/files/conference/usenixsecurity26/sec26_prepub_henzinger.pdf)
- 承重实验：端到端训练时间与服务器间通信（Netflix 数据集，50 万用户 × 1 万项，"50 mins with 40 GB of server-to-server communication"，3 台 192 核服务器）。
- 形式化证明：[待核]。
- 家族映射：无（推荐，非位置读取）；但题中"delivers the recommended content without any server learning which item was fetched"的取回部分与 PIR 类似 [待核机制]。

**[U26-2] Oblivious Signaling**（Mirza Kamrul Bashar Shuhan, Foteini Baldimtsi, Giuseppe Ateniese）
- 应用问题：接收者私密的消息投递，把开销从接收者转移到发送者。服务器为每个收件人保存定长加密收件箱，对所有收件箱施加同一同态更新。— [presentation](https://www.usenix.org/conference/usenixsecurity26/presentation/shuhan)；[ePrint 2026/1975](https://eprint.iacr.org/2026/1975)；[code](https://github.com/shuhanmirza/oblivious-signaling-usenix26)
- 信任模型：单服务器 FHE；接收者隐私对不可信服务器成立，"even when the server colludes with other users"。
- 承重实验：发送/检查开销（"delivery is expensive but checking is cheap"），具体数字 [待核]。形式化证明：[待核]。
- 家族映射：无。

**[U26-3] SophOMR: Improved Oblivious Message Retrieval from SIMD-Aware Homomorphic Compression**（Lee 等）
- 相比 PerfOMR："3.4x in runtime, 2.2x in digest size, and 1.5x in key size"（65,536 payloads）。— [prepub PDF](https://www.usenix.org/system/files/conference/usenixsecurity26/sec26_prepub_lee.pdf)。单服务器 HE。家族映射：无。

**[U26-4] InstantOMR: Oblivious Message Retrieval with Low Latency and High Parallelism**（Liang 等）
- TFHE functional bootstrapping + RLWE 混合；实现用 Primus-fhe。— [prepub PDF](https://www.usenix.org/system/files/conference/usenixsecurity26/sec26_prepub_liang.pdf)。家族映射：无。

**[U26-5] DDR-SSE: Duplicated Retrieval of Documents for System-wide Secure Searchable Symmetric Encryption**（Zichen Gui, Simon-Philipp Merz, Kenneth G. Paterson, Sikhar Patranabis）
- 应用问题：系统级安全（含文档取回阶段）的 SSE，只需静态客户端状态。原句："it introduces a novel encrypted document retrieval scheme that uses duplicated document storage and randomized document retrieval to suppress access pattern leakage."— [USENIX '26 cycle1 list](https://www.usenix.org/conference/usenixsecurity26/cycle1-accepted-papers)；[ePrint 2026/753](https://eprint.iacr.org/2026/753)；[IBM Research](https://research.ibm.com/publications/ddr-sse-duplicated-retrieval-of-documents-for-system-wide-secure-searchable-symmetric-encryption)
- 信任模型：单服务器 SSE，客户端持对称密钥，静态状态。
- 承重实验：抗 query reconstruction 攻击（"even under 'unrealistically' strong attack assumptions"）+ 原型可扩展到真实规模数据库。
- 形式化证明：有（IBM 页面："simulation-based security proof with respect to a formal system-wide leakage profile"）。
- 家族映射：B/C（关键词→文档集合，含变长文档取回）。

**[U26-6] PICS: Private Intersection over Committed (and reusable) Sets**（Goel 等）— PSI；应用列 mobile contact discovery 与 password breach detection。— [prepub PDF](https://www.usenix.org/system/files/conference/usenixsecurity26/sec26_prepub_goel.pdf)。家族映射：无（PSI）。

**[U26-7] Enhanced Private Set Union from Secret-shared ...**（Hao 等）— PSU 原语。— [PDF](https://www.usenix.org/system/files/usenixsecurity26-hao.pdf)。家族映射：无。

（USENIX'26 PIR 原语：ZipPIR，见第 2 节。）

#### 1.4 NDSS 2024

**[N24-1] Private Aggregate Queries to Untrusted Databases**（Syed Mahbub Hafiz 等，UC Davis）
- 应用问题：对不可信数据库做隐私保护的聚合查询（sum/count/mean/histogram）并单轮交互。原句："permits a user to fetch the aggregated result, hiding all sensitive sections of the complex query from the hosting PIR server in a single round of interaction."— [NDSS paper page](https://www.ndss-symposium.org/ndss-paper/private-aggregate-queries-to-untrusted-databases/)；[PDF](https://www.ndss-symposium.org/wp-content/uploads/2024-1211-paper.pdf)；[arXiv 2403.13296](https://arxiv.org/abs/2403.13296)
- 机制：IT-PIR（基于 Goldberg Oakland'07 regular IT-PIR）；原文称可 "privately search and retrieve documents from a database without requiring any encryption for the database"。
- 信任模型：IT-PIR 通常为多服务器不合谋 [待核论文系统模型原句]。
- 承重实验/证明：[待核]。家族映射：B（关键词/条件→聚合结果），但多服务器。

**[N24-2] Unus pro omnibus: Multi-Client Searchable Encryption via Access Control**
- 应用问题：多写者多读者的可委托搜索加密（DSE），最优搜索时间。原句涉及 volume hiding："prior solutions usually pad dummy ciphertexts until each keyword has roughly the same volume"；本文让写者 "makes dummy updates for keywords with fewer volumes and batches them with real updates"；并称其 counter-maintenance 组件 "concretely more efficient than generic multi-client oblivious primitives without non-colluding assumptions"。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2024-288-paper.pdf)
- 信任模型：单服务器（无不合谋假设）[待核]。实验/证明 [待核]。家族映射：B/C。

**[N24-3] PriSrv: Privacy-Enhanced and Highly Usable Service Discovery**（Yang 等）— 基于 DNS-SD 的私密服务发现，机制为匹配加密（FEME），非 PIR/SSE/ORAM。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2024-174-paper.pdf)。家族映射：无。

**[N24-4] Pencil: Private and Extensible Collaborative Learning without the Non-Colluding Assumption**；**HEIR: A Unified Representation for Cross-Scheme Compilation of FHE**— HE 训练/编译器，非检索。— [Pencil PDF](https://www.ndss-symposium.org/wp-content/uploads/2024-512-paper.pdf)；[HEIR PDF](https://www.ndss-symposium.org/wp-content/uploads/2024-67-paper.pdf)。家族映射：无。

#### 1.5 NDSS 2025

**[N25-1] SHAFT**（NDSS 2025 program 列表）— 不平衡 PSI，Levelled FHE + Cuckoo hashing，"minimizes communication by 25-41%"。— [NDSS 2025 program](https://www.ndss-symposium.org/ndss-program/symposium-2025/)。家族映射：无（PSI）。

**[N25-2] Recurrent Private Set Intersection for Unbalanced Databases**（paper 365）— 大集合通信一次性、小集合线性递归成本。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2025-365-paper.pdf)。家族映射：无。

**[N25-3] BumbleBee**— RLWE-HE 私密 Transformer 推理（NDSS 2025 program 列表）。— [NDSS 2025 program](https://www.ndss-symposium.org/ndss-program/symposium-2025/)。家族映射：无。

**[N25-4] Iris: Dynamic Privacy Preserving Search in Authenticated Chord Peer-to-Peer Networks**（Aktypi, Rasmussen, Oxford；paper 392）— 不是 PIR：仅改请求方行为，定义 (α,δ)-privacy 对抗合谋节点；related work 引 Peer2PIR。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2025-392-paper.pdf)；[NDSS page](https://www.ndss-symposium.org/ndss-paper/iris-dynamic-privacy-preserving-search-in-authenticated-chord-peer-to-peer-networks/)。家族映射：无。

**[N25-5] Ring of Gyges: Accountable Anonymous Broadcast via Secret-Shared Shuffle**（paper 268）— 匿名广播，仅提及 PIR。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2025-268-paper.pdf)。家族映射：无。

#### 1.6 NDSS 2026

**[N26-1] BKPIR: Keyword PIR for Private Boolean Retrieval**（Jie Song, Zhen Xu, Yan Zhang, Pengwei Zhan, Mingxuan Li, Shuai Ma, Ru Xie；中科院信工所；Summer cycle, paper s536）
- 应用问题：对 many-to-many keyword→values 数据库做私密布尔检索；自述目标应用 "privacy-preserving web search and patent retrieval"。— [NDSS paper page](https://www.ndss-symposium.org/ndss-paper/bkpir-keyword-pir-for-private-boolean-retrieval/)；[PDF](https://www.ndss-symposium.org/wp-content/uploads/2026-s536-paper.pdf)；[code](https://github.com/gimmehug/bkpir)
- 机制：先构造 Many-to-Many Keyword PIR (MMKPIR)，再加布尔算子；"leverages advancements in homomorphic equality operations"。其定位原句："PIR—particularly computational PIR—operates over plaintext databases while offering stronger protection of user access and query patterns. Queries are stateless, support a single round of interaction, and impose minimal burden on clients."
- 信任模型：计算型 PIR（单服务器）[待核原句]；"this extension preserves the core security guarantees of classical PIR"。
- 承重实验：通信成本（"reduction proportional to the total number of values"）与聚合查询处理性能（"scale linearly with the number of values"）。
- 形式化证明：[待核]。家族映射：**B**（关键词→变长值集合）。

**[N26-2] MVP-ORAM: a Wait-free Concurrent ORAM for Confidential BFT Storage**（LASIGE, Lisbon；paper f1809）
- 应用问题：并发客户端访问拜占庭容错复制存储时隐藏访问模式。"the first ORAM protocol designed explicitly for concealing access patterns in BFT SMR systems while achieving wait-freedom"。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2026-f1809-paper.pdf)；[arXiv 2512.12006](https://arxiv.org/pdf/2512.12006)；[NDSS 2026 program](https://www.ndss-symposium.org/ndss-program/symposium-2026/)
- 信任模型：多副本服务器，部分拜占庭故障（非单服务器）。实验/证明 [待核]。家族映射：C。

**[N26-3] VDORAM: Towards a Random Access Machine with Both Public Verifiability and Distributed Obliviousness**（山东大学/清华/港城大）— 分布式 ORAM 原语。— [NDSS 2026 program](https://www.ndss-symposium.org/ndss-program/symposium-2026/)。家族映射：C（偏原语）。

**[N26-4] SNPeek: Side-Channel Analysis for Privacy Applications on Confidential VMs**（paper f699）— 对 PIR、private heavy hitters 等在机密 VM 上的侧信道分析（攻击/评估类）。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2026-f699-paper.pdf)。家族映射：无。

**[N26-5] Learning from Leakage: Database Reconstruction from ... (REMIN)**（paper f935）— 对多维 SE 的泄露滥用攻击。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2026-f935-paper.pdf)。攻击类。

**[N26-6] MinBucket MPSI**（paper f182）— 多方 PSI，列 private contact discovery 为应用。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2026-f182-paper.pdf)。家族映射：无。

#### 1.7 应用方向覆盖小结（可核到的）
| 应用问题 | 两 venue 2024–26 论文 | 单服务器？ | 家族 |
|---|---|---|---|
| 元数据私密消息投递 (OMR) | PerfOMR U24, SophOMR U26, InstantOMR U26, Oblivious Signaling U26 | 是（HE） | 无 |
| 多用户/系统级加密文档搜索 | MUSES U24（多服务器 DPF）, d-DSE U24, I/O-Eff DSE U24, Unus pro omnibus N24, DDR-SSE U26 | 除 MUSES 外为单服务器 SSE | B/C |
| ORAM 托管存储 | Treebeard U25, V-ORAM U25, MVP-ORAM N26, VDORAM N26 | V-ORAM 是；Treebeard 代理分布式；MVP/VDORAM 多副本 | C |
| 私密推荐 | Nudge U26 | 否（3PC） | 无 |
| 私密布尔/关键词检索（web search、专利） | BKPIR N26 | 计算型 PIR [待核] | B |
| 私密聚合查询 | Private Aggregate Queries N24 | 否（IT-PIR）[待核] | B |
| CT 日志私密审计 | YPIR U24（原语论文，CT 为动机）；ZipPIR U26（CrySP 讲座称应用于 CT） | 是 | 无/C |
| 私密 RAG / LLM 检索 | **未找到** | — | A |
| 私密 DNS / blocklist / password checkup / contact discovery（PIR 主语） | 2024–26 两 venue 未找到 PIR 主语论文；仅 Batch PIR U24 引言与 ZipPIR U26 列为应用；PICS U26 / MinBucket N26 为 PSI | — | — |

### Inferences
- 两 venue 近三年"应用主语 + 单服务器"的组合，主要出现在 OMR（HE）与 SSE 两条线；PIR 主语论文在这两 venue 上几乎都以"原语改进"而非"某应用系统"投稿（YPIR、Keyword PIR、Distributional PIR、ZipPIR），应用只作为引言动机或评估场景（CT 审计）。这与 USENIX Sec'21 Checklist（private blocklist）那种应用主语 PIR 论文形成对比，提示近年 PIR 应用型论文可能更多流向 PETS/CCS/系统会（SOSP Tiptoe），需与 Q1 其他 venue 笔记交叉核对。
- 家族 B 的直接锚点是 BKPIR（NDSS'26）与 d-DSE（USENIX'24）：前者正面承认 "one-to-one keyword→value" 假设不够并处理 many-to-many；后者正面处理 volume leakage。两者都未在可见片段中描述"按容量档分桶"的变长策略 [待核]。
- 家族 C 的锚点是 ORAM 托管存储线（Treebeard/V-ORAM/MVP-ORAM）与 DDR-SSE 的文档取回阶段；这些工作的记录模型均为定长 block [待核 DDR-SSE 文档长度处理]。
- 家族 A 在两 venue 无直接先例；最接近的只有私密 Transformer 推理（Breaking the Layer Barrier U25、BumbleBee N25）和 PoisonedRAG（U25，攻击类），均非"私密取回 passage"。

### Gaps
- **覆盖不全**：无法访问 DBLP/官网，USENIX'24、'25 与 NDSS'24、'25 的完整列表未逐条检查；USENIX'26 仅见 Cycle 1 列表；NDSS'26 仅见 Summer/Fall 片段。可能遗漏以 "private"/"oblivious" 为题但未被搜索引擎召回的论文。
- 多数论文的"客户端持什么密钥""是否形式化证明""承重实验类型"字段未能从摘要片段确认，已标 [待核]。
- Hermes（多写者加密数据库）与 "Single Pass Client-Preprocessing PIR"（Lazzaretti, sec24fall prepub）venue 归属需核。
- Private Aggregate Queries（N24）是否多服务器、BKPIR（N26）是否严格单服务器，需读系统模型章节。

## 关键问题 2：纯原语论文（供对照）

### Takeaway
2024–2026 两 venue 的 PIR 原语论文全部为单服务器（LWE/RLWE/Paillier），记录模型均为定长条目；可见片段中**没有一篇**声明支持变长记录。Keyword PIR（USENIX'25）将 key→index 映射后跑标准 PIR，实验用 1M × 32B 定长条目。

### Cited Findings
| 题目 | venue/年 | 单服务器 | 变长记录 | 关键原句 / URL |
|---|---|---|---|---|
| YPIR: High-Throughput Single-Server PIR with Silent Preprocessing（Menon, Wu）pp. 5985–6002 | USENIX Sec 2024 | 是 | 未见（1-bit/1-byte 记录基准） | "12.1 GB/s/core server throughput and requires 2.5 MB of total communication"（32 GB DB）；CT 审计动机，"reduces the server-side costs by a factor of 8×"。[presentation](https://www.usenix.org/conference/usenixsecurity24/presentation/menon)；[PDF](https://www.usenix.org/system/files/usenixsecurity24-menon.pdf)；[ePrint 2024/270](https://eprint.iacr.org/2024/270)；[slides](https://www.usenix.org/system/files/usenixsecurity24_slides-menon.pdf) |
| Batch PIR and Labeled PSI with Oblivious Ciphertext Compression（Bienstock, Patel, Seo, Yeo） | USENIX Sec 2024 | 是 | 未见 | 引言列真实部署："certificate transparency, contact discovery, device enrollment, leaked-password checks, and URL blocklists"。[PDF](https://www.usenix.org/system/files/usenixsecurity24-bienstock.pdf)；[presentation](https://www.usenix.org/conference/usenixsecurity24/presentation/bienstock) |
| Unbalanced Circuit-PSI from Oblivious Key-Value Retrieval（Hao 等） | USENIX Sec 2024 | 两方 PSI | 未见 | 提出 OKVR 功能与 sparse OKVS；"sublinear communication complexity in the size of the larger set"。[PDF](https://www.usenix.org/system/files/usenixsecurity24-hao-meng-unbalanced.pdf) |
| Single Pass Client-Preprocessing Private Information Retrieval（Lazzaretti 等） | USENIX Sec 2024 [待核，依据 sec24fall-prepub 文件名] | 是 | 未见 | [prepub PDF](https://www.usenix.org/system/files/sec24fall-prepub-150-lazzaretti.pdf) |
| Practical Keyword Private Information Retrieval from Key-to-Index Mappings（Meng Hao 等）pp. 3397–3416 | USENIX Sec 2025 | 是 | 未见；实验 "1 million 32-byte entries ... 47 ms" | "encode a key-value database into an indexable database with a key-to-index mapping, then run standard PIR"；相比 ChalametPIR "15 to 178× reduction in communication and a 1.1 to 2.4× runtime improvement"；三构造 KPIR^kvs / KPIR^hash / KPIR^index。[presentation](https://www.usenix.org/conference/usenixsecurity25/presentation/hao)；[PDF](https://www.usenix.org/system/files/usenixsecurity25-hao.pdf)；[ePrint 2025/210](https://eprint.iacr.org/2025/210) |
| Distributional Private Information Retrieval（Lehmkuhl, Henzinger, Corrigan-Gibbs） | USENIX Sec 2025 | 是 | 未见 | 依记录热度分布加速；"in-distribution queries succeed with good probability while out-of-distribution queries succeed with lower probability"。[prepub PDF](https://www.usenix.org/system/files/conference/usenixsecurity25/sec25cycle1-prepub-41-lehmkuhl.pdf)；[final PDF](https://www.usenix.org/system/files/usenixsecurity25-lehmkuhl.pdf) |
| Flexway O-Sort: Enclave-Friendly and Optimal Oblivious Sorting（Gu 等） | USENIX Sec 2025 | TEE | — | 应用举 Signal 私密联系人发现的 oblivious KV store。[PDF](https://www.usenix.org/system/files/usenixsecurity25-gu-tianyao.pdf) |
| H2O2RAM（Zheng 等） | USENIX Sec 2025 | TEE | — | [PDF](https://www.usenix.org/system/files/usenixsecurity25-zheng.pdf) |
| ZipPIR: High-throughput Single-server PIR without Client-side Storage（Mahdavi, Diaa, Kerschbaum） | USENIX Sec 2026 (Cycle 1) | 是（Paillier 压缩 LWE 密文） | 未见 | ">2 GB/s"；"less than 200 KB of server-side storage per client"；"almost silent offline phase, requiring no communication beyond an initial public key"；应用列表含 "compromised credential checks, private contact discovery, ... private blocklist lookups, private SCT auditing, private messaging, and private distributed file systems"。[presentation](https://www.usenix.org/conference/usenixsecurity26/presentation/mahdavi)；[prepub PDF](https://www.usenix.org/system/files/conference/usenixsecurity26/sec26_prepub_mahdavi.pdf)；[CrySP 讲座](https://uwaterloo.ca/computer-science/events/phd-seminar-crysp-zippir-high-throughput-single-server-pir-without-client-side-storage) |
| Enhanced Private Set Union from Secret-shared ...（Hao 等） | USENIX Sec 2026 | 两方 | — | [PDF](https://www.usenix.org/system/files/usenixsecurity26-hao.pdf) |
| VDORAM | NDSS 2026 | 分布式 | — | [NDSS 2026 program](https://www.ndss-symposium.org/ndss-program/symposium-2026/) |

### Inferences
- 单服务器 PIR 原语线（SimplePIR'23 → YPIR'24 → Keyword PIR'25 / Distributional PIR'25 → ZipPIR'26）在 USENIX Security 连续四年有录用，说明该 venue 对"单服务器 PIR 系统改进"接纳稳定；但这些论文的评估口径是定长小记录吞吐（GB/s/core）与通信量，不含变长记录。

### Gaps
- 未能读取任何一篇原语论文全文，"是否支持变长记录"仅基于摘要/实验描述判断为"未见"，非"不支持"。

## 关键问题 3：变长记录 / keyword PIR / 分桶填充 / volume-hiding 的处理方式

### Takeaway
在两 venue 2024–2026 可核到的论文中，变长问题以三种方式出现：(a) SSE 线以 **volume（结果条数）** 为对象——d-DSE 用 distinct search 消除填充，Unus pro omnibus 用写者 dummy update 平衡 volume，DDR-SSE 用文档复制 + 随机取回抑制访问模式；(b) keyword PIR 线——USENIX'25 Keyword PIR 通过 key→index 映射归约到定长 index PIR，NDSS'26 BKPIR 处理 keyword→多值集合；(c) **没有一篇两 venue 论文可核到"按 2^k 容量档分桶后 PIR"的做法**。最接近"PIR + 变长记录"主语的论文在 venue 范围外（EuroS&P 2025 "Densely-Packed Single-Server PIR for Variable-Length Data"；NDSS 2013 "One (Block) Size Fits All"）。

### Cited Findings
- d-DSE（USENIX'24）：以 distinct search 代替填充，"sharply reduces communication cost compared with padding strategies, by roughly 6.36 to 53.14x for search queries"，基线含填充方案 SEAL、ShieldDB；单服务器 SSE。— [arXiv 2403.01182](https://arxiv.org/pdf/2403.01182)；[USENIX presentation](https://www.usenix.org/conference/usenixsecurity24/presentation/liu-dongli)
- Unus pro omnibus（NDSS'24）：描述先前 volume-hiding 做法 "pad dummy ciphertexts until each keyword has roughly the same volume"，本文改为 "a writer makes dummy updates for keywords with fewer volumes and batches them with real updates"。— [PDF](https://www.ndss-symposium.org/wp-content/uploads/2024-288-paper.pdf)
- MUSES（USENIX'24）：隐藏 volume pattern 靠 DPF-PIR 取回固定 K 列（Bloom filter 参数）而非填充；多服务器。— [PDF](https://www.usenix.org/system/files/usenixsecurity24-le.pdf)
- DDR-SSE（USENIX'26）："duplicated document storage and randomized document retrieval to suppress access pattern leakage"；静态客户端状态；simulation-based proof w.r.t. system-wide leakage profile。文档长度如何处理 [待核]。— [ePrint 2026/753](https://eprint.iacr.org/2026/753)；[IBM](https://research.ibm.com/publications/ddr-sse-duplicated-retrieval-of-documents-for-system-wide-secure-searchable-symmetric-encryption)
- Keyword PIR from Key-to-Index Mappings（USENIX'25）：key→index 映射 + 标准 PIR，实验为定长 32B 条目；变长值处理在可见片段中未出现。— [presentation](https://www.usenix.org/conference/usenixsecurity25/presentation/hao)
- BKPIR（NDSS'26）：明确指出现有 keyword PIR "implicit assumption that these schemes handle databases with a one-to-one relationship between keywords and values"，并为 many-to-many 设计 MMKPIR；通信成本 "reduction proportional to the total number of values"。分桶/填充细节 [待核]。— [NDSS page](https://www.ndss-symposium.org/ndss-paper/bkpir-keyword-pir-for-private-boolean-retrieval/)
- Distributional PIR（USENIX'25）：按记录访问热度分布优化，而非按长度分档。— [prepub](https://www.usenix.org/system/files/conference/usenixsecurity25/sec25cycle1-prepub-41-lehmkuhl.pdf)
- **venue 外对照**：Densely-Packed Single-Server PIR for Variable-Length Data（Nakamori, Motomura, Fujiki, Institute of Science Tokyo；Zenodo 标注 EuroS&P, 2025-07）："Existing ways of handling records of different lengths either waste space on padding or depend on complicated key management"；对比 "a symmetric encryption approach and a padding approach"，"performs more consistently than padding across different record-length distributions"，含密钥传输时 "up to 1.8 times faster than the symmetric encryption method"。— [Zenodo 16740584](https://zenodo.org/records/16740584)
- **venue 外对照（年份外）**：One (Block) Size Fits All: PIR and SPIR with Variable-Length Records via Multi-Block Queries（Henry 等，NDSS 2013）— [PDF](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
- **venue 外对照（arXiv）**：PIR-RAG: A System for Private Information Retrieval in Retrieval-Augmented Generation（arXiv 2509.21325，2025）——后续论文仅以 arXiv 预印本引用，未见正会归属。— [arXiv](https://arxiv.org/pdf/2509.21325)；PRAG（arXiv 2604.26525，2026-04，双模式 HE 近似 + 客户端协助）— [arXiv](https://arxiv.org/abs/2604.26525)

### Inferences
- 两 venue 的审稿社区把"变长"主要理解为 SSE 的 volume leakage（结果条数），而非 PIR 记录字节长度；"按容量档公开分桶 + 桶内 PIR"的表述在可核到的两 venue 论文中没有先例，最近的同类表述来自 EuroS&P'25（填充 vs 对称加密 vs 密集打包三分法）。
- keyword PIR 两篇（U25、N26）与 d-DSE 共同构成家族 B 的"问题被承认"证据：keyword→变长值集合的取回是该社区承认的公开问题。

### Gaps
- EuroS&P'25 Densely-Packed PIR 的具体打包方法、是否单服务器、是否有证明，未能读取全文。
- 所有两 venue 论文中关于"如何对变长记录填充/分桶"的具体描述均未能从片段核到，需后续读全文（BKPIR §系统模型、DDR-SSE 文档取回节、Keyword PIR 实验设置）。
- 因搜索引擎召回限制，可能遗漏两 venue 中题名不含 "private/oblivious/PIR" 但处理 volume/length 泄露的论文。
