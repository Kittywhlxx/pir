# Q1：ACM CCS 与 IEEE S&P（2024–2026）中以 PIR / SSE / ORAM / HE / DPF 为机制的"应用主语"论文清单

> 调研环境说明（影响证据质量，请先读）：本会话的网络出口策略屏蔽了 dblp.org、dblp.uni-trier.de、sigsac.org、sp2024/2025/2026.ieee-security.org、eprint.iacr.org、arxiv.org、dl.acm.org 等几乎所有直接抓取目标（WebFetch 全部返回 EGRESS_BLOCKED；curl 对 dblp 返回 CONNECT 403）。因此本笔记**只能依赖 WebSearch 返回的检索摘要与其引用的 URL**，无法逐篇打开原文核对摘要原句、信任模型、实验类型与安全证明。凡未能从检索结果中直接读到的字段一律标 [待核]，没有填补。清单覆盖**不完整**（见各节 Gaps）。

## KQ1：CCS 2024/2025/2026 与 S&P 2024/2025/2026 中，机制为 PIR/SSE/ORAM/HE/DPF 且以应用问题为主语的论文有哪些？（纯原语单独列出）

### Takeaway
在可核到的范围内，这两个 venue 2024–2026 年录用的 PIR/ORAM 类论文**以纯原语为主**（Respire、Piano、VIA、InsPIRe、Zelda、LatORAM、Toss、ShiftPIR、BULKOR 等）；以应用问题为主语且机制为 PIR/ORAM/HE 的论文集中在三类：**元数据私密消息/OMR**（Distributed PIR、HomeRun、Group OMR、Myco）、**去中心化/区块链数据结构的私密读取**（Peer2PIR for IPFS、TreePIR for Merkle proofs）、以及**单服务器私密向量近邻检索**（Panther, CCS 2025）。未在这两个 venue 核到任何以"私密 RAG / 私密 LLM 检索"或"托管加密对象存储私密点查"为主语的录用论文；CCS 2026 录用列表尚未检索到。

### Cited Findings

#### 1) 应用主语论文（机制为 PIR / ORAM / HE / DPF / SSE）

**CCS 2024**

