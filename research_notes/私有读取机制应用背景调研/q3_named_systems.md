# Q3 占位检查专项：六个指定系统的精确技术事实

> **取证方法与总体限制（务必先读）**
> 本次会话的网络出口代理封锁了全部学术 PDF 主机：`eprint.iacr.org`、`www.iacr.org`、`usenix.org`、`dl.acm.org`、`vldb.org`/`mail.vldb.org`、`arxiv.org`、`par.nsf.gov`、`ndss-symposium.org`、`petsymposium.org`、`people.csail.mit.edu`、`pdos.csail.mit.edu`、`dspace.mit.edu`、`cs.brown.edu`、`cs.uwaterloo.ca`、`cypherpunks.ca`、`cse.hkust.edu.hk`、`escholarship.org`、`semanticscholar.org`、`scholar.archive.org`、`web.xidian.edu.cn` 等全部返回 `EGRESS_BLOCKED`。**唯一能直接读取的是 `raw.githubusercontent.com`（作者代码仓库 README）。**
> 因此下文的英文原句分两类：
> - **[直接核]**：来自可直接打开的 GitHub README（Tiptoe、Pantheon）。
> - **[待核·WebSearch 摘录]**：WebSearch 后端在索引原文 PDF 时返回的句子。它们看起来是逐字引文，但本会话**未能打开 PDF 核对**，页码/节号/表号一律缺失。报告撰写者在引用前必须用所附 URL 核对原文。
> 凡 Key questions 中要求"附原句 + 出处页码/章节/表号/图号"而本会话无法提供的字段，均在各系统 Gaps 中如实列出。**本笔记不下任何"新颖性"结论。**

---

## 1. Tiptoe (SOSP 2023)

### Takeaway
Tiptoe 是单一信任域（不依赖不合谋服务器/可信硬件）的私有网页搜索系统，分"ranking service"与"URL service"两个客户端面向服务：先用线性同态加密做私有最近邻排名得到文档 ID，再向 URL service 做 PIR 取回 URL（不取回文档正文）。关于 URL 的定长/截断处理与"服务器学到 cluster 编号"的泄漏原句，本会话未能从原文核到。

