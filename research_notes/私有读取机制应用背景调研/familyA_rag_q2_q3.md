# 应用家族 A：私密 RAG / LLM agent 从共享知识库取回变长 passage —— Q2 问题存在性 与 Q3 占位检查

> **证据等级声明（必读）**：本会话的网络出口代理封锁了 arxiv.org、eprint.iacr.org、dl.acm.org、usenix.org、proceedings.iclr.cc、alphaxiv、escholarship、各高校 PDF 镜像（pdos/csail、cs.brown、northwestern、ucsb、berkeley sky）、zenodo 等全部论文托管域（WebFetch 与 curl 均返回 EGRESS_BLOCKED / CONNECT 403）。**唯一可用渠道是 WebSearch（extended 模式）返回的摘录**，以及 github.com（仅 Tiptoe 代码 README 可读）。因此下文所有"原句"均为**搜索引擎摘录中以引号或直接引语形式出现的英文句子**，未能逐句对照 PDF 核验页码/章节。凡标 [待核] 者需在可访问 PDF 的环境下复核；凡标 [转述] 者是搜索摘要的改写而非原句，**不满足本任务"只收原句"的标准**，仅作线索。章节/页码一律缺失，请视为 Gap。

---

## Q2 问题存在性：私密 RAG 社区是否明确认定 (i) 取回哪条 passage 敏感 + (ii) passage 变长 + (iii) 内容外包/加密

### Takeaway
(i) "检索了哪条文档/访问模式需对服务器隐藏"在多篇 2025–2026 私密 RAG 论文中有可引的直接表述（PIR-RAG、RAGtime-PIANO、Spruce、Formally-Verified Private RAG、Compass）；(iii) "知识库加密外包、服务器不得读内容"在 Compass、PRAG、Formally-Verified Private RAG 中有直接表述；(ii) "被取回的 passage/文档长度可变、需要定宽填充"**仅在 Spruce（arXiv 2609.03376）中找到明确原句**（fixed-width content rows / per-document byte length），其余私密 RAG 论文的摘录中未见讨论变长记录。三点同时出现在同一篇的，目前只有 Spruce（但 Spruce 是两云/多服务器模型，见 Q3）。全部原句均来自搜索摘录，章节页码缺失 [待核]。

### Cited Findings

**(i) 取回哪条 passage/文档是敏感的，须对服务器隐藏（access-pattern / query privacy）**