- **Distributed PIR: Scaling Private Messaging via the Users' Machines**（Tovey, Weiss, Gilad；CCS 2024，pp. 1967–1981，DOI 10.1145/3658644.3670350）
  - 应用问题：元数据私密消息（metadata-private messaging）的可扩展性，"proposes a metadata-private messaging architecture that offloads most computations to the clients"；核心是 "a distributed PIR protocol"，"the responder delegates its work ... and catches misbehaving delegates by efficiently verifying their results"。— [ACM DL](https://dl.acm.org/doi/10.1145/3658644.3670350)；[HUJI CRIS](https://cris.huji.ac.il/en/publications/distributed-pir-scaling-private-messaging-via-the-users-machines/)；[ePrint 2024/978](https://eprint.iacr.org/2024/978)
  - 角色与信任模型：服务器 + 大量客户端（客户端作为 PIR 代理/delegates）；存在激励机制 "the server drops clients that return invalid results"；作者称 "asymptotically improves scalability over prior work with the same threat model"。是否要求不合谋、客户端密钥 [待核]。— 同上
  - 承重实验：吞吐/可扩展性，"3.25× and 4.31× improvement over prior work, with the gap growing as the user base increases"。— 同上
  - 形式化安全证明：[待核]
  - 映射：无（消息传递，非 A/B/C）
- **HomeRun: High-efficiency Oblivious Message Retrieval, Unrestricted**（Jia, Madathil, Kate；CCS '24，DOI 10.1145/3658644.3670381）
  - 应用问题：Oblivious Message Retrieval（接收方私密取回属于自己的消息），"unrestricted" 指 "an unlimited number of pertinent messages for a recipient"。— [ACM DL PDF](https://dl.acm.org/doi/pdf/10.1145/3658644.3670381)
  - 机制/信任模型：检索摘要称其构造 "uses a two-party DPF-based PIR"，作者称 "achieve full privacy without trusted hardware or heavy machinery like FHE"。两服务器不合谋假设为推断（见 Inferences），[待核]。— 同上
  - 承重实验 / 形式化证明：[待核]
  - 映射：无
- **Simple and Practical Amortized Sublinear PIR using Dummy Subsets**（Ren, Mughees, Sun, Luo, Liao, Xu, Kirda, Lie；CCS '24，DOI 10.1145/3658644.3690266，pp. 1420–1433）— 归入"纯原语"（见下）。— [ACM DL 检索结果](https://dl.acm.org/doi/proceedings/10.1145/3658644?tocHeading=heading28)

**S&P 2024**

- **Group Oblivious Message Retrieval**（Zeyu Liu, Eran Tromer, Yunhao Wang；S&P 2024 Summer Cycle 录用列表）
  - 应用问题：群组场景的 oblivious message retrieval（私密消息/交易检测）。— [S&P 2024 accepted papers](https://sp2024.ieee-security.org/accepted-papers.html)（检索摘要引用；页面本身本会话无法打开）
  - 信任模型 / 承重实验 / 证明：[待核]
  - 映射：无

**S&P 2025**

- **Peer2PIR: Private Queries for IPFS**（Mazmudar, Veitch, Akhavan Mahdavi；S&P 2025 录用列表；IEEE Xplore 11023408；ETH Research Collection 记录发表年 2025）
  - 应用问题：IPFS 中"any query within the network leaks the queried content to other peers"；论文覆盖 "peer routing, provider advertisements, and content retrieval" 三类查询的私密化。— [arXiv 2405.17307](https://arxiv.org/abs/2405.17307)；[IEEE Xplore](https://ieeexplore.ieee.org/document/11023408/)；[ETH Research Collection](https://www.research-collection.ethz.ch/server/api/core/bitstreams/e140f525-7435-4f95-a7cf-f03562f83db2/content)；S&P 2025 归属来自 [sp2025 accepted papers 检索摘要](https://sp2025.ieee-security.org/accepted-papers.html)
  - 承重实验：延迟，"private peer-routing queries take under 100 ms and private provider-advertisement queries take about 1.5 s"。— [arXiv 2405.17307](https://arxiv.org/abs/2405.17307)
  - 信任模型（单/多服务器、是否不合谋）、客户端密钥、形式化证明：[待核]
  - 映射：C（弱：面向 P2P 内容寻址存储的私密点查，而非托管加密对象存储）
- **TreePIR: Efficient Private Retrieval of Merkle Proofs via Tree Colorings with Fast Indexing and Zero Storage Overhead**（S&P 2025 录用列表）
  - 应用问题：私密取回 Merkle 证明（"Tree nodes along a root-to-leaf path forms the well-known Merkle proof"），作者主张 "zero storage overhead is achievable for tree-shaped databases"。— [arXiv 2205.05211](https://arxiv.org/abs/2205.05211)；[Monash 记录](https://research.monash.edu/en/publications/treepir-efficient-private-retrieval-of-merkle-proofs-via-tree-col/)；[GitHub](https://github.com/PIR-PIXR/TreePIR)；S&P 2025 归属来自 [sp2025 accepted papers 检索摘要](https://sp2025.ieee-security.org/accepted-papers.html)
  - 信任模型 / 承重实验 / 证明：[待核]
  - 映射：无（接近 C：对公开树形数据结构的私密点查；记录为定长路径，非变长）
- **Myco: Unlocking Polylogarithmic Accesses in Metadata-Private Messaging**（S&P 2025 录用列表）
  - 应用问题：元数据私密消息；机制是否为 ORAM/PIR [待核]。— [sp2025 accepted papers 检索摘要](https://sp2025.ieee-security.org/accepted-papers.html)
  - 映射：无

**CCS 2025**

- **Panther: Private Approximate Nearest Neighbor Search in the Single Server Setting**（Jingyu Li, Zhicong Huang, Min Zhang, Cheng Hong, Jian Liu, Tao Wei, Wenguang Chen；CCS '25 Taipei，pp. 365–379，DOI 10.1145/3719027.3765190）
  - 应用问题：私密近似最近邻搜索（ANNS）。摘要论点：既有系统要么 "carry heavy communication costs"（Chen et al., USENIX Security 2020），要么 "depend on two non-colluding servers"（Servan-Schreiber et al., S&P 2022）；Panther "is designed to work with a single server"。— [ACM DL](https://dl.acm.org/doi/10.1145/3719027.3765190)；[ePrint 2024/1774](https://eprint.iacr.org/2024/1774)
  - 角色与信任模型：**单服务器**（1 客户端 + 1 服务器），不需要不合谋假设；机制组合 "private information retrieval, secret-sharing, garbled circuits, and homomorphic encryption"。客户端持何密钥 [待核]。— 同上
  - 承重实验：端到端延迟 + 通信量，"answer an ANNS query on 10 million points in 18 seconds with 284 MB of communication"，"more than 7.8× faster and 20× more compact than Chen et al."。— [ePrint 2024/1774](https://eprint.iacr.org/2024/1774)
  - 形式化安全证明：[待核]
  - 产物：artifact（Zenodo 17020254，"CCS B Submission #2973"）与代码 AntCPLab/OpenPanther。— [Zenodo](https://zenodo.org/records/17020254)；[GitHub AntCPLab](https://github.com/AntCPLab)
  - 映射：A（私密向量/passage 检索是私密 RAG 的检索步；但 Panther 取回的是近邻 ID/向量而非变长 passage，[待核]）
- **Differentially Private Access in Encrypted Search: Achieving Privacy at a Small Cost?**（Daniel Pöllmann, Tianxin Tang；CCS 2025；DBLP CCS 2025 页亦有收录）
  - 应用问题：加密搜索（encrypted search / SSE）中的访问模式隐私，采用差分隐私访问。— [GitHub wzx442/CCS-2025-Papers（第三方整理，标注为 AI 分类）](https://github.com/wzx442/CCS-2025-Papers)；[DBLP CCS 2025](https://dblp.org/db/conf/ccs/ccs2025.html)（检索摘要引用；页面本会话无法打开）
  - 信任模型 / 承重实验 / 证明 / 变长处理：[待核]
  - 映射：B（私密关键词查找取回倒排表，若其对象为关键词→文档列表；[待核]）

**S&P 2026**

- 检索摘要称 S&P 2026 录用列表含 "a RAG paper, but that one is about poisoned knowledge, not privacy-preserving retrieval"，以及 "an encrypted-database homomorphic execution paper"（题目未取得，[待核]）。— [sp2026 accepted papers](https://sp2026.ieee-security.org/accepted-papers.html)（检索摘要引用）
- 未在检索结果中核到 S&P 2026 以私密检索应用为主语的 PIR/ORAM 论文；见 Gaps。

**CCS 2026**

- 未找到录用列表。会议时间 "November 15 - 19, 2026 in The Hague"；程序委员会透明报告称 "final acceptance rate at 19.5% of the 981 papers in the final decision pool, and notes that this figure may still change"。— [ACM-CCS-2026/Transparency-Report](https://github.com/ACM-CCS-2026/Transparency-Report)；[CCS 2026 CFP](https://www.sigsac.org/ccs/CCS2026/call-for/call-for-papers.html)
- 有个人主页称有论文被 CCS 2026 录用，但题目未在检索摘要中出现。— [haibohu.org](https://haibohu.org/2026/04/10/one-paper-accepted-by-acm-ccs-2026/)

#### 2) 纯原语（供对照；不混入应用清单）

| 题目 | Venue/年 | 机制与模型 | 是否支持变长记录 | 来源 |
|---|---|---|---|---|
| Call Me By My Name: Simple, Practical Private Information Retrieval for Keyword Queries（ChalametPIR） | CCS 2024 | 单服务器 keyword PIR，"fast, low-bandwidth keyword queries" | [待核] | [NOVA LINCS 新闻](https://nova-lincs.di.fct.unl.pt/?p=13848)；[Brave PDF](https://brave.com/research/files/chalamet-pir.pdf) |
| Respire: High-Rate PIR for Databases with Small Records | CCS '24（DOI 10.1145/3658644.3690328） | 格基单服务器 PIR，"tailored for databases of small records" | 否（面向小定长记录；检索摘要称 "does not cover variable-length records or volume hiding"） | [ACM DL](https://dl.acm.org/doi/pdf/10.1145/3658644.3690328)；[ePrint 2024/1165](https://eprint.iacr.org/2024/1165) |
| Simple and Practical Amortized Sublinear PIR using Dummy Subsets | CCS '24（DOI 10.1145/3658644.3690266） | 预处理/摊销亚线性 PIR | [待核] | [ACM DL 检索结果](https://dl.acm.org/doi/proceedings/10.1145/3658644?tocHeading=heading28) |
| Computationally Secure Aggregation and Private Information Retrieval in the Shuffle Model | CCS 2024（作者主页列为 CCS 2024） | shuffle 模型 PIR | [待核] | [Yiping Ma 主页](https://seas.upenn.edu/~yipingma/by-year.html) |
| Oblivious Single Access Machines: A New Model for Oblivious Computation | CCS '24（DOI 10.1145/3658644.3690352，pp. 3080–3094） | ORAM 相关新模型（"each memory slot is written at most once and read at most once"） | 不适用 | [ACM DL](https://dl.acm.org/doi/abs/10.1145/3658644.3690352) |
| "hintless KsPIR scheme of Luo et al." | 被 ePrint 2026/1883 引为 CCS 2024 [待核：确切题目] | 单服务器 hintless PIR | [待核] | [ePrint 2026/1883](https://eprint.iacr.org/2026/1883.pdf) |
| Piano（Zhou et al.）[待核：完整题目] | S&P 2024 | 单服务器预处理 PIR，"gets high throughput by streaming the entire database during preprocessing" | [待核] | [IM-PIR arXiv 2509.06514 引用](https://arxiv.org/pdf/2509.06514) |
| BULKOR: Enabling Bulk Loading for Path ORAM | S&P 2024 | Path ORAM | 不适用 | [sp2024 accepted papers](https://sp2024.ieee-security.org/accepted-papers.html) |
| Distributed & Scalable Oblivious Sorting and Shuffling | S&P 2024 Summer Cycle | oblivious sort/shuffle | 不适用 | 同上 |
| Toss: Garbled PIR from Table-Only Stacking | CCS 2025 | garbled PIR | [待核] | [GitHub wzx442/CCS-2025-Papers](https://github.com/wzx442/CCS-2025-Papers) |
| ShiftPIR: An Efficient PIR System with Gravity Shifting from Client to Server | CCS 2025 | PIR 系统 | [待核] | 同上 |
| VIA: Communication-Efficient Single-Server PIR | S&P 2026，pp. 1503–1521 | 单服务器 PIR | [待核] | [sp2026 accepted papers](https://sp2026.ieee-security.org/accepted-papers.html)；[ePrint 2025/2074](https://eprint.iacr.org/2025/2074.pdf) |
| InsPIRe: Communication-Efficient PIR with Server-Side Preprocessing | S&P 2026 | 单服务器、仅服务器端预处理；"keys five times smaller and up to 50% less online query communication" vs YPIR | [待核] | [Columbia 新闻](https://www.cs.columbia.edu/2026/security-research-recognized-at-ieee-sp-2026/) |
| Zelda: Efficient Multi-server Preprocessing PIR with Unconditional Security | S&P 2026，pp. 1403–1422 | 多服务器信息论 PIR，"first concretely efficient, information-theoretic multi-server PIR scheme with sublinear computation" | [待核] | [ePrint 2025/1340](https://eprint.iacr.org/2025/1340)；[CSDL 目录](https://www.computer.org/csdl/proceedings/sp/2026/2bojuokAJK8) |
| LatORAM: ORAMs from Lateral Stashes and Delayed Shuffling | S&P 2026 | ORAM | 不适用 | [CSDL 目录](https://www.computer.org/csdl/proceedings/sp/2026/2bojuokAJK8) |
| Verifiable PIR with Small Client Storage（Rathee, Lee, Popa）| S&P 2026 [待核：作者版 PDF 仅称 "submitted to ... 2026 IEEE S&P"；检索摘要另称录用列表含 "a verifiable PIR paper"] | 基于 SimplePIR 的可验证 PIR | [待核] | [ePrint 2025/1714](https://eprint.iacr.org/2025/1714.pdf) |

### Inferences
- 两个 venue 的 PIR 录用重心在原语性能（通信、预处理、可验证性），应用主语论文多出现在"消息/OMR"和"去中心化系统（IPFS、Merkle 证明）"方向；这说明 CCS/S&P 社区对"把 PIR 嫁接到一个具体系统并以系统问题为主语"的写法有接纳先例（Peer2PIR、Distributed PIR、Panther），但先例不在 RAG/对象存储方向。
- Panther 明确把"单服务器、无不合谋假设"作为相对 S&P 2022 两服务器方案的卖点，说明单服务器设定在 CCS 被视为有价值的差异点（仅为论文自述，非本调研结论）。
- HomeRun 使用 two-party DPF-based PIR，通常意味着两服务器不合谋假设；这是基于一般背景的推断，检索摘要未确认。

### Gaps
- 未能打开 DBLP/官网/ACM DL/IEEE Xplore 的完整录用列表，清单必然遗漏；尤其 S&P 2025（257 篇）、S&P 2024、CCS 2024（DOI 10.1145/3658644）与 CCS 2025（316 篇）中可能还有以 private web search / contact discovery / DNS / blocklist / password checkup / ad retrieval / navigation / oblivious file system 为主语的论文，本会话一篇都未能核到（既不能确认存在也不能确认不存在）。
- CCS 2026 录用列表未检索到。
- 每篇论文的"客户端持什么密钥""是否含形式化安全证明""承重实验类型"几乎全部 [待核]，因无法读原文。
- "Piano" 与 "KsPIR/Luo et al." 的完整题目未能核实。
- S&P 2026 列表中"encrypted-database homomorphic execution paper"题目未取得。

## KQ2：这些应用主语论文的角色/信任模型、承重实验、形式化证明与 A/B/C 映射

### Takeaway
可核到的信息中，只有 Panther（单服务器）、Distributed PIR（服务器 + 客户端代理，带激励/验证）与 HomeRun（two-party DPF PIR）的信任模型能从检索摘要读出；承重实验类型可核到的是延迟（Peer2PIR、Panther）、通信量（Panther、InsPIRe）、可扩展性倍数（Distributed PIR）。形式化证明字段全部 [待核]。映射结果：A ← Panther（弱）；B ← Differentially Private Access in Encrypted Search（弱，[待核]）；C ← Peer2PIR（弱）；其余为"无"。

### Cited Findings
- Panther：单服务器；机制组合 PIR + secret sharing + garbled circuits + HE；10M 点 18 s / 284 MB；相对 Chen et al. 7.8× 更快、20× 更省通信。— [ePrint 2024/1774](https://eprint.iacr.org/2024/1774)；[ACM DL](https://dl.acm.org/doi/10.1145/3719027.3765190)
- Distributed PIR：客户端承担大部分计算，服务器验证代理结果并剔除返回无效结果的客户端；3.25×/4.31× 优于先前工作，"same threat model"。— [HUJI CRIS](https://cris.huji.ac.il/en/publications/distributed-pir-scaling-private-messaging-via-the-users-machines/)；[ePrint 2024/978](https://eprint.iacr.org/2024/978)
- HomeRun：two-party DPF-based PIR；无可信硬件、无 FHE。— [ACM DL PDF](https://dl.acm.org/doi/pdf/10.1145/3658644.3670381)
- Peer2PIR：延迟 <100 ms（peer routing）、≈1.5 s（provider advertisement）。— [arXiv 2405.17307](https://arxiv.org/abs/2405.17307)
- InsPIRe（原语）：相对 YPIR "keys five times smaller and up to 50% less online query communication"。— [Columbia 新闻](https://www.cs.columbia.edu/2026/security-research-recognized-at-ieee-sp-2026/)
- Zelda（原语）：评估对象为 "online response performance"，对比 QuarterPIR（Eurocrypt'24）。— [ePrint 2025/1340](https://eprint.iacr.org/2025/1340)

### Inferences
- 应用主语论文在 CCS/S&P 的承重实验倾向"端到端延迟 + 通信量 + 相对既有系统的倍数"，而非效果指标（如检索召回率）；RAG 类效果指标在这两个 venue 没有可核先例。

### Gaps
- 所有论文的形式化安全证明、客户端密钥持有情况、是否要求不合谋（除 Panther 明确单服务器外）均 [待核]。
- Myco、Group OMR、TreePIR、Differentially Private Access in Encrypted Search 的机制细节与实验类型未取得。

## KQ3：2024–2026 CCS/S&P 中与变长记录 / keyword PIR / 稀疏或分桶数据库 / volume-hiding 相关的论文及其变长处理方式

### Takeaway
在 CCS/S&P 2024–2026 可核范围内，**没有找到以变长记录或 volume-hiding 为核心贡献的 PIR 论文**；最接近的是 CCS 2024 的 keyword PIR（ChalametPIR）与小记录 PIR（Respire，定长小记录），以及 CCS 2025 的 DP 访问加密搜索。真正处理变长记录的工作出现在其他 venue：EuroS&P 2025 "Densely-Packed Single-Server PIR for Variable-Length Data"、NDSS 2023 "One (Block) Size Fits All"、NDSS 2026 "BKPIR"，以及 ePrint 预印本 Npir。

### Cited Findings
- 检索（volume-hiding + variable-length + keyword PIR + CCS/S&P 2024–2025）结论："I didn't find a paper that matches all four criteria"。— [WebSearch 汇总，引用 ePrint/zenodo/NDSS 等](https://zenodo.org/records/16740584)
- **Densely-Packed Single-Server PIR for Variable-Length Data**（EuroS&P 2025，非正会 S&P）：变长处理方式为"密集打包"，"packs variable-length entries as efficiently as symmetric-key schemes but without any additional key management, and guarantees that each query retrieves at most one record with the strongest privacy guarantees"；检索摘要称其 "does not address volume hiding or keyword search"。— [Zenodo 16740584](https://zenodo.org/records/16740584)
- **Npir: High-Rate PIR for Databases with Moderate-Size Records**（ePrint 2025/2257，venue [待核]）：检索摘要称其 "describes support for variable record sizes"，但 "volume hiding is not its focus"。— [ePrint 2025/2257](https://eprint.iacr.org/2025/2257.pdf)
- **Respire**（CCS 2024）：面向小记录，不处理变长/volume-hiding。— [ePrint 2024/1165](https://eprint.iacr.org/2024/1165)
- **More Efficient (Hintless) Keyword Private Information Retrieval**（ePrint 2026/1883，venue [待核]）：将 "the hintless KsPIR scheme of Luo et al." 扩展到 keyword 设定；检索摘要未见变长/volume-hiding。— [ePrint 2026/1883](https://eprint.iacr.org/2026/1883.pdf)
- **BKPIR: Keyword PIR for Private Boolean Retrieval**（NDSS 2026，非目标 venue）："handles keywords mapped to sets of values"（一对多，即变长值列表）。— [NDSS 2026 PDF](https://www.ndss-symposium.org/wp-content/uploads/2026-s536-paper.pdf)
- **One (Block) Size Fits All: PIR and SPIR with Variable-Length Records via Multi-Block Queries**（NDSS，年份在范围外 [待核：2023]）：变长处理为多块查询。— [cypherpunks.ca PDF](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
- **Don't be Dense: Efficient Keyword PIR for Sparse Databases**（ePrint 2023/466，范围外，作为稀疏数据库 keyword PIR 背景）。— [ePrint 2023/466](https://eprint.iacr.org/2023/466.pdf)
- **Veil: A Storage and Communication Efficient Volume-Hiding Algorithm**（arXiv，非 PIR）：volume-hiding 方式为 "partitions the dataset by randomly mapping keys to a set of equi-sized buckets"。— [arXiv 2310.12491](https://arxiv.org/pdf/2310.12491)
- **Pantheon: Private Retrieval from Public Key-Value Store**（VLDB，范围外 venue，KV 私密点查背景）。— [VLDB PDF](https://www.vldb.org/pvldb/vol16/p643-ahmad.pdf)
- ChalametPIR（CCS 2024）：单服务器 keyword PIR；变长支持 [待核]。— [NOVA LINCS](https://nova-lincs.di.fct.unl.pt/?p=13848)

#### 附：私密 RAG / LLM 检索相关（均未核到 CCS/S&P 录用）
- RAGtime-PIANO: Efficient Secure Remote RAG（ePrint 2026/231）：FHE 对查询 embedding 加密做 k-means centroid 打分，再 "a series of PIR queries retrieves the centroid-associated embeddings and top-ranked documents"。— [ePrint 2026/231](https://eprint.iacr.org/2026/231.pdf)
- GoldenRetriever: Non-Interactive Homomorphic Encrypted Retrieval for Privacy-Preserving RAG（arXiv 2607.29019）：阈值选择而非 top-k 排序，"reduces computational complexity from quadratic to linear in the corpus size"。— [arXiv](https://arxiv.org/abs/2607.29019)
- PRAG: End-to-End Privacy-Preserving RAG（arXiv 2604.26525）：CKKS；PRAG-I 非交互、PRAG-II 客户端辅助。— [arXiv](https://arxiv.org/abs/2604.26525)
- P²RAG（arXiv 2603.14778）、PIR-RAG（arXiv 2509.21325，被 P²RAG 引用）、Spruce: Scalable Private Outsourced Retrieval Using Compact Embeddings（arXiv 2609.03376）、Pointing the Way, Hiding the Destination: Practical Private Dense Retrieval at Scale（arXiv 2608.25735）：venue 均 [待核]。— [P²RAG](https://arxiv.org/pdf/2603.14778)；[Spruce](https://arxiv.org/pdf/2609.03376)；[Pointing the Way](https://arxiv.org/pdf/2608.25735)
- RemoteRAG：ACL 2025 Findings（非安全 venue）。— [ACL 2025 Findings](https://2025.aclweb.org/program/find_papers/)
- 其他 venue 未知的相关预印本：OblivCDN（arXiv 2501.07262）、Onyx: Cost-Efficient Disk-Oblivious ANN Search（arXiv 2604.20401）、BOLT 不经意 map（arXiv 2509.01742）、Bringing Private Reads to Hyperledger Fabric via PIR（arXiv 2511.02656）。— [OblivCDN](https://arxiv.org/pdf/2501.07262)；[Onyx](https://arxiv.org/pdf/2604.20401)；[BOLT](https://arxiv.org/pdf/2509.01742)；[Hyperledger PIR](https://arxiv.org/pdf/2511.02656)

### Inferences
- 变长记录 PIR 的已发表先例（EuroS&P 2025、NDSS 2023/2026）采用"密集打包""多块查询""keyword→集合"三种路径；"按 2^k 容量档分桶成公开信封"的做法在 CCS/S&P 2024–2026 可核论文中没有直接对应，volume-hiding 分桶思路可见于 SSE 文献（Veil）而非 PIR。
- 私密 RAG 的密码学方案（FHE+PIR 两步、HE 阈值检索）在 2026 年集中出现在预印本，尚未核到 CCS/S&P 录用，说明该应用在这两个 venue 的接纳证据目前为空（而非负面）。

### Gaps
- 无法打开 ePrint/arXiv 原文，上述变长处理描述均来自检索摘要转述；"One (Block) Size Fits All" 的年份、Npir 与 ePrint 2026/1883 的 venue 未核。
- 未能检索到 CCS/S&P 2024–2026 中是否有 SSE 方向的 volume-hiding 论文（关键词检索未命中，不能断定不存在）。