### Cited Findings
- 题目/作者/venue：**"Private Web Search with Tiptoe"**，Alexandra Henzinger, Emma Dauterman, Henry Corrigan-Gibbs, Nickolai Zeldovich，SOSP 2023，ACM DOI 10.1145/3600006.3613134，pp. 396–416；预印本 ePrint 2023/1438 — [直接核：GitHub README](https://raw.githubusercontent.com/ahenzinger/tiptoe/main/README.md)；[ACM DL（被封，未打开）](https://dl.acm.org/doi/10.1145/3600006.3613134)；[ePrint 2023/1438（被封，未打开）](https://eprint.iacr.org/2023/1438.pdf)；[MIT PDOS 副本（被封）](https://pdos.csail.mit.edu/papers/tiptoe:sosp23.pdf)
- 官方 PDF URL 候选：`https://eprint.iacr.org/2023/1438.pdf`、`https://pdos.csail.mit.edu/papers/tiptoe:sosp23.pdf`、`https://dl.acm.org/doi/pdf/10.1145/3600006.3613134`、MIT OA 副本 `https://dspace.mit.edu/bitstream/handle/1721.1/152985/3600006.3613134.pdf` — 均 [待核]（被封）。
- 服务器设定（单信任域，非多服务器不合谋）：**"Tiptoe's privacy guarantee is based on cryptography alone; it does not require hardware enclaves or non-colluding servers."** — [直接核：GitHub README](https://raw.githubusercontent.com/ahenzinger/tiptoe/main/README.md)
- 方法概述：**"Tiptoe uses semantic embeddings to reduce the problem of private full-text search to private nearest-neighbor search."** — [直接核：GitHub README](https://raw.githubusercontent.com/ahenzinger/tiptoe/main/README.md)
- 部署形态（逻辑上单服务方，物理上分片）：**"The ranking service's state is split into 80 shards"**；**"The URL service's state is split into 8 shards"**；README 另述一个 coordinator 位于两服务之前，各进程 "communicate with each other over the network." — [直接核：GitHub README](https://raw.githubusercontent.com/ahenzinger/tiptoe/main/README.md)
- 两阶段架构：**"A Tiptoe deployment consists of data-loading batch jobs, a client, and two client-facing services—a ranking service and a URL service."**；取回顺序：**"After the client has the IDs of the best-matching documents, it queries the URL service. From the URL service's answer, the client recovers the best documents' URLs."** — [待核·WebSearch 摘录自 ePrint/PDOS PDF](https://eprint.iacr.org/2023/1438.pdf)
- 密码学原语：**"the servers ... take a query ciphertext from the user and, using linearly homomorphic encryption and private information retrieval, compute a response ciphertext without ever decrypting the query ciphertext"**（WebSearch 转述性摘录，措辞可能非逐字）— [待核·WebSearch](https://eprint.iacr.org/2023/1438.pdf)
- 聚类索引规模：**"this allows Tiptoe to scale to up to C≈350K clusters"**（384 维 embedding 条件下）— [待核·WebSearch](https://eprint.iacr.org/2023/1438.pdf)
- 聚类 README 中的数据处理步骤：**"Break large clusters into smaller clusters"**、**"Group URLs for related pages in the same cluster"** — [直接核：GitHub README](https://raw.githubusercontent.com/ahenzinger/tiptoe/main/README.md)
- 承重实验（摘要数字）：**"Running on a 45-server cluster, Tiptoe can privately search over 360 million web pages with 145 core-seconds of server compute, 56.9 MiB of client-server communication (74% of which occurs before the client enters its search query), and 2.7 seconds of end-to-end latency."**；效果：**"Tiptoe ranks the best-matching result in position 7.7 on average on the MS MARCO benchmark ... state-of-the-art non-private neural search (average rank 2.3) ... classical tf-idf (average rank 6.7)"** — [待核·WebSearch 摘录自摘要](https://eprint.iacr.org/2023/1438)
- 正确性非精确：**"The nearest-neighbor protocol returns approximate results. It does not produce exact results and provides no formal correctness guarantees."**（WebSearch 转述）— [待核·WebSearch](https://eprint.iacr.org/2023/1438.pdf)
- 后续工作对其的评价（非原文）："Tiptoe uses a clustering-based approximate nearest neighbor (ANN) algorithm that significantly limits the search quality." — [Pacmann, ePrint 2024/1600（被封，经 WebSearch）](https://eprint.iacr.org/2024/1600.pdf)

### Inferences
- 由 README "does not require ... non-colluding servers" 与 "linearly homomorphic encryption and private information retrieval" 可推断 Tiptoe 属单服务器计算型（LHE/PIR）设定；分片与 coordinator 是同一信任域内的工程部署。
- 由 "recovers the best documents' URLs" 可推断 URL retrieval 阶段取回对象是 URL 而非文档正文；ranking 阶段客户端取回的是加密的排名响应（具体是否为"内积分数向量"需核原文 §4/§5）。
- 文档内容是公开网页（Common Crawl 类语料），服务器持明文索引——这是由"private web search over public web pages"的问题设定推断，原句未核到。

### Gaps
- **URL 是否定长截断 / 每 cluster 内 URL 如何打包填充**：未找到原句（原文 URL service 一节被封）。[待核]
- **泄漏契约原句**（服务器是否学到 cluster 索引、查询次数、时序；Theorem/Section 编号）：WebSearch 两次均未返回相关句子；仅有摘要级"reveals no information about the search query to its servers"。[待核]
- **ranking 阶段客户端取回的精确对象**（分数向量/cluster 内文档打分）：未核。[待核]
- **表号/图号**：全部缺失。[待核]

---

## 2. Coeus (SOSP 2021)

### Takeaway
Coeus 解决"用户持私有查询串 q，服务器持公开文档集 D，用户取回 top-K 之一且服务器不知 q 与所取文档"的问题；三轮协议：安全矩阵-向量积打分（query-scorer）→ multi-retrieval PIR 取元数据（metadata provider）→ single-retrieval PIR 取文档（document provider）。变长文档靠"分离元数据与文档、打包与压缩文档库"处理。服务器是否为单服务器计算型 PIR 的原句本会话未核到。

### Cited Findings
- 题目/作者/venue：**"Coeus: A System for Oblivious Document Ranking and Retrieval"**，Ishtiyaque Ahmad, Laboni Sarker, Divyakant Agrawal, Amr El Abbadi, Trinabh Gupta（UCSB），SOSP 2021，DOI 10.1145/3477132.3483586 — [NSF PAR 条目（被封，经 WebSearch）](https://par.nsf.gov/biblio/10316017)；[ACM DL（被封）](https://dl.acm.org/doi/10.1145/3477132.3483586)
- 官方/开放 PDF URL 候选：`https://par.nsf.gov/servlets/purl/10316017`、`https://dl.acm.org/doi/pdf/10.1145/3477132.3483586`、`https://eprint.iacr.org/2022/154.pdf`、课程副本 `https://cs.brown.edu/courses/csci2390/2021/readings/coeus.pdf` — 均 [待核]（被封）。
- 问题设定（摘要）：**"Given a private string q and a remote server that holds a set of public documents D, how can one of the K most relevant documents to q in D be selected and viewed without anyone (not even the server) learning anything about q or the document?"** — [待核·WebSearch 摘录自摘要](https://par.nsf.gov/biblio/10316017)
- 原语组合（摘要）：**"Coeus composes two cryptographic primitives: secure matrix-vector product for scoring document relevance using the widely-used term frequency-inverse document frequency (tf-idf) method, and private information retrieval (PIR) for obliviously retrieving documents."** — [待核·WebSearch](https://par.nsf.gov/biblio/10316017)
- 元数据/文档分离（摘要）：**"Coeus first reduces the PIR overhead by separating out private metadata retrieval from document retrieval, and it then scales secure matrix-vector product to tf-idf matrices with several hundred billion elements through a series of novel cryptographic refinements."** — [待核·WebSearch](https://par.nsf.gov/biblio/10316017)
- 三轮角色（正文摘录，措辞可能非逐字）：**"the query-scorer maintains a tf-idf matrix and uses secure matrix-vector product to score documents against a user query"**；**"In round two, Coeus runs multi-retrieval PIR between the client, who holds K indices, and a metadata provider holding the metadata library. For round three, the client and a document provider use single-retrieval PIR."** — [待核·WebSearch 摘录](https://cs.brown.edu/courses/csci2390/2021/readings/coeus.pdf)
- 变长文档处理（正文摘录，措辞可能非逐字）：**"PIR expects all objects in the server's library to be the same size. Coeus gets around this by separating metadata from documents. The server can pack variable-sized documents and compress the document library, which reduces PIR compute time."** — [待核·WebSearch 摘录](https://cs.brown.edu/courses/csci2390/2021/readings/coeus.pdf)
- 威胁模型（正文摘录）：论文交叉引用 **"Coeus's threat model (§2.2)"**，并称 **"adopts a strong threat model that does not make assumptions about the server"**；附录 **"contains a proof that Coeus's protocol provides query privacy"**，第一轮 **"the server learns nothing about the query because the client's encrypted vector is protected by semantic security"** — [待核·WebSearch 摘录](https://cs.brown.edu/courses/csci2390/2021/readings/coeus.pdf)
- 承重实验（摘要）：**"for a corpus of English Wikipedia containing 5 million documents, a keyword dictionary with 64K keywords, and on a cluster of 143 machines on AWS, Coeus enables a user to obliviously rank and retrieve a document in 3.9 seconds—a 24× improvement over a baseline system."** — [待核·WebSearch 摘录自摘要](https://par.nsf.gov/biblio/10316017)
- 代码：GitHub `ishtiyaque/Coeus_artifact`（"Artifact for SOSP 2021 paper Coeus"）— [GitHub（README 路径 404，未读到内容）](https://github.com/ishtiyaque/Coeus_artifact)
- 后续：E-Coeus（"Sparsity Aware of TF-IDF Matrix to Accelerate Oblivious Document Ranking and Retrieval"，IEEE，2024）报告比 Coeus 提升 23% — [IEEE Xplore（经 WebSearch）](https://ieeexplore.ieee.org/document/10538556/)

### Inferences
- 由"does not make assumptions about the server"与"semantic security"保护查询向量，可推断 Coeus 为单信任域计算型（HE+CPIR）设定而非不合谋多服务器；但 WebSearch 明确表示"None of the results describe a two-server or non-colluding model"，而 query-scorer / metadata provider / document provider 是三种逻辑角色——它们是否要求互不合谋，必须读 §2.2 确认。
- 取回对象：第 1 轮取回加密打分，第 2 轮取回 K 条元数据，第 3 轮取回完整文档（由三轮描述推断）。
- 文档内容公开（"set of public documents D"），服务器持明文。

### Gaps
- **§2.2 威胁模型原句**、**是否单服务器 / 哪种 PIR 库（SealPIR/XPIR）**：未核。[待核]
- **变长文档"打包/压缩"的具体机制原句与节号**（是否填充到定长块、是否泄漏文档大小）：未核。[待核]
- **泄漏契约**是否提及文档长度/块数：未核。[待核]
- **表号/图号**：缺失。[待核]

---

## 3. Pantheon (VLDB 2023)

### Takeaway
Pantheon 是单服务器、单轮、基于同态加密（Microsoft SEAL 库）的"公开 KV 存储私有取值"系统：服务器持明文、客户端无法加密或修改存储；以 n（条目数）、键大小、值大小为缩放参数，代码以统一的 `-s` 值大小运行。值是否强制定长及泄漏契约原句未从原文核到。

### Cited Findings
- 题目/作者/venue：**"Pantheon: Private Retrieval from Public Key-Value Store"**，Ishtiyaque Ahmad, Divyakant Agrawal, Amr El Abbadi, Trinabh Gupta（UCSB），Proc. VLDB Endow. 16(4): 643–656，DOI 10.14778/3574245.3574251（VLDB 2023 会议；dblp 标 2022 年卷）；ePrint 2023/1331 — [VLDB 卷页（被封，经 WebSearch）](https://mail.vldb.org/pvldb/volumes/16/paper/Pantheon%3A%20Private%20Retrieval%20from%20Public%20Key-Value%20Store)；[dblp](https://dblp1.uni-trier.de/rec/journals/pvldb/AhmadAAG22.html)；[ePrint 2023/1331（被封）](https://eprint.iacr.org/2023/1331)
- 官方 PDF URL：`https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf`、`https://eprint.iacr.org/2023/1331.pdf` — [待核]（被封）。
- 问题设定/内容可见性：**"Pantheon is a system that allows its clients to retrieve the value corresponding to a key from such a key-value store privately"**；**"The key-value store in this case is _public_ and a client cannot encrypt or modify it to ensure privacy."** — [直接核：GitHub README](https://raw.githubusercontent.com/ishtiyaque/Pantheon/main/README.md)
- 单服务器实现：**"The code for deploying Pantheon server on a single machine is in `Pantheon/src/single_machine/`"**；README 未讨论多服务器部署 — [直接核：GitHub README](https://raw.githubusercontent.com/ishtiyaque/Pantheon/main/README.md)
- 记录模型（代码参数）：`-n` **"The number of key-value tuples in the key-value store."**；`-k` **"The size of each key in bits."**；`-s` **"The size of each value in Bytes."**；示例 `./Pantheon -n 32768 -k 64 -s 256` — [直接核：GitHub README](https://raw.githubusercontent.com/ishtiyaque/Pantheon/main/README.md)
- 论文对值大小的立场（WebSearch 摘录，措辞可能非逐字）：**"Pantheon should support arbitrarily large values, because hashing a value does not serve the purpose in most cases"**；缩放参数为 **"the size of each key, the size of each value, and the number of tuples in the key-value store (n)"** — [待核·WebSearch 摘录](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)
- 威胁模型（WebSearch 摘录，措辞可能非逐字）：**"privacy in this context implies hiding the access pattern of a client"**；**"The adversary can monitor, log, and analyze any network traffic"**；**"guarantee that no information about the client's query is leaked even in the presence of an active adversary that can modify data or computation"** — [待核·WebSearch 摘录](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)
- 单轮 / 客户端无需知 n：**"Pantheon's client-side Get protocol is intended to be independent of the total number of tuples (n) in the key-value store"**；对比 keyword-PIR **"requires the client to know the total number of keys (n) ... and runs multiple sequential interactions"** — [待核·WebSearch 摘录](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)
- 实现库：**"Microsoft SEAL"**（"a state-of-the-art homomorphic encryption library"）；WebSearch 摘录未明确出现 "BFV" 字样 — [待核·WebSearch](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)
- 承重实验（摘要）：**"Pantheon achieves a 93× improvement for server-side latency over a state-of-the-art solution"**；ePrint 版本中 **"retrieving a value privately from a key-value store containing 64 K tuples is 107.8 seconds, with equality checking taking most of that time"**（WebSearch 摘录）— [待核·WebSearch](https://eprint.iacr.org/2023/1331.pdf)
- 后续引用：Femur（SIGMOD 2025）引 Pantheon "1.15 seconds to execute a query on only 65,536 key-value pairs" 并提出放松安全的变体 — [Femur（经 WebSearch）](https://people.iiis.tsinghua.edu.cn/~gaomy/pubs/femur.sigmod25.pdf)

### Inferences
- 由代码 `-s` 单一值大小参数与论文把"值大小"作为缩放参数，可推断 Pantheon 的评估按统一值大小运行；论文是否要求所有值等长、或对变长值做填充，原句未核到。
- 单服务器计算型（HE）设定与"公开存储、服务器持明文"由 README 直接核实。

### Gaps
- **值定长与否 / 变长处理原句**：未核。[待核]
- **安全定义/泄漏契约的 Theorem/Definition 编号与原句**（是否提及值大小、查询次数）：未核；WebSearch 明示"None of the retrieved sources analyze what an observer learns from how many queries a client sends"。[待核]
- **是否 BFV**：ePrint 摘录未提；只确认 Microsoft SEAL。[待核]
- **表号/图号**：缺失。[待核]

---

## 4. SEAL (USENIX Security 2020)

### Takeaway
SEAL 是单客户端-单服务器的可调泄漏加密数据库方案：ADJ-ORAM-α 把 N 大小数组切成 N/2^α 个区域各跑一个 ORAM，只泄漏 α bit 访问模式；ADJ-PADDING-x 把每个关键字列表填充到 x 的幂，使可观测到的不同大小只有 ⌈log_x N⌉+1 种（泄漏 log log_x N + 1 bit）。这两个参数定义的原句经 WebSearch 摘录得到，但形式化泄漏定义（Definition/Theorem 编号）未核。

### Cited Findings
- 题目/作者/venue：**"SEAL: Attack Mitigation for Encrypted Databases via Adjustable Leakage"**，Ioannis Demertzis, Dimitrios Papadopoulos, Charalampos Papamanthou, Saurabh Shintre，USENIX Security 2020；预印本 ePrint 2019/811 — [USENIX 页（被封，经 WebSearch）](https://www.usenix.org/conference/usenixsecurity20/presentation/demertzis)；[ePrint 2019/811（被封）](https://eprint.iacr.org/2019/811)
- 官方 PDF URL：`https://www.usenix.org/system/files/sec20-demertzis.pdf`；作者副本 `https://cse.hkust.edu.hk/~dipapado/docs/seal.pdf`；幻灯片 `https://www.usenix.org/system/files/sec20_slides_demertzis.pdf` — 均 [待核]（被封）。
- 动机（摘要）：**"Defenses against such leakage-abuse attacks typically require the use of Oblivious RAM or worst-case padding---such countermeasures are however quite impractical."** — [待核·WebSearch 摘录自摘要](https://eprint.iacr.org/2019/811)
- 泄漏以比特计量：**"In SEAL, the amount of privacy loss is expressed in leaked bits of search or access pattern and can be defined at setup."** — [待核·WebSearch 摘录](https://www.usenix.org/system/files/sec20-demertzis.pdf)
- **α 参数定义原句**：**"The adjustable ORAM, ADJ-ORAM-α, hides only α bits of the access pattern by partitioning the accessed N-sized array into N/2^α regions of 2^α size each and by applying an individual standard ORAM per region."**；取值范围：**"We recall that parameter α is defined in the range [0, log N] and that for α = 0 all the search/overlapping pattern bits are protected, and for α = log N all bits are leaked."** — [待核·WebSearch 摘录](https://www.usenix.org/system/files/sec20-demertzis.pdf)
- **x 参数定义原句**：**"Our padding can be parameterized by a value x that defines the number of different sizes (which are exactly ⌈log_x N⌉+ 1) that the adversary can observe."**；算法：**"Given a keyword list D(w) of size, we find the integer i such that x^i−1 < |D(w)| ≤ x^i. Then we pad the list D(w) with x^i −|D(w)| dummy entries."**；代价与泄漏：**"this padding strategy can increase the space and search overhead by a factor of x and yields leakage of log log_x N + 1 bits!"** — [待核·WebSearch 摘录](https://www.usenix.org/system/files/sec20-demertzis.pdf)
- 安全-效率折中：**"larger values for α and x yield slower but more secure SEAL"** — [待核·WebSearch 摘录](https://www.usenix.org/system/files/sec20-demertzis.pdf)
- 实验结论（摘要）：**"protecting only three to four bits of access-pattern leakage was enough to defeat existing and newer attacks, at a query slowdown of roughly one order of magnitude compared with conventional searchable-encryption databases"**（WebSearch 转述摘要）— [待核·WebSearch](https://eprint.iacr.org/2019/811)
- 版本差异：ePrint 摘要列 point/join/range 查询，作者副本另含 group-by（WebSearch 观察）— [待核·WebSearch](https://cse.hkust.edu.hk/~dipapado/docs/seal.pdf)
- 后续评价：**"SEAL does not support update operations, which is impractical in the EDB scenarios."** — [d-DSE, arXiv 2403.01182（被封，经 WebSearch）](https://arxiv.org/pdf/2403.01182)；SWAT（arXiv 2306.16851）为系统级可调泄漏后续 — [SWAT（被封）](https://arxiv.org/pdf/2306.16851)

### Inferences
- 由"applying an individual standard ORAM per region"与 SSE 范式可推断 SEAL 为单服务器、服务器持加密数据（ADJ-ORAM 与 ADJ-PADDING 都是客户端对单个不可信服务器的方案）；"single server"字样未直接核到。
- ADJ-PADDING-x 本质是把结果体积按对数档（x 的幂）分桶并泄漏档号，对应 log log_x N + 1 bit；取回对象是填充后的加密记录列表（含 dummy）。

### Gaps
- **形式化泄漏定义（Definition/Theorem 编号、Real/Ideal 实验）原句**：WebSearch 明示"The search snippets did not include the paper's formal leakage profile or security definitions"。[待核]
- **"single server"/服务器持加密数据的原句、ADJ-Search 的定义**：未核。[待核]
- **取回对象原句**、**实验图号**（latency/storage）：未核。[待核]
- **页码/节号**：全部缺失。[待核]

---

## 5. Volume-hiding SSE 一族（Kamara–Moataz Eurocrypt 2019；Patel–Persiano–Yeo–Yung CCS 2019；后续）

### Takeaway
Kamara–Moataz 首次给出"不靠 naive padding（把所有 tuple 填到同一长度）"的 volume-hiding 加密 multi-map（VLH 有损；AVLH 基于 dense subgraph transform 无损，安全依赖 planted densest subgraph 假设）；Patel 等用 cuckoo hashing + stash 构造 dprfMM（存储与查询复杂度均渐近最优），并定义 (ε,δ)-差分隐私 volume-hiding（dpMM），把查询代价从"最大 volume"降到"该键 volume + 对数项"。两者均为客户端-单服务器加密 multi-map 模型。两篇的 volume / volume-hiding 形式化定义原句本会话未核到。

### Cited Findings
**Kamara–Moataz**
- 题目/作者/venue：**"Computationally Volume-Hiding Structured Encryption"**，Seny Kamara, Tarik Moataz，Eurocrypt 2019（Springer DOI 10.1007/978-3-030-17656-3_7）；早期预印本题为 **"Encrypted Multi-Maps with Computationally-Secure Leakage"**（ePrint 2018/978）— [IACR 存档 PDF（被封）](https://www.iacr.org/archive/eurocrypt2019/114760319/114760319.pdf)；[ePrint 2018/978（被封）](https://eprint.iacr.org/2018/978)；[Springer](https://link.springer.com/doi/10.1007/978-3-030-17656-3_7)
- 官方 PDF URL：`https://www.iacr.org/archive/eurocrypt2019/114760319/114760319.pdf`（注意：题干所给 ePrint 2019/295 未被搜索结果关联到本文，ePrint 号应为 2018/978）— [待核]
- 目标（摘要，WebSearch 转述）：构造 **"volume-hiding encrypted multi-maps that hide response length from computationally-bounded adversaries"**；**"the first volume-hiding STE schemes that do not rely on naïve padding"**，即 **"padding all tuples to the same length"** — [待核·WebSearch 摘录自摘要](https://eprint.iacr.org/2018/978)
- 两个构造：**"The first is efficient in query complexity and storage, but it can lose information. The second loses none, and its storage overhead is smaller than naive padding for Zipf-distributed multi-maps"**（WebSearch 转述摘要）；信息损失 **"can be bounded with overwhelming probability for a large class of multi-maps"** — [待核·WebSearch](https://eprint.iacr.org/2018/978)
- 无损构造：**"The transform produces multi-maps represented as a dictionary and a standard multi-map, and the authors encrypt that representation with a new scheme called AVLH."**；安全假设：**"relies on the conjectured hardness of the planted densest subgraph problem"** — [待核·WebSearch 转述](https://www.iacr.org/archive/eurocrypt2019/114760319/114760319.pdf)
- 记号（来自后续文献的转述）：**"n is the maximum number of values in the multi-map, m is the number of unique keys, and t is the maximum volume of a label"**；DST 的代价 **"O(n) storage at the server ... O(ℓ log n) query complexity"**（ℓ 为最大 volume）— [待核·WebSearch 摘录自 Wang et al. ePrint 2021/765](https://eprint.iacr.org/2021/765.pdf)

**Patel–Persiano–Yeo–Yung**
- 题目/作者/venue：**"Mitigating Leakage in Secure Cloud-Hosted Data Structures: Volume-Hiding for Multi-Maps via Hashing"**，Sarvar Patel, Giuseppe Persiano, Kevin Yeo, Moti Yung，CCS 2019；ePrint 2019/1292 — [ePrint 2019/1292（被封）](https://eprint.iacr.org/2019/1292)
- 官方 PDF URL：`https://eprint.iacr.org/2019/1292.pdf`、`https://dl.acm.org/doi/pdf/10.1145/3319535.3354213` — [待核]（被封）
- volume 定义（摘要级）：**"Volume-hiding encrypted multi-maps ensure that the number of responses (volume) for any query remains hidden from the adversarial server."** — [待核·WebSearch 摘录自摘要](https://eprint.iacr.org/2019/1292)
- 构造：**"dprfMM ... the first volume-hiding encrypted multi-map whose storage and query complexity are both asymptotically optimal"**；机制：**"places each key/value pair in one of two candidate locations and pads the empty slots with dummy strings. The data owner then outsources the encrypted cuckoo hashing structure to an untrusted server."**；表大小 **"two tables of size (1+α)n for a small constant 0 < α < 1"**；**"an additional stash exponentially decreases the probability of failure"**（Kirsch et al. 变体）— [待核·WebSearch 摘录](https://eprint.iacr.org/2019/1292.pdf)
- 实验（摘要）：**"smaller server storage footprint than the best previous construction, along with a 10-16x query speedup"** — [待核·WebSearch](https://eprint.iacr.org/2019/1292)
- **差分隐私 volume-hiding 定义（WebSearch 转述）**：**"A leakage function and its scheme are (ε, δ)-differentially private volume-hiding if, for neighboring signatures with the same total number of values n, one adversary's output probability is bounded by e^ε times the other's plus δ. If δ = 0, the definition simply says ε-differentially private."**；dpMM 代价：**"Its query complexity is the volume of the queried key plus an additional logarithmic factor ... earlier volume-hiding schemes, whose query overhead was the maximum volume of any key."** — [待核·WebSearch 摘录](https://eprint.iacr.org/2019/1292.pdf)

**后续（2020–2026）volume-hiding / 分档泄漏变体（题目/venue/年份，均经 WebSearch 发现，PDF 未打开）**
- Wang, Sun, et al.? **"Dynamic Volume-Hiding Encrypted Multi-Maps with Applications to Searchable Encryption"**，ePrint 2021/765；PoPETs 2023(1) 0025 — [ePrint（被封）](https://eprint.iacr.org/2021/765)；[PoPETs 2023-0025（被封）](https://www.petsymposium.org/popets/2023/popets-2023-0025.pdf)。作者名单 [待核]。
- **"On the Cost of Suppressing Volume for Encrypted Multi-maps"**，PoPETs 2022(4) 0098 — [PoPETs（被封）](https://www.petsymposium.org/popets/2022/popets-2022-0098.pdf)。作者 [待核]。
- **"Efficient Volume-Hiding Encrypted Multi-Maps with Support for Conjunctive Queries"**，Springer（IFIP SEC 2022 系列，DOI 10.1007/978-3-031-06975-8_27）— [Springer（经 WebSearch）](https://link.springer.com/chapter/10.1007/978-3-031-06975-8_27)。作者/年份 [待核]。
- **"Practical Volume-Hiding Encrypted Multi-Maps with Optimal Overhead and Beyond"**（Jianfeng Wang 组，西电）— [作者页 PDF（被封）](https://web.xidian.edu.cn/jfwang/files/69046b3630297.pdf)。venue/年份 [待核]。
- **"Volume-Hiding Dynamic Searchable Symmetric Encryption ..."**，ePrint 2021/786 — [ePrint（被封）](https://eprint.iacr.org/2021/786.pdf)。全题/venue [待核]。
- **"Lower Bounds for Encrypted Multi-Maps and Searchable Encryption in the Leakage Cell Probe Model"**（Patel–Persiano–Yeo，CRYPTO 2020）— [ResearchGate 条目（经 WebSearch）](https://www.researchgate.net/publication/343590242)。venue [待核]。
- **"d-DSE: Distinct Dynamic Searchable Encryption Resisting Volume Leakage in Encrypted Databases"**，arXiv 2403.01182（2024）— [arXiv（被封）](https://arxiv.org/pdf/2403.01182)
- **"SWAT: A System-Wide Approach to Tunable Leakage Mitigation in Encrypted Data Stores"**，arXiv 2306.16851（2023）— [arXiv（被封）](https://arxiv.org/pdf/2306.16851)
- **"Cuckoo Hashing in Cryptography: Optimal Parameters, Robustness and Applications"**（Yeo），ePrint 2022/1455 / arXiv 2306.11220 — [arXiv（被封）](https://arxiv.org/pdf/2306.11220)
- Ghous Amjad 博士论文 **"Theoretical and Practical Advances in Structured Encryption"**，Brown 2022 — [Brown（被封）](https://cs.brown.edu/research/pubs/theses/phd/2022/amjad.ghous.pdf)

### Inferences
- 两篇基础工作均为"数据拥有者把加密 multi-map 外包给单个不可信服务器"的客户端-单服务器模型（由 "outsources the encrypted cuckoo hashing structure to an untrusted server" 与 STE 范式推断）；服务器只见密文与查询 token。
- Kamara–Moataz 明确以"naive padding = 把所有 tuple 填到最大长度"为基线并回避之；Patel 等用 cuckoo hashing 固定每次查询读取 2ℓ（最大 volume）位置并以 dummy 填充空槽，dpMM 则允许泄漏带噪声的 volume。
- Patel 等的 DP 定义中"neighboring signatures with the same total number of values n"表明总值数 n 被视为允许泄漏；"最大 volume ℓ"是否列入泄漏函数需核 Definition 原文。

### Gaps
- **Kamara–Moataz 的 volume / volume-hiding 形式化定义原句（Definition 编号）、VLH/AVLH 泄漏剖面具体列项（最大 volume、总 volume、标签数）**：未核。[待核]
- **Patel 等 Definition 编号与逐字原句**（volume-hiding leakage function 的形式化定义，是否列入 ℓ 与 n）：仅得转述。[待核]
- **后续"分桶/分档泄漏"变体是否存在以"桶编号/长度档"为泄漏粒度的 EMM 方案**：本会话未能打开任何 PDF 核实；所列题目仅为候选清单。[待核]
- **两篇的承重实验表号/图号**：缺失。[待核]
- 题干所给 "Kamara–Moataz ePrint 2019/295" 编号与搜索结果不符（搜索结果指向 2018/978）；需核。[待核]

---

## 6. Henry–Huang–Goldberg (NDSS 2013)

### Takeaway
HHG 在 Goldberg 的多服务器信息论 PIR（IT-PIR）上用 ramp 秘密共享实现"一次查询取多块"，使 PIR 块大小与记录长度解耦，从而不靠填充处理变长记录；摘要称多块查询"不向服务器泄漏取了多少块或哪些块"。它是 ℓ-server IT-PIR，不是单服务器。关于"bucketing by size / zero-padding"与"leaks the length"的具体原句，本会话未能打开 PDF 核到。

### Cited Findings
- 题目/作者/venue：**"One (Block) Size Fits All: PIR and SPIR with Variable-Length Records via Multi-Block Queries"**，Ryan Henry, Yizhou Huang, Ian Goldberg，NDSS 2013（dblp: conf/ndss/HenryHG13）— [NDSS 2013 页（被封，经 WebSearch）](https://www.ndss-symposium.org/ndss2013/ndss-2013-programme/one-block-size-fits-all-pir-and-spir-variable-length-records-multi-block-queries/)；[dblp](https://dblp.dagstuhl.de/rec/conf/ndss/HenryHG13.html)
- 官方/作者 PDF URL：`https://www.cs.uwaterloo.ca/~iang/pubs/vbspir-ndss.pdf`、`https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf`；NDSS 幻灯片 `https://www.ndss-symposium.org/wp-content/uploads/2017/09/Presentation09_1.pdf` — 均 [待核]（被封）
- 服务器设定与核心贡献（摘要，WebSearch 摘录）：**"The user fetches several b-bit blocks in a single query without revealing either the number of blocks fetched or the index of any block."**（基于 Goldberg 的 robust IT-PIR）— [待核·WebSearch 摘录](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
- 变长记录处理（摘要）：**"multi-block queries naturally lead to PIR queries over variable-length records without padding or other efficiency-harming workarounds"**；块大小解耦：**"Separating the PIR block size from individual record lengths lets the scheme fix the block size at its communication-optimal value without limiting what records can contain"**（WebSearch 转述）；最优块大小分析 **"taking into consideration both our new multi-block queries and the lengths of individual database records"** — [待核·WebSearch 摘录](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
- SPIR：**"leverages multi-block queries to construct four symmetric PIR (SPIR) protocols for databases with variable-length records. Three of our protocols support pricing."** — [待核·WebSearch 摘录](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
- 代价：**"trades off some Byzantine robustness to improve throughput without affecting privacy"** — [待核·WebSearch 摘录](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
- 技术（后续文献转述）：**"Henry et al. replace the Shamir secret sharing in Goldberg's PIR scheme with a ramp scheme."** — [待核·WebSearch 摘录自 Henry PoPETs 2016-0036](https://petsymposium.org/popets/2016/popets-2016-0036.pdf)
- 同组海报对动机的说明：**"in standard PIR, records are typically padded to a fixed length, or users must send extra queries for larger records. Doing so without leaking information forces every user to send the maximum number of queries, which increases communication cost"**（WebSearch 转述）— [IEEE S&P 2012 海报 "Symmetric PIR"（经 WebSearch）](https://www.ieee-security.org/TC/SP2012/posters/Symmetric%20PIR.pdf)
- 后续：Hafiz–Henry，PoPETs 2019 **"A bit more than a bit is more than a bit better"**（变长记录查询的后续协议）— [待核·WebSearch 提及](https://petsymposium.org/popets/2016/popets-2016-0036.pdf)；Henry PoPETs 2016 **"Polynomial Batch Codes for Efficient IT-PIR"**（题名 [待核]）称其为 HHG 多块查询的推广 — [PoPETs 2016-0036（被封）](https://petsymposium.org/popets/2016/popets-2016-0036.pdf)
- 相关单服务器变长 PIR 后续（非 HHG）：**"Densely-Packed Single-Server PIR for Variable-Length Data"**（Zenodo 记录 16740584，年份/venue [待核]）；其称 **"existing optimizations for variable-length records either add significant padding overhead or require complex key management"** — [Zenodo（经 WebSearch）](https://zenodo.org/records/16740584)

### Inferences
- HHG 属 ℓ-server、阈值 t 不合谋的信息论 PIR（Goldberg 2007 家族），数据库在各服务器为明文；客户端取回完整记录（由"fetch several blocks"推断）。
- 其隐私声明是"不泄漏取了多少块"，即 multi-block 查询本身试图隐藏记录块数；但为隐藏块数，协议需按固定的查询块数上限发送（海报中"forces every user to send the maximum number of queries"的动机描述可作侧证）——此推断需以原文 §3/§4 的具体分析核实。

### Gaps
- **"bucketing by size / zero-padding / records occupy contiguous blocks"原句**：未核。[待核]
- **"leaks the length / size of record"原句**：WebSearch 明示"I did not find a source that states the leakage of record length ℓ under zero-padding or bucketing for this scheme"。[待核]
- **ℓ 服务器数与 t 阈值的原句、实验图号（throughput/communication）**：未核。[待核]
- **SPIR 定价/分档（tier）协议细节**：未核。[待核]

---

## 7. 横向对照用速查（仅汇总上文已引事实，不含结论）

| 系统 | 服务器设定（核实程度） | 记录模型 / 变长处理 | 取回对象 | 泄漏契约粒度（已核到的部分） | 内容可见性 |
|---|---|---|---|---|---|
| Tiptoe | 单信任域，LHE+PIR；"does not require ... non-colluding servers" [直接核 README] | URL 处理方式 [待核] | 排名阶段：加密排名响应；URL 阶段：URLs [待核·WebSearch] | 原句未核 [待核] | 公开网页（推断） |
| Coeus | "does not make assumptions about the server" [待核·WebSearch]；三逻辑角色 | "pack variable-sized documents and compress the document library" [待核·WebSearch] | 打分 → 元数据 → 完整文档 | 原句未核 [待核] | 公开文档（摘要） |
| Pantheon | 单机单服务器 HE（Microsoft SEAL） [直接核 README] | `-s` 统一值大小 [直接核 README]；"arbitrarily large values" [待核] | 键对应的值 | 原句未核 [待核] | 公开 KV，服务器持明文 [直接核 README] |
| SEAL | 单服务器 SSE/ORAM（推断） | ADJ-PADDING-x：列表填到 x^i，可观测大小 ⌈log_x N⌉+1 种 [待核·WebSearch] | 加密记录列表（含 dummy） | log log_x N + 1 bit（volume）+ α bit（access） [待核·WebSearch] | 加密数据库 |
| KM 2019 / PPYY 2019 | 客户端-单服务器 EMM（推断） | KM：避免 naive padding（有损 VLH / DST+AVLH）；PPYY：cuckoo hashing + dummy 填充 + stash；dpMM 为 DP 泄漏 [待核·WebSearch] | 加密值集合 | 形式化定义原句 [待核]；dpMM 允许"same total number of values n" | 加密 multi-map |
| HHG 2013 | ℓ-server IT-PIR（Goldberg 家族），非单服务器 [待核·WebSearch] | 多块查询；"without padding" [待核·WebSearch] | 完整记录 | "without revealing either the number of blocks fetched or the index of any block" [待核·WebSearch] | 明文数据库（推断） |

### 全局 Gaps
- 本会话**没有任何一篇论文 PDF 被直接打开**；所有页码/节号/表号/图号均缺失，所有 [待核·WebSearch] 引文需在报告定稿前用所附 URL 逐条核对。
- 若后续会话的网络策略允许，优先核对顺序建议：(1) HHG 原文中关于块数/长度泄漏的句子；(2) Tiptoe §URL service 的定长/截断处理与安全定理；(3) PPYY Definition（volume-hiding leakage function）与 KM Definition；(4) SEAL 的形式化泄漏定义与"single server"措辞；(5) Coeus §2.2；(6) Pantheon 值大小与安全定义。