- PIR-RAG（Wang, Lou, Zheng, Zhao，arXiv 2509.21325，2025-09 提交；预印本，未见同行评审 venue）：论文将 RAG 中"检索内容本身"列为隐私目标——"For a true RAG application, both Graph-PIR and the Tiptoe-style system would require K additional, expensive PIR queries to privately fetch the document content for the LLM." [待核：出自搜索摘录，章节未知] — [arXiv 2509.21325](https://arxiv.org/abs/2509.21325)
- PIR-RAG 对 Tiptoe 式"只返回 ID"的批评："fundamentally misaligned with the end goal of RAG, which requires the full content of the retrieved documents, not just their IDs." [待核] — [arXiv 2509.21325 PDF](https://www.arxiv.org/pdf/2509.21325)
- RAGtime-PIANO（IACR ePrint 2026/231，预印本，作者 [待核]）："RAG-as-a-Service has emerged, in which third-party servers can be used to process client queries via an external database. Unfortunately, using such a service would expose the client's query to a third party." 及其安全声明 "neither the query, any distances, nor retrieval information is known to the database hosting server." [待核] — [ePrint 2026/231](https://eprint.iacr.org/2026/231.pdf)
- Spruce（arXiv 2609.03376，"Scalable Private Outsourced Retrieval Using Compact Embeddings"，预印本）泄漏分析原句："each cloud only sees uniformly random PIR selectors and mask corrections, plus public parameters and bucket capacity, so it learns neither the selected cluster IDs nor a query-dependent scan size." 以及残余泄漏自述："repeated full-scan candidate sets can reveal approximate unlabeled document neighborhoods even though the protocol never opens codes, embeddings, ranking, content, or queries." [待核] — [arXiv 2609.03376](https://arxiv.org/pdf/2609.03376)
- Formally-Verified Private RAGs for LLMs（Du, Huang, Ratliff 等；**课程项目报告**，非同行评审）："even if the stored content itself is encrypted, access patterns can inadvertently reveal sensitive information about user queries or document contents." 设计用 TEE 做语义搜索、Path ORAM 隐藏从不可信存储取文档的访问模式。[待核] — [PDF (royrinberg.com)](https://royrinberg.com/assets/projects/papers/class_private_RAG.pdf)
- Compass（Zhu, Patel, Zaharia, Popa；OSDI 2025，同行评审；ePrint 2024/1255）摘要："ensuring the privacy of data, queries, and results, even if the server is compromised"；并有 "In this setting, Compass also enables secure retrieval over an encrypted RAG (Retrieval-Augmented Generation) database." [待核] — [OSDI 25](https://www.usenix.org/conference/osdi25/presentation/zhu-jinhao); [ePrint 2024/1255](https://eprint.iacr.org/2024/1255)
- Tiptoe（Henzinger, Dauterman, Corrigan-Gibbs, Zeldovich；SOSP 2023，同行评审）摘要："Tiptoe is a private web search engine that allows clients to search over hundreds of millions of documents, while revealing no information about their search query to the search engine's servers." [待核] — [ePrint 2023/1438](https://eprint.iacr.org/2023/1438)
- Coeus（Ahmad, Sarker, Agrawal, El Abbadi, Gupta；SOSP 2021，同行评审）问题陈述 [转述，非原句]：用户持私密查询，从服务器的**公开**文档集合中取回 K 个最相关文档之一，服务器既不知道查询也不知道取回的是哪篇文档。— [ACM DOI 10.1145/3477132.3483586](https://dl.acm.org/doi/abs/10.1145/3477132.3483586); [NSF PAR](https://par.nsf.gov/biblio/10316017-coeus-system-oblivious-document-ranking-retrieval)

**(ii) 被取回的 passage/文档块长度可变（或讨论填充开销）**

- Spruce（arXiv 2609.03376）是唯一找到的**私密 RAG/检索论文**中明确讨论文档变长与定宽填充的："fixed-width content rows suppress per-document byte length, which blocks volume-based reconstruction attacks." 与 "fixed-width padding removes per-record byte length from setup leakage." 以及记录模型 "each padded bucket is stored as a PIR record, the record index is hidden by PIR, and the record contents remain secret-shared after retrieval." [待核] — [arXiv 2609.03376](https://arxiv.org/pdf/2609.03376)
- 非 RAG 社区但直接相关（PIR 社区对变长记录的表述，供裁决者判断"社区自己是否承认"时区分）：
  - Densely-Packed Single-Server PIR for Variable-Length Data（Zenodo 16740584，2025）："existing optimizations for variable-length records either add significant padding overhead or require complex key management."；其 packing 方法 "more stable than the padding method across various data length distributions." [待核] — [Zenodo](https://zenodo.org/records/16740584)
  - One (Block) Size Fits All: PIR and SPIR with Variable-Length Records（Henry 等，NDSS 2013）："multi-block PIR queries naturally lead to PIR queries over variable-length records without padding or other efficiency-harming workarounds."；以及 "it becomes necessary for all users to submit the maximum possible number of queries needed." [待核] — [PDF](https://cypherpunks.ca/~iang/pubs/vbspir-ndss.pdf)
  - PURBs（Nikitin 等，PETS 2019）关于 2 的幂填充开销："Padding to a power of two can impose up to 100% overhead by nearly doubling the object's size."；PADMÉ "at most 12% for tiny messages, which decreases gradually with message size." [待核] — [arXiv 1806.03160](https://arxiv.org/pdf/1806.03160)
- **未找到**：PIR-RAG、RAGtime-PIANO、PRAG、P²RAG、Compass、Pacmann、Tiptoe、Coeus 的搜索摘录中均无关于 passage/文档变长或填充开销的句子（见 Gaps）。

**(iii) 知识库内容外包/加密/第三方，服务器不应读到内容**

- PRAG（arXiv 2604.26525，"End-to-End Privacy-Preserving RAG"，预印本）："To ensure the cloud learns nothing about document contents, these operations cannot be performed on plaintext and must instead be executed entirely in the ciphertext domain." 另有 "embedding inversion attacks have demonstrated that continuous embeddings are not opaque fingerprints"（论证即使只外包 embedding 也须加密）。[待核] — [arXiv 2604.26525](https://arxiv.org/html/2604.26525v1)
- Compass（OSDI 2025）：[转述] 目标场景是用户在"自己存放于服务器上的加密机密数据"上做搜索；原句 "Compass also enables secure retrieval over an encrypted RAG (Retrieval-Augmented Generation) database." [待核] — [ePrint 2024/1255](https://eprint.iacr.org/2024/1255)
- Formally-Verified Private RAGs："even if the stored content itself is encrypted, access patterns can inadvertently reveal..."（同时覆盖 (i) 与 (iii)）。[待核] — [PDF](https://royrinberg.com/assets/projects/papers/class_private_RAG.pdf)
- Spruce 的标题即 "Private **Outsourced** Retrieval"，其泄漏分析称协议 "never opens codes, embeddings, ranking, content, or queries"。[待核] — [arXiv 2609.03376](https://arxiv.org/pdf/2609.03376)
- 反例（内容公开、非外包）：Tiptoe 在"public web crawl with 360 million pages"上搜索 [待核]；Coeus 自述为 "the first system for oblivious ranking and retrieval over public documents" [待核]。两者属于"内容公开、仅隐藏查询与访问模式"的场景，不满足 (iii)。— [ePrint 2023/1438](https://eprint.iacr.org/2023/1438); [Coeus PDF (Brown 课程镜像)](https://cs.brown.edu/courses/csci2390/2021/readings/coeus.pdf)

### Inferences
- 私密 RAG 社区对 (i) 与 (iii) 的认定是明确且多源的；对 (ii)"变长 passage + 填充开销"的认定几乎为空白，仅 Spruce 一篇用"fixed-width content rows"一句带过（且动机是防 volume 攻击而非效率）。这意味着"长度档"这一粒度的泄漏契约在该社区文献中基本没有被单独表述过——但这是基于搜索摘录的推断，需 PDF 核验。
- PIR-RAG 与 RAGtime-PIANO 把"取回文档全文"作为与 Tiptoe/Coeus 式"只取 ID/URL"的区分点，说明该社区自己已把"内容取回"识别为独立问题（见"额外"一节）。

### Gaps
- 所有引文均无章节/页码；无法确认引号内文字与 PDF 逐字一致（网络封锁）。
- Coeus 的原句（威胁模型、公开语料、变长处理）完全未获得，仅有转述。
- PIR-RAG、RAGtime-PIANO、PRAG、P²RAG 关于文档定长/变长、填充、块大小的具体描述未获得。
- Compass 论文正文中关于"RAG 数据库加密"的完整动机段落未获得，只有摘要级别句子。
- 未能检索 ACL/EMNLP/NeurIPS 侧（NLP 社区）的私密 RAG 论文是否有 (i)(ii)(iii) 表述；搜索结果以 cs.CR/系统方向为主。

---

## Q3 占位检查：已有哪些系统用 PIR / ORAM / SSE / HE 在私密 RAG / private retrieval 场景做"私密取回 passage 或文档块"

### Takeaway
已占位系统分三类：(a) 只做私密排序/打分并取回 ID 或 URL，不取内容（Tiptoe；Pacmann 取图节点/向量）；(b) 取回文档内容但以"整簇"或"固定宽度记录"为单位（PIR-RAG 整簇取回；Spruce 定宽内容行 + 两云 XOR 份额；Coeus 元数据与文档分离的 PIR；RAGtime-PIANO 对 top-ranked documents 做 PIR；P²RAG 用 SimplePIR 取文档）；(c) ORAM/TEE 路线（Compass 单服务器 ORAM 遍历 HNSW 于加密 RAG 库；Formally-Verified Private RAG 用 TEE + Path ORAM；Onyx/Hoss 用 TEE）。**没有任何一篇的摘录提到按长度档分桶、或在泄漏契约中写明"长度档/分桶/分组级"的粒度**；唯一接近的是 Spruce 的 "bucket capacity" 公开参数与 "fixed-width content rows"。单服务器且取回内容的：PIR-RAG、RAGtime-PIANO、P²RAG（三者均为预印本）、Compass（OSDI 2025，ORAM）。全部细节 [待核]。

### Cited Findings（按系统）

**1. Tiptoe — "Private Web Search with Tiptoe"，SOSP 2023（同行评审）；ePrint 2023/1438**
- 取回物：排序步骤得到最佳匹配文档的 ID，然后 "Once the client knows the IDs of the best-matching documents, it queries the URL service. Finally, from the URL service's answer, the client recovers the best documents' URLs." [待核] — [ePrint 2023/1438](https://eprint.iacr.org/2023/1438)
- 元数据 PIR："To fetch the document metadata, Tiptoe uses an existing single-server private information retrieval protocol with additional optimizations."；"By default, the Tiptoe client fetches and outputs the metadata for the top 100 search results." [待核] — 同上
- 元数据内容：默认为 URL；[转述] 论文称也可包含网页标题、摘要或图片说明。— 同上
- 内容公开：评估在 "a public web crawl with 360 million pages" 上进行 [待核]；**不取回文档全文**（搜索摘录与 GitHub README 均只显示 URL 输出："Reconstructed PIR answers. The top 10 retrieved urls are:"）— [GitHub README](https://github.com/ahenzinger/tiptoe/blob/main/README.md)
- 威胁模型：单服务器，"Tiptoe's privacy guarantee is based on cryptography alone; it does not require hardware enclaves or non-colluding servers." [待核] — [ePrint 2023/1438](https://eprint.iacr.org/2023/1438)
- 泄漏契约粒度：摘录未给出"服务器学到所选簇"的原句 [待核]；URL service 状态分 8 个 shard（README）。**变长 URL 如何处理（定长/截断/填充）：README 明确"does not describe any fixed byte length, padding, or size normalization"，论文原句未获得 [待核]。**
- 局限自述："Our protocol only allows the client to find approximate nearest neighbors—it does not produce exact results and provides no formal correctness guarantees."；"Malicious servers can decide which corpus to serve, and lie about the contents of documents." [待核] — 同上
- 性能："Running on a 45-server cluster, Tiptoe can privately search over 360 million web pages with 145 core-seconds of server compute, 56.9 MiB of client-server communication (74% of which occurs before the client enters its search query), and 2.7 seconds of end-to-end latency." [待核] — 同上

**2. Coeus — "A System for Oblivious Document Ranking and Retrieval"，SOSP 2021（同行评审），pp. 672–690**
- 取回物：[转述] 先用加密矩阵-向量积按 tf-idf 打分，再用 PIR 取回所选文档；"cuts PIR overhead by separating private metadata retrieval from document retrieval" [待核，可能为摘要原句]。— [ACM DOI](https://dl.acm.org/doi/abs/10.1145/3477132.3483586); [Brown 镜像 PDF](https://cs.brown.edu/courses/csci2390/2021/readings/coeus.pdf)
- 内容公开："the first system for oblivious ranking and retrieval over public documents" [待核]。— 同上
- 威胁模型：搜索结果中**未出现**两服务器不合谋的表述；摘要措辞为"server"（单数）[待核]。注意：ResearchGate 页面上附有一段"two distinct cloud servers…encrypted document collection"的摘要文字，搜索工具判断其属于另一篇论文，**不要当作 Coeus 的设计**。— [ResearchGate](https://www.researchgate.net/publication/355649870_Coeus_A_System_for_Oblivious_Document_Ranking_and_Retrieval)
- 变长处理：未获得任何原句 [待核]。
- 规模：[转述] Wikipedia 5M 文档、64K 关键词词典、143 台 AWS 机器，3.9 s 完成排序+取回，较基线快 24×。— [NSF PAR](https://par.nsf.gov/biblio/10316017-coeus-system-oblivious-document-ranking-retrieval)
- 后续：E-Coeus（IEEE，2024）利用 tf-idf 稀疏性，自报较 Coeus 快 23%。— [IEEE Xplore](https://ieeexplore.ieee.org/document/10538556/)

**3. PIR-RAG — arXiv 2509.21325（2025-09，预印本）**
- 记录模型：[转述] 粗粒度语义聚类 + 格基 PIR，**以整簇为取回单位**；原句 "allows for the efficient retrieval of entire document clusters, uniquely optimizing for the end-to-end RAG workflow where full document content is required." [待核] — [arXiv 2509.21325](https://arxiv.org/abs/2509.21325)
- 变长处理：未获得（整簇是否填充到等大小、簇内文档是否定长，均 [待核]）。
- 泄漏契约：未获得原句；簇 ID 是否对服务器公开 [待核]。
- 服务器数：[转述] 单服务器格基 PIR [待核]。
- 基线：对比 "Graph-PIR" 与 "Tiptoe-style private scoring"，引入 "RAG-Ready Latency" 指标 [转述]。
- 注意：PIR-RAG 将 Compass 标注为 NSDI 2024，与 OSDI 2025 实际 venue 不符（搜索工具指出的引用错误）。

**4. RAGtime-PIANO — IACR ePrint 2026/231（预印本）**
- 流程原句（搜索摘录）："the query embedding is encrypted and sent to the server for k-means centroid scoring to obtain a similarity ranking. Then a series of PIR queries is used to privately retrieve the centroid-associated embeddings and the top-ranked documents." [待核] — [ePrint 2026/231](https://eprint.iacr.org/2026/231.pdf)
- 取回物：embedding + top-ranked documents（内容）。FHE + PIR（PIANO 系单服务器 PIR，故推断为单服务器 [推断]）。
- 泄漏契约："neither the query, any distances, nor retrieval information is known to the database hosting server." [待核]
- 对比 PIR-RAG/GraphRAG：通信低至 323×、延迟低 40× [待核]。
- 变长处理、文档记录大小：未获得。

**5. Spruce — arXiv 2609.03376（预印本）"Scalable Private Outsourced Retrieval Using Compact Embeddings"**
- 记录模型："each padded bucket is stored as a PIR record, the record index is hidden by PIR, and the record contents remain secret-shared after retrieval."；内容行定宽："fixed-width content rows suppress per-document byte length" [待核] — [arXiv 2609.03376](https://arxiv.org/pdf/2609.03376)
- 泄漏契约（最细的一篇）：全扫描模式 "the per-query leakage is the public radius and the revealed set of permuted physical slots"；私密剪枝模式 "bucket IDs remain PIR-hidden"；服务器可见 "public parameters and bucket capacity"。残余泄漏量化：precision@10 为 0.017–0.070（held-out）与 0.099–0.280（20k-query 压力，L=128）；客户端填充变体（r=2）降低 39–57%。[待核]
- 服务器数：**两云 / 多服务器**（"each cloud"、"each server sends the client its XOR shares of the candidate embeddings"）[待核]。
- 取回物：候选 embedding 的 XOR 份额 → 客户端重排 → top-k；内容行亦以定宽 PIR 记录取回 [待核]。

**6. PRAG — arXiv 2604.26525（预印本）"End-to-End Privacy-Preserving RAG"**
- 路线：CKKS 同态加密；"for each encrypted sub-query and designated cluster, the server retrieves the corresponding encrypted HNSW graph from the index. It then runs greedy search from the entry point to find top-k nearest neighbors in the encrypted domain" [待核] — [arXiv 2604.26525](https://arxiv.org/html/2604.26525v1)
- 内容加密外包（见 Q2 (iii) 原句）。"designated cluster" 暗示簇 ID 对服务器可见 [推断]。
- 文档内容如何返回、变长处理：未获得。

**7. P²RAG — arXiv 2603.14778（预印本）"Efficient Privacy-Preserving RAG Service Supporting Arbitrary Top-k Retrieval"**
- [转述] 假设存在单服务器 PIR 协议做文档取回，附录以 SimplePIR 做基准；"each document text is converted to a number for the PIR operations" [待核，可能为转述]。— [arXiv 2603.14778](https://arxiv.org/pdf/2603.14778)
- 变长处理、泄漏契约：未获得。

**8. Compass — OSDI 2025（同行评审）；ePrint 2024/1255**
- 路线：在 ORAM 上遍历 HNSW；三项技术 Directional Neighbor Filtering、Speculative Neighbor Prefetch、Graph-Traversal Tailored ORAM。[转述] — [OSDI 25](https://www.usenix.org/conference/osdi25/presentation/zhu-jinhao); [slides](https://www.usenix.org/sites/default/files/conference/protected-files/osdi25_slides_zhu_jinhao.pdf)
- 威胁模型：单服务器、可被攻陷/恶意（"even if the server is compromised"）；[转述] 不依赖可信硬件。
- 取回物：摘录未说明最终返回的是邻居 ID 还是文档内容 [待核]；ORAM 块大小/变长处理 [待核]。
- 后续评价：Hoss（arXiv 2609.04522）[转述] 称 ORAM 访问仍是主导瓶颈，并称 Compass 为"current state-of-the-art in oblivious semantic search with open-source artifacts"；MESS（arXiv 2607.28999）[转述] 称自适应图遍历需多次 ORAM 操作、增加通信；Onyx（arXiv 2604.20401）[转述] 称 Compass 网络开销高、在 TEE 内运行需大量 CPU/SSD，Onyx 自报 ~12 ms 延迟、~70 QPS。— [Hoss](https://arxiv.org/html/2609.04522); [MESS](https://arxiv.org/html/2607.28999); [Onyx](https://arxiv.org/pdf/2604.20401)

**9. Pacmann — ICLR 2025（同行评审）；ePrint 2024/1600；Zhou, Shi, Fanti（CMU）**
- 摘要原句："Clients run a graph-based ANN search, where in each hop on the graph, the client privately retrieves local graph information from the server." [待核] — [ePrint 2024/1600](https://eprint.iacr.org/2024/1600); [ICLR PDF](https://proceedings.iclr.cc/paper_files/paper/2025/file/391d50b3fe1c59b3e2b8b644e0c8fe81-Paper-Conference.pdf)
- [转述] 将图 ANN 改造为与 PIR 子图取回兼容，使用"以离线预处理换在线效率"的 PIR（Piano/Quarter-PIR）；最大 1 亿向量。
- 取回物：图节点/向量，**摘录中未提及取回文档内容**；单服务器（Piano 系）[推断]；变长无涉。

**10. 其他相关占位（摘录级，均 [待核]）**
- Formally-Verified Private RAGs for LLMs（课程项目）：TEE 语义搜索 + Path ORAM 从不可信存储取文档。— [PDF](https://royrinberg.com/assets/projects/papers/class_private_RAG.pdf)
- PILLAR（arXiv 2609.36326）：稀疏倒排索引私密查找；批评 "prior PPRAG constructions rely on dense retrieval alone, translating approximate nearest-neighbor search into many query-dependent rounds of PIR." — [arXiv](https://arxiv.org/html/2609.36326)
- Quantization Enables Private Dense Retrieval against Malicious Service Providers（arXiv 2609.36376）：恶意服务器；"reduces private and verifiable retrieval to multiplication of a committed matrix by an encrypted vector"；最大 2.68M passages。— [arXiv](https://arxiv.org/html/2609.36376v1)
- Pointing the Way, Hiding the Destination（arXiv 2608.25735）："existing cryptographic approaches to private retrieval struggle to balance security with the scale and dimensionality of modern dense embeddings." — [arXiv](https://arxiv.org/pdf/2608.25735)
- Wally（Apple，arXiv 2406.06761）：批量 HE 私密最近邻，只返回加密分数/结果；生产用于 Photos Enhanced Visual Search。— [Apple ML Research](https://machinelearning.apple.com/research/homomorphic-encryption)
- Panther（单服务器私密 ANN）、RemoteRAG（扰动 embedding + 密文距离）仅在引用列表/综述中出现，未获原句。— [emergentmind 综述](https://www.emergentmind.com/topics/privacy-preserving-strategies-for-rag-systems)

### 汇总表（基于摘录，全部 [待核]）

| 系统 | venue/年 | 取回物 | 内容公开/加密 | 变长处理 | 泄漏契约写到的粒度 | 服务器模型 |
|---|---|---|---|---|---|---|
| Tiptoe | SOSP 2023 | 文档 ID → URL/元数据（PIR） | 公开 | 未知 | 未获原句；README 无填充说明 | 单服务器 |
| Coeus | SOSP 2021 | tf-idf 分数 → 文档（PIR），元数据/文档分离 | 公开 | 未知 | 未获原句 | 单服务器（摘要用单数"server"，[待核]） |
| PIR-RAG | arXiv 2025-09 | 整簇文档内容 | 未知 | 未知（整簇） | 未获原句 | 单服务器（格基 PIR）[待核] |
| RAGtime-PIANO | ePrint 2026 | centroid embedding + top-ranked documents（PIR） | 未知 | 未知 | "neither the query, any distances, nor retrieval information" | 单服务器 [推断] |
| Spruce | arXiv 2026-09 | 候选 embedding XOR 份额；定宽内容行 | 外包、不开内容 | **定宽内容行（padded bucket）** | 半径、置换槽位、bucket capacity 公开；bucket ID 隐藏 | 两云/多服务器 |
| PRAG | arXiv 2026-04 | 密文域 HNSW top-k | 加密外包 | 未知 | "designated cluster" [推断簇可见] | 单服务器 [待核] |
| P²RAG | arXiv 2026-03 | 文档（SimplePIR） | 未知 | 未知 | 未知 | 单服务器 PIR |
| Compass | OSDI 2025 | ORAM 遍历 HNSW；返回物未知 | 加密（encrypted RAG database） | 未知 | 未获原句 | 单服务器（可被攻陷） |
| Pacmann | ICLR 2025 | 图节点/向量（PIR） | 未知 | 不涉及 | 未获原句 | 单服务器 [推断] |
| Formally-Verified Private RAG | 课程项目 | 文档（Path ORAM） | 加密 | 未知 | 未知 | TEE + 不可信存储 |

### Inferences
- 现有系统要么不取内容（Tiptoe、Pacmann），要么以簇/定宽记录为单位取内容；**没有一篇在摘录层面给出"按长度档分组、公开长度档地址"的记录模型或泄漏契约**。最接近的是 Spruce 的"padded bucket + public bucket capacity"，但其为两云 XOR 份额模型。
- Tiptoe/Coeus 均在公开语料上工作，与本任务 (iii) 条件不符；它们占据的是"公开语料 + 查询/访问模式隐私"位置。

### Gaps
- 未能读取任何论文 PDF；所有"取回物/变长处理/泄漏粒度/服务器数"字段中标 [待核]/[推断]/未知 的项都需要复核。
- Coeus 的威胁模型与文档定长/分块处理未获原句；Tiptoe URL 记录的定长/截断处理未获原句。
- Compass 最终返回物（ID 还是内容）与 ORAM 块大小未获原句。
- PIR-RAG 的簇 ID 是否公开、簇是否等大小填充未获原句。
- 未覆盖 SSE 路线（searchable symmetric encryption）在 RAG 场景的具体系统；搜索结果未出现以 SSE 为主的私密 RAG 论文。
- 未覆盖 NLP venue（ACL/EMNLP/NeurIPS）中的私密检索论文（搜索结果偏 cs.CR/系统）。

---

## 额外：是否有原句承认"取回文档内容本身（而非仅排序/打分）"是未解决或开销大的问题

### Takeaway
有：PIR-RAG 明确把"为 LLM 私密取回文档内容需要 K 次额外昂贵 PIR 查询"作为对 Tiptoe 式与 Graph-PIR 基线的批评，并称"单次 PIR 跨百万文档仍过慢"；RAGtime-PIANO 把"top-ranked documents 的 PIR 取回"作为流程的独立步骤；Coeus 的摘要把"分离元数据取回与文档取回以削减 PIR 开销"作为贡献。均为搜索摘录 [待核]。

### Cited Findings
- PIR-RAG："For a true RAG application, both Graph-PIR and the Tiptoe-style system would require K additional, expensive PIR queries to privately fetch the document content for the LLM." [待核] — [arXiv 2509.21325](https://arxiv.org/abs/2509.21325)
- PIR-RAG："A single PIR query over a database of millions of documents is still too slow and resource-intensive for an interactive user experience." [待核] — 同上
- PIR-RAG（对 Tiptoe）："fundamentally misaligned with the end goal of RAG, which requires the full content of the retrieved documents, not just their IDs." [待核] — [PDF](https://www.arxiv.org/pdf/2509.21325)
- Coeus：[待核，疑为摘要原句] "cuts PIR overhead by separating private metadata retrieval from document retrieval" — [ACM DOI](https://dl.acm.org/doi/abs/10.1145/3477132.3483586)
- RAGtime-PIANO："Then a series of PIR queries is used to privately retrieve the centroid-associated embeddings and the top-ranked documents." [待核] — [ePrint 2026/231](https://eprint.iacr.org/2026/231.pdf)
- Tiptoe 自身（间接）：[转述] 默认只取回 top-100 的元数据（URL），论文称元数据"could potentially also include web-page titles, summaries, or image captions"——即内容取回不在 Tiptoe 范围内 [待核]。— [ePrint 2023/1438](https://eprint.iacr.org/2023/1438)

### Inferences
- "内容取回是独立且昂贵的步骤"已被 PIR-RAG 与 RAGtime-PIANO 明确作为其设计动机，说明该社区内部已承认此问题；但没有摘录将开销归因于"记录变长/填充"，而是归因于"PIR 次数 K"与"数据库规模"。

### Gaps
- Tiptoe 论文中是否有原句说明"取回文档全文是未来工作/开销大"：未获得。
- Compass、Pacmann 是否有类似承认：未获得。
