# Q1：PETS / PoPETs 2024–2026 接纳证据——以 PIR / SSE / ORAM / HE / DPF 为机制、以应用问题为主语的论文

> 调研日期：2026-10-08。
> **重要方法学说明（先读）**：本会话的出站代理把 `petsymposium.org`、`moria.petsymposium.org`、`crysp.petsymposium.org`、`dblp.org`、`eprint.iacr.org`、`arxiv.org`、`zenodo.org` 全部列为策略拒绝（HTTP 403 CONNECT），因此**无法直接打开任何 issue 目录页、论文页或 PDF**。以下全部证据来自 WebSearch 返回的搜索引擎摘要片段（snippet），每条附带原始 URL 供后续核验。凡摘要中未出现的字段一律标 [待核]，未填补。**本清单不是 2024–2026 PoPETs 的完整枚举**，只是搜索可触及的子集；完整性缺口见各节 Gaps。

## 关键问题 1：PoPETs 2024/2025/2026 录用的、以应用问题为主语且机制为 PIR/SSE/ORAM/HE/DPF 的论文

### Takeaway
在可触及的搜索片段范围内，PoPETs 2024–2026 以密码私有读取组件为机制、以应用问题为主语的论文主要落在：隐私 DNS（CODoH 2026）、元数据保护消息/IoT（Mohito 2024、TEEMS 2025）、草根组织加密搜索基础设施（tigro 2026）、联邦异常检测成员查询（2024）、MPC 结构化数据分析中的 oblivious access（RingOA 2026）。与候选家族 A（私密 RAG / LLM 取回）直接对应的 PoPETs 论文**未检索到**；与 B/C 相关的多为 SSE/结构化加密线（SWiSSSE 2024、conjunctive DSSE 2025、tigro 2026）。单服务器 PIR 在该窗口只检到 1 篇纯原语论文（Pirouette 2026）。

### Cited Findings

#### 应用主语论文（逐篇）

**[2026-A1] CODoH: Privacy-Preserving Caching for Oblivious DNS over HTTPS**
- venue：PoPETs 2026, Issue 4, pp. 608–627, DOI 10.56553/popets-2026-0137 — [PoPETs 页面](https://petsymposium.org/popets/2026/popets-2026-0137.php)；[PDF](https://petsymposium.org/popets/2026/popets-2026-0137.pdf)
- 作者单位：William & Mary（第一作者 Pankaj Niroula）— [PDF](https://petsymposium.org/popets/2026/popets-2026-0137.pdf)
- 应用问题一句话：ODoH 因逐查询加密导致代理无法缓存；CODoH 把代理侧缓存放进 SGX enclave，用 ORAM 后端隐藏缓存访问模式，使代理不学到明文查询/响应。原句："CODoH places a proxy-side cache inside a trusted execution environment--namely, an Intel SGX enclave--and uses end-to-end encryption to ensure that the proxy never learns plaintext queries or cached responses." — [PDF](https://petsymposium.org/popets/2026/popets-2026-0137.pdf)
- 角色与信任模型：客户端 / 代理（持 enclave）/ 解析器；ODoH 本身要求代理与解析器不合谋（"no single party can link a client's IP address with its DNS queries"）；额外防护为解析器提供 cover responses、批量混合多客户端插入、ORAM 后端 — [PDF](https://petsymposium.org/popets/2026/popets-2026-0137.pdf)。客户端持何密钥 [待核]。
- 承重实验类型：端到端延迟（cache hit 中位延迟 18.0 ms vs ODoH 48.0 ms，2.7×；Zipf 流量 22.3 vs 48.6 ms，2.2×；SGX 开销 <1 ms）；另有 ORAM vs 线性扫描的交叉点（线性扫描在约 524k–786k 条目以内快于 PathORAM）— [PDF](https://petsymposium.org/popets/2026/popets-2026-0137.pdf)
- 形式化安全证明：[待核]
- 映射家族：C（托管 KV 的私密点查，近似；但依赖 TEE，不是纯密码单服务器）

**[2026-A2] RingOA: Fast Oblivious Access for Large-Scale Privacy-Preserving Structured Data Analysis**
- venue：PoPETs 2026, Issue 4, pp. 1088–1105；作者 Tomoki Uchiyama, Kana Shimizu（Waseda University）；带 "Artifact: Available, Functional" 徽章 — [PoPETs 2026 卷目录](https://petsymposium.org/popets/2026/)（论文编号 [待核]）
- 应用问题一句话：MPC 结构化数据分析中"取数据库条目"成为瓶颈；DPF 方案仍需与数据库规模线性的本地计算；RingOA 是首个支持 early termination 且直接输出算术份额的三方 oblivious access 协议 — [搜索摘要引自 PoPETs 2026 页](https://petsymposium.org/popets/2026/)
- 角色与信任模型：三方（3-party OA）；合谋假设 [待核]
- 承重实验类型：运行时（>10 亿条目数据库上相对 SOTA OA 协议 13.1×–15.7× 加速）— [PoPETs 2026 卷目录](https://petsymposium.org/popets/2026/)
- 形式化安全证明：[待核]
- 映射家族：B（变长列表/结构化数据的私密取回，但多方 MPC 设定，非单服务器）

**[2026-A3] tigro: Trust Infrastructure for Grassroots Organizing via …（全题 [待核]）**
- venue：PoPETs 2026，popets-2026-0052 — [PDF](https://petsymposium.org/popets/2026/popets-2026-0052.pdf)；issue [待核]
- 应用问题一句话：为草根组织提供信任基础设施，其加密搜索组件使用 structured encryption；作者指出 ORAM 可支撑加密搜索但只按地址取块，在其上构建更丰富搜索会泄漏 — [PDF](https://petsymposium.org/popets/2026/popets-2026-0052.pdf)
- 角色/信任模型、承重实验、证明：[待核]
- 映射家族：B/C（结构化加密上的关键词查找）

**[2026-A4] Enabling Personal Dataflow Sovereignty via Bolt-on Data Escrow**
- venue：PoPETs 2026，popets-2026-0147（第一作者 Zhiru Zhu）— [PDF](https://petsymposium.org/popets/2026/popets-2026-0147.pdf)；issue [待核]
- 机制片段：硬件 enclave + ORAM，使服务不学到个人联系人数据；计算等价于 PSI — [PDF](https://petsymposium.org/popets/2026/popets-2026-0147.pdf)
- 其余字段：[待核]
- 映射家族：无（联系人发现/PSI 方向，TEE 依赖）

**[2025-A1] TEEMS: A Trusted Execution Environment based Metadata-protected Messaging System**
- venue：PoPETs 2025，popets-2025-0119 — [PoPETs 页面](https://petsymposium.org/popets/2025/popets-2025-0119.php)；issue [待核]
- 应用问题一句话：基于 TEE 的元数据保护消息系统，采用 oblivious mailbox 设计；相关工作指出 PIR 式消息方案吞吐低且水平扩展受限 — [PoPETs 页面](https://petsymposium.org/popets/2025/popets-2025-0119.php)
- 其余字段：[待核]
- 映射家族：无（消息系统）

**[2025-A2] Searchable Encryption for Conjunctive Queries with Extended Forward and Backward Privacy**
- venue：PoPETs 2025, Issue 1, pp. 440–455，popets-2025-0024 — [PoPETs 页面](https://petsymposium.org/popets/2025/popets-2025-0024.php)；[PDF](https://petsymposium.org/popets/2025/popets-2025-0024.pdf)
- 应用问题一句话：动态 SSE 的合取查询；提出 SDSSE-CQ 与 SDSSE-CQ-S 两个方案，同时达成前向/后向隐私，并在 OXT 框架下新增 Type-O / Type-O- 两级后向隐私以分析合取查询泄漏 — [PoPETs 页面](https://petsymposium.org/popets/2025/popets-2025-0024.php)
- 角色/信任模型：单服务器 DSSE（客户端持对称密钥，标准 SSE 设定）[待核—摘要未明示]
- 相关工作原句（对多服务器 SSE 的评价）："a more efficient oblivious distributed DSSE based on multiple servers by utilizing the composition of Private Information Retrieval (PIR) and generic ORAM" 以及 DORY "applying a different technique (i.e., Distributed Point Function (DPF)) to multiple servers" — [PDF](https://petsymposium.org/popets/2025/popets-2025-0024.pdf)
- 承重实验、证明：[待核]（SSE 论文通常含泄漏函数定义与模拟器证明，但未核原文）
- 映射家族：B（关键词→倒排表取回）

**[2024-A1] Scalable Metadata-Hiding for Privacy-Preserving IoT Systems（系统名 Mohito）**
- venue：PoPETs 2024，popets-2024-0135；作者 Chen, Heath, Chatterjee, Fernandes — [PoPETs 页面](https://petsymposium.org/popets/2024/popets-2024-0135.php)；[PDF](https://petsymposium.org/popets/2024/popets-2024-0135.pdf)；issue [待核]
- 机制/变长处理原句：以 oblivious key-value storage 为原语；采用简单填充——"assumes all messages are set to the same size, and more efficient padding algorithms could be incorporated to reduce bandwidth overhead" — [PDF](https://petsymposium.org/popets/2024/popets-2024-0135.pdf)
- 角色/信任模型、承重实验、证明：[待核]
- 映射家族：C（弱；IoT 消息而非点查）

**[2024-A2] Privacy-Preserving Membership Queries for Federated Anomaly Detection**
- venue：PoPETs 2024，popets-2024-0074（第一作者 Jelle Vos）— [PoPETs 页面](https://petsymposium.org/popets/2024/popets-2024-0074.php)；[PDF](https://petsymposium.org/popets/2024/popets-2024-0074.pdf)；issue [待核]
- 应用问题一句话：中心方对多个参与方的数据集做集合成员查询；用椭圆曲线 ElGamal + oblivious key-value store；各方只上传一次加密数据，便于重复查询 — [PoPETs 页面](https://petsymposium.org/popets/2024/popets-2024-0074.php)
- 其余字段：[待核]
- 映射家族：无（成员测试）

**[2024-A3] SWiSSSE: System-Wide Security for Searchable Symmetric Encryption**
- venue：PoPETs 2024，popets-2024-0032（第一作者 Zichen Gui）— [PDF](https://petsymposium.org/popets/2024/popets-2024-0032.pdf)；issue [待核]
- 应用问题一句话：系统级 SSE 安全，提出 "obliviousness of operations"（服务器不能区分搜索与更新）；建于 key-value store 之上；文中称 FHE 与 ORAM 为最强但最慢的隐私手段 — [PDF](https://petsymposium.org/popets/2024/popets-2024-0032.pdf)
- 其余字段：[待核]
- 映射家族：B/C

**[2024-A4] MAPLE: MArkov Process Leakage attacks on Encrypted Search**
- venue：PoPETs 2024，popets-2024-0025（Seny Kamara 等）— [PDF](https://petsymposium.org/popets/2024/popets-2024-0025.pdf)；issue [待核]
- 性质：加密搜索的泄漏攻击论文（非构造）；背景覆盖 SSE/FHE/ORAM — [PDF](https://petsymposium.org/popets/2024/popets-2024-0025.pdf)
- 映射家族：B（作为攻击面证据，而非系统）

**[2024-A5] Finding (and Partially Fixing) Privacy Flaws in TEE-based …（全题 [待核]）**
- venue：PoPETs 2024，popets-2024-0035 — [PDF](https://crysp.petsymposium.org/popets/2024/popets-2024-0035.pdf)
- 性质：TEE 智能合约平台 KV 状态的 page-fault 泄漏；建议 ORAM/加密数据库技术缓解 — [PDF](https://crysp.petsymposium.org/popets/2024/popets-2024-0035.pdf)
- 映射家族：C（作为"访问模式泄漏需要 ORAM"的动机证据）

**[2024-A6] PRAC: Round-Efficient 3-Party MPC for Dynamic Data Structures**
- venue：PoPETs 2024，popets-2024-0100（Sajin Sasy 等）— [PDF](https://petsymposium.org/popets/2024/popets-2024-0100.pdf)；issue [待核]
- 机制：基于 Duoram（分布式 ORAM），扩展 incremental / wide DPF，构建三个 MPC 下的 oblivious 数据结构协议；报告二分搜索协议的大幅 wall-clock/带宽改进 — [PDF](https://petsymposium.org/popets/2024/popets-2024-0100.pdf)
- 映射家族：无（三方 MPC 数据结构）

**[2024-A7] SoK: Metadata-Protecting Communication Systems**
- venue：PoPETs 2024(1), pp. 509–524（Sajin Sasy 等）— [PDF](https://petsymposium.org/popets/2024/popets-2024-0030.pdf)
- 性质：SoK；按技术分类，含 "DC-nets, mixnets, Private Information Retrieval (PIR), Reverse PIR, differential privacy (DP), and secure multi-party computation (SMC)" — [PDF](https://petsymposium.org/popets/2024/popets-2024-0030.pdf)
- 映射家族：无（但说明 PETS 社区把 PIR 视为消息系统的标准构件）

**[2025-A3] SoK: Private Knowledge Sharing in Distributed Learning**
- venue：PoPETs 2025，popets-2025-0141（第一作者 Yasas Supeksala）— [PDF](https://petsymposium.org/popets/2025/popets-2025-0141.pdf)；issue [待核]
- 与家族 A 的关系：SoK 中论及 RAG："Retrieval Augmented Generation (RAG) integrates an external retrieval mechanism into LLM inference, enabling the model to access relevant knowledge from a distributed corpus during decoding" — [PDF](https://petsymposium.org/popets/2025/popets-2025-0141.pdf)。但这是 SoK，不是用 PIR/SSE 做私密 RAG 取回的系统。
- 映射家族：A（仅作为"PETS 社区已把 RAG 纳入隐私议题"的间接证据）

#### 其他相关 PoPETs 2024–2026 论文（机制含 HE/OKVS，但主语为 PSI/PSU 类，不属于私密读取）
- Multiparty Private Set Intersection Cardinality and Its Applications（2024，popets-2024-0041，基于 OKVS + OPPRF）— [PDF](https://petsymposium.org/popets/2024/popets-2024-0041.pdf)
- Summation-based Private Segmented Membership Test from Threshold-FHE（2024，popets-2024-0114，threshold FHE + OKVS；指出 ORAM 方案在多数据持有者时难扩展）— [PDF](https://petsymposium.org/popets/2024/popets-2024-0114.pdf)
- Toward A Practical Multi-party Private Set Union（2024，popets-2024-0133）— [PDF](https://petsymposium.org/popets/2024/popets-2024-0133.pdf)
- Maliciously Secure Circuit Private Set Intersection via SPDZ-Compatible Oblivious PRF（2025，popets-2025-0082）— [页面](https://petsymposium.org/popets/2025/popets-2025-0082.php)
- Private Computation on Common Fuzzy Records（2025，popets-2025-0031）— [页面](https://moria.petsymposium.org/popets/2025/popets-2025-0031.php)
- A non-comparison oblivious sort and its application to private k-NN（2025 Issue 3，popets-2025-0093，FHE 上的 oblivious sort）— [PDF](https://petsymposium.org/popets/2025/popets-2025-0093.pdf)
- Unbalanced Private Set Intersection from Client-Independent Relaxed Oblivious PRF（2025 Issue 3）— [PoPETs 2025 卷目录](https://petsymposium.org/popets/2025/)
- OPPID: Single Sign-On with Oblivious Pairwise Pseudonyms（2025 Issue 2）— [PoPETs 2025 卷目录](https://petsymposium.org/popets/2025/)
- Multi-Party Private Join（2026，popets-2026-0035，Lehmann 等，HPI；helper server 不学到交集）— [PDF](https://petsymposium.org/popets/2026/popets-2026-0035.pdf)
- Oblivis: A Framework for Delegated and Efficient Oblivious Transfer（2026 Issue 3）— [PoPETs 2026 卷目录](https://petsymposium.org/popets/2026/)
- HE 应用类（非读取）：Argmax and XGBoost Training over FHE（2026 Issue 1）；Secure Change-Point Detection for Time Series under HE（2026 Issue 2）；SentinelTouch（2026 Issue 2）；POPPY: Scalable and Secure Spectral Centrality for Distributed Graphs via HE（popets-2026-0149）；SoK: Can FHE Support General AI Computation?（popets-2026-0066）— [PoPETs 2026 卷目录](https://petsymposium.org/popets/2026/)；[POPPY 页面](https://petsymposium.org/popets/2026/popets-2026-0149.php)；[SoK 页面](https://petsymposium.org/popets/2026/popets-2026-0066.php)

### Inferences
- 在可见范围内，PoPETs 2024–2026 "应用主语 + 私有读取机制"的论文偏向 **DNS/消息/IoT/MPC 数据分析**，且多篇借助 **TEE + ORAM** 而非纯密码单服务器 PIR（CODoH、TEEMS、Bolt-on Data Escrow）。这意味着 PETS 读者熟悉"访问模式隐藏"的问题框架，但近期接纳的系统论文常把 TEE 当可接受信任根。
- 候选家族 A（私密 RAG/LLM 取回）在 PoPETs 2024–2026 **未见以 PIR/SSE 为机制的系统论文**（仅 SoK 提及 RAG）。这是空白而非反证——不可据此推断可投性。
- 家族 B/C 的最近邻是 SSE/结构化加密线（SWiSSSE 2024、conjunctive DSSE 2025、tigro 2026）与 OKVS 线；这些论文的主语多为"搜索/多方"而非"单服务器私密点查"。

### Gaps
- **无法逐 issue 枚举**：petsymposium.org / dblp.org 被代理策略拒绝，2024 Issues 1–4、2025 Issues 1–4、2026 各 issue 的完整论文列表均未获取。上面列出的 issue 号只有部分来自搜索摘要（如 2025(1) 的 0024、2024(1) 的 0030、2026(4) 的 0137/RingOA、2025(3) 的 0095/0093、2026(2) 的 Pirouette）。
- 多数论文的"角色与信任模型 / 客户端密钥 / 承重实验类型 / 是否含形式化证明"均 [待核]，因无法打开 PDF。
- 未检索到的常见应用主语：private web/wiki browsing、private contact discovery（PIR 型）、private maps、private genomic lookup、private blocklist/CT lookup、private ad/recommendation retrieval、encrypted email index——在 2024–2026 PoPETs 搜索片段中均无命中；"无命中"≠"不存在"。
- 2026 年已出版 issue 数：搜索摘要显示至少存在 Issue 1–4 条目（RingOA/CODoH 标注 Issue 4），但 2026 卷是否已全部出齐 [待核]。

## 关键问题 2：纯原语论文（供对照）——题目/年份/是否支持变长记录/是否单服务器

### Takeaway
2024–2026 PoPETs 可见的 PIR 纯原语论文只有两篇：Pirex（2025，两服务器 online-offline PIR）与 Pirouette（2026，单服务器 FHE-based PIR、36 B 查询）；二者摘要均未提及变长记录。历史对照 FrodoPIR（2023，单服务器）讨论了 keyword PIR 框架。

### Cited Findings
- **Client-Efficient Online-Offline Private Information Retrieval（系统名 Pirex）** — PoPETs 2025, Issue 3，popets-2025-0095；作者 Hoang-Dung Nguyen, Jorge Guajardo, Thang Hoang；PETS 2025 paperlist 标注 artifact reproduced — [PDF](https://petsymposium.org/popets/2025/popets-2025-0095.pdf)；[PETS 2025 paperlist](https://petsymposium.org/2025/paperlist.php)；[代码](https://github.com/vt-asaplab/pirex)
  - 服务器数：**两服务器**（"a two-server online-offline PIR scheme"，建于 CK20 两服务器蓝图）；半诚实安全 — [PDF](https://petsymposium.org/popets/2025/popets-2025-0095.pdf)
  - 性能：1 TB 数据库上取 4 KB 条目 55 ms（SOTA 9–30 s）；客户端入站带宽不随数据库规模增长 — [PDF](https://petsymposium.org/popets/2025/popets-2025-0095.pdf)
  - 变长记录：摘要未提及，固定 4 KB 条目实验 [待核是否讨论变长]
- **Pirouette: Query Efficient Single-Server PIR** — PoPETs 2026, Issue 2，popets-2026-0045；作者 Jiayi Kang, Leonard Schild（KU Leuven COSIC）；ePrint 2025/680 — [PoPETs 页面](https://petsymposium.org/popets/2026/popets-2026-0045.php)；[ePrint](https://eprint.iacr.org/2025/680)
  - 服务器数：**单服务器**，基于 FHE；查询 36 B（相对 T-Respire 9.3× 缩减、Respire 420× 缩减）；客户端只传高精度 LWE 密文的一个分量作为 transciphering 的替代 — [PoPETs 2026 卷目录](https://petsymposium.org/popets/2026/)
  - 变长记录：摘要未提及 [待核]
- **对照（窗口外）FrodoPIR: Simple, Scalable, Single-Server PIR** — PoPETs 2023，popets-2023-0022；单服务器；1M×1KB 数据库响应 <1 s，响应放大 <3.6×；讨论 PIR-by-keyword（服务器 DB 为 KV store，客户端以关键词查值）— [PoPETs 页面](https://petsymposium.org/popets/2023/popets-2023-0022.php)
- **对照（窗口外）Dynamic Volume-Hiding Encrypted Multi-Maps with Applications to Searchable Encryption** — PoPETs 2023，popets-2023-0025；结构化加密而非 PIR；原句："as responses are variable-length, EMMs are subject to volume leakage attacks"；首个完全动态的 volume-hiding EMM — [PoPETs 页面](https://petsymposium.org/popets/2023/popets-2023-0025.php)
- **对照（窗口外）XPIR: Private Information Retrieval for Everyone** — PoPETs 2016，单数据库 cPIR，基于格 — [PoPETs 页面](https://petsymposium.org/popets/2016/popets-2016-0010.php)

### Inferences
- 2024–2026 窗口内 PoPETs 录用的 PIR 原语论文极少（可见仅 2 篇），且都以"查询/客户端开销"为卖点，不以变长记录为卖点。

### Gaps
- Pirex 与 Pirouette 正文是否有 variable-size / bucketing 讨论：未能打开 PDF，[待核]。
- 可能存在未被搜索片段覆盖的 2024–2026 PoPETs PIR/ORAM 原语论文（例如两服务器 ORAM、DORAM），本次未能确认。

## 关键问题 3：PETS 2024–2026 中 variable-size records / keyword PIR / bucketing-padding / volume-hiding 相关论文；Henry–Huang–Goldberg 一脉后续

### Takeaway
在 2024–2026 PoPETs 可见范围内，**没有检索到以变长记录 PIR 或 keyword PIR 为主题的论文**；唯一明确的"填充"叙述来自 Mohito（2024，固定消息长度简单填充）。Henry–Huang–Goldberg 变长记录 PIR（NDSS 2013）在 PoPETs 2024–2026 未见作者本人后续；变长单服务器 PIR 的近期工作出现在 PoPETs 之外（2025 Zenodo 预印本、SmartPIR 2025）。

### Cited Findings
- 原始工作：Henry, Huang, Goldberg, "One (Block) Size Fits All: PIR and SPIR with Variable-Length Records via Multi-Block Queries"，NDSS 2013；核心思想为把 PIR 块大小与记录长度解耦以免填充 — [NDSS](https://www.ndss-symposium.org/ndss2013/ndss-2013-programme/one-block-size-fits-all-pir-and-spir-variable-length-records-multi-block-queries/)；[PDF](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
- Ryan Henry 在 PoPETs 2016(4):202–218 有一篇引用多块查询工作的论文（摘要截断，是否为变长记录扩展 [待核]）— [PDF](https://petsymposium.org/popets/2016/popets-2016-0036.pdf)
- 2024–2026 PoPETs 中未发现 Henry/Huang/Goldberg 的变长记录后续 — 基于 [搜索结果](https://petsymposium.org/popets/2025/)（无命中，非反证）
- PoPETs 之外的近期变长 PIR 工作（**非 PoPETs**，仅供背景）：
  - "Densely-Packed Single-Server PIR for Variable-Length Data"，Zenodo 预印本，2025-07-01；作者/venue [待核]（zenodo.org 被代理拒绝）— [Zenodo](https://zenodo.org/records/16740584)
  - "SmartPIR: A Private Information Retrieval System using Computational Storage Devices"（ACM 2025，处理变长数据低效问题）— [ACM DL](https://dl.acm.org/doi/10.1145/3725843.3756060)
  - "Private Aggregate Queries to Untrusted Databases"（arXiv 2403.13296，相关工作讨论变长记录协议）— [arXiv](https://arxiv.org/pdf/2403.13296)
  - Keyword PIR（非 PoPETs）："Practical Keyword Private Information Retrieval from Key-to-Index Mappings"（ePrint 2025/210，USENIX Security 2025；相对 ChalametPIR 通信降 15–178×）— [ePrint](https://eprint.iacr.org/2025/210)；"KPIR-C: Keyword PIR with Arbitrary Server-Side Computation"（ePrint 2025/1952）— [ePrint](https://eprint.iacr.org/2025/1952.pdf)
- PoPETs 2024 中的填充叙述：Mohito 假定所有消息同长，并指出可引入更高效填充算法 — [PDF](https://petsymposium.org/popets/2024/popets-2024-0135.pdf)
- PoPETs 2023 的 volume-hiding EMM（窗口外对照，见关键问题 2）— [PoPETs 页面](https://petsymposium.org/popets/2023/popets-2023-0025.php)

### Inferences
- 变长记录/volume-hiding 在 PETS 社区的最近一次明确处理是 2023（结构化加密线），2024–2026 可见论文未把它作为主题；该方向的新工作目前出现在 USENIX Security/ePrint/ACM 系统会议。

### Gaps
- 2024–2026 PoPETs 全文级搜索（"variable-length"、"bucket"、"padding"）未能执行，仅凭标题/摘要片段，可能漏掉在正文中处理变长的论文。
- Zenodo 预印本的作者、venue 与是否单服务器 [待核]。
