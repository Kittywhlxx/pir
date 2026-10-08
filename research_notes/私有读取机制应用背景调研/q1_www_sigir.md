# Q1 接纳证据：WWW（The Web Conference）与 SIGIR，2024–2026，密码学私有检索组件（PIR / SSE / ORAM / HE / MPC）作为机制、应用问题作为主语的正会论文

> 检索环境说明（影响覆盖面，必读）：本会话的网络出口代理封锁了 dblp.org、dl.acm.org、doi.org 直连、arxiv.org、eprint.iacr.org、sigweb.org、paperdigest.org、papercopilot.com、bohrium.com、sigir2026.org、sigir2025.dei.unipd.it、sigir-2024.github.io、www2024/2025/2026.thewebconf.org、dei.unipd.it 等全部一手目录站点（WebFetch 返回 EGRESS_BLOCKED；curl 返回 CONNECT 403；`gh api` 对非会话仓库返回 403）。因此本笔记**只能依赖 WebSearch 的搜索摘要（snippet）**来读取 DBLP/ACM DL/会议 accepted-papers 页面的片段，无法逐页完整扫描任何一年的接收列表。下文每个 venue-年份的"未找到"均指"在上述受限检索方式下未找到"，不等于"不存在"。所有 URL 均来自搜索结果，题目保留英文原文；未能核读原文的字段标 [待核]。

## 关键问题 1：WWW 2024 / 2025 / 2026 与 SIGIR 2024 / 2025 / 2026 正会录用了哪些"密码组件为机制、应用问题为主语"的论文？

### Takeaway
在受限检索下，六个 venue-年份中只找到 **2 篇**机制含密码组件（MPC / HE）的正会论文：WWW 2024 的 "Differentially Private Selection from Secure Distributed Computing"（MPC+DP，主语是"分布式私密选择/最流行项"）和 WWW 2026 的 "Reliable Non-Leveled Homomorphic Encryption for Web Services"（FHE 框架，主语偏机制而非单一应用）。**SIGIR 2024、2025、2026 三年均未找到任何以 PIR/SSE/ORAM/HE/MPC 为机制的正会论文**；SIGIR 侧与隐私相关的论文以 DP、查询混淆（obfuscation）、联邦学习、攻击研究和 perspective/resource 论文为主。WWW 2025 也未找到。另有若干 WWW 2026 Security & Privacy track 的密码学相关论文（浏览器 E2EE、匿名账号所有权证明）但与"私有读取/检索"无关。

### Cited Findings

**WWW 2024（ACM Web Conference 2024，Singapore；proceedings DOI 10.1145/3589334）**

- 论文 1：**"Differentially Private Selection from Secure Distributed Computing"** — Ivan Damgård, Hannah Keller, Boel Nelson, Claudio Orlandi, Rasmus Pagh。WWW '24 research track，pp. 1103–1114。 — [ACM DL / DOI](https://doi.org/10.1145/3589334.3645435)；[IACR ePrint 2023/894](https://eprint.iacr.org/2023/894)；[arXiv 2306.04564](https://arxiv.org/pdf/2306.04564)；列于 [WWW 2024 accepted research-track 页面](https://www2024.thewebconf.org/accepted/research-tracks/)（搜索摘要可见该条目）。
  - 应用问题一句话：多方各持一个二值向量 {0,1}^d，私密地输出其和中近似最大项的索引；摘要给出的用例是 "recommending a popular item based on user feedback" 和 "releasing statistics on the most popular web sites"，以及超参/特征选择（引自搜索摘要转述的 ACM 版摘要）。
  - 机制：secure multi-party computation（integer secret sharing，因通用 MPC 不可扩展）+ differential privacy。
  - 角色与信任模型：至少 3 台服务器，**honest-majority（诚实多数）假设**，即要求服务器间不合谋超过少数；用户只提交秘密分享，不持有解密密钥（基于摘要转述，[待核]原文细节）。
  - 承重实验：摘要称 "analyze both utility and efficiency theoretically and empirically"，并称相对 Champion–shelat–Ullman (CCS'19) "substantially reducing communication costs"；效用（utility）对比中心模型机制。具体延迟/吞吐数值 [待核]。
  - 形式化安全证明：ePrint 版本称给出 "information-theoretic guarantees"；DP 定理与 MPC 安全性应为形式化（密码学作者群），但本会话未能读到定理原文 [待核]。
  - 映射家族：**无**（它是聚合统计/选择，不是对变长记录的私有读取）。
- WWW 2024 accepted research-track 页面搜索摘要中其他含 privacy/private/secure 字样的题目（均非密码组件，不计入）："Towards Personalized Privacy: User-Governed Data Contribution for Federated Recommendation"；"From Promises to Practice: Evaluating the Private Browsing Modes of Android Browser Apps"；"GEES: Enabling Location Privacy-Preserving Energy Saving in Multi-Access Edge Computing"；"SSI, from Specifications to Protocol? Formally Verify Security!"；"Poisoning federated recommender systems with fake users"（pp. 3555–3565）。 — [WWW 2024 accepted research tracks](https://www2024.thewebconf.org/accepted/research-tracks/)；[ACM DL WWW '24](https://dl.acm.org/doi/proceedings/10.1145/3589334)
- 检索方式：WebSearch 针对 www2024.thewebconf.org accepted 页面、ACM DL WWW '24 proceedings、"ACM Web Conference 2024" + PIR/oblivious/encrypted search/homomorphic/secure two-party 等关键词；DBLP API（dblp.org/search/publ/api, venue:WWW）被代理封锁未能执行。

**WWW 2025（ACM Web Conference 2025，Sydney；proceedings DOI 10.1145/3696410；Companion 10.1145/3701716）**

- **未找到**任何以 PIR/SSE/ORAM/HE/MPC 为机制的论文。搜索摘要中唯一含 privacy 字样的 WWW '25 题目是 "Effectiveness of Privacy-preserving Algorithms in LLMs: A Benchmark and Empirical Analysis"（主题为 LLM 隐私算法基准，机制 [待核]，从题目看非密码组件）。 — [ACM DL WWW '25 proceedings](https://dl.acm.org/doi/proceedings/10.1145/3696410?tocHeading=heading43)
- WWW 2025 research track 规模：一份第三方 GitHub PR 记录 "2025: 409 selected for publication / 2500 full research submissions"（二手数据）。 — [ccfddl PR #2208](https://github.com/ccfddl/ccf-deadlines/pull/2208)
- 检索方式：WebSearch 针对 www2025.thewebconf.org/accepted-papers、ACM DL 10.1145/3696410、"ACM Web Conference 2025" + homomorphic / secure multi-party / trusted execution / private information retrieval；三次不同措辞均未命中 WWW '25 论文。papercopilot 的 WWW 2025 列表与 sigweb.org TOC 均被封锁。

**WWW 2026（ACM Web Conference 2026，Dubai；proceedings DOI 10.1145/3774904；research track 676/3370=20%；Industry track 本届并入主 proceedings）**

- 论文 2：**"Reliable Non-Leveled Homomorphic Encryption for Web Services"** — Baigang Chen (U. Minnesota, Twin Cities), Dongfang Zhao (U. Washington, Tacoma)。WWW '26 pp. 3171–3182，ACM 页面发布日期 2026-04-12；index terms "Security and privacy"，keywords "privacy-preserving machine learning; fully homomorphic encryption"。accepted research-track 页面编号 rfp2521（所属 track 标题在搜索摘要中与 Security and Privacy 相邻，[待核]）。 — [DOI](https://doi.org/10.1145/3774904.3792476)；[arXiv 2508.02943](https://arxiv.org/html/2508.02943v2)；[WWW 2026 accepted research tracks](https://www2026.thewebconf.org/accepted/research-tracks.html)
  - 应用问题一句话：面向"web services"的可靠 FHE 计算框架；验证场景为加密推理（MNIST、CIFAR-10）、非 IID 联邦聚合、家庭用电流式分析——**主语更接近机制/框架而非单一应用问题**（本条按证据标准列出，但注意它不是"应用为主语"的典型样本）。
  - 机制：非 leveled 的 CKKS 变体（固定模数，用轻量 Refresh 取代 Rescale）+ 基于置换交织器（permutation-based interleaver）的代数纠错层。
  - 角色与信任模型：标准单服务器 HE 外包计算，客户端持 HE 私钥 [待核]（基于 CKKS 常规设定推断，未读原文）。
  - 承重实验：端到端延迟（与 leveled CKKS 相当或更优，尤其 N=8192）；突发信道错误下任务准确率与明文基线差 ≤0.5 个百分点（引自搜索摘要转述的 introduction）。
  - 形式化安全证明：[待核]。
  - 映射家族：**无**（非检索/读取；最接近 C 的"托管加密计算"但不是点查）。
- WWW 2026 accepted research-track 页面（搜索摘要）中其他含关键词的题目（均不属 PIR/SSE/ORAM 私有读取；机制是否含密码组件按条标注）：
  - "Trust on Reload: Securing Browser-Based End-to-End Encryption" — Security and Privacy track；CISPA 页面标注 venue 为 The Web Conference (WWW) 2026，日期 2026-04-12；主题是 web 应用 E2EE 的客户端代码可信分发问题（密码学应用，但与检索无关；DOI [待核]）。 — [CISPA 出版页](https://cispa.de/en/research/publications/213253-trust-on-reload-securing-browser-based-end-to-end-encryption)；[WWW 2026 accepted research tracks](https://www2026.thewebconf.org/accepted/research-tracks.html)
  - "YouChoose: A Lightweight Anonymous Proof of Account Ownership" — rfp4558，Security and Privacy track；机制 [待核]（匿名证明，可能为密码学原语），非检索。 — [WWW 2026 accepted research tracks](https://www2026.thewebconf.org/accepted/research-tracks.html)
  - "Verifiable Federated Representation Learning for Cross-domain Sequential Recommendation" — DOI 10.1145/3774904.3792533；"verifiable" 是否基于密码学承诺/ZK [待核]；联邦为主。 — [ACM DL](https://dl.acm.org/doi/10.1145/3774904.3792533)
  - "PrivSplit: A Lossless Method for Prompt Privacy in Distributed Parameter-Efficient Fine-Tuning"（rfp0379，Security and Privacy）、"DIARY: Differentially Private Recovery ... Federated Unlearning"（rfp2143，DP）、"Differentially Private Anonymous Bandits for Multi-User Systems"（rfp3786，DP）、"The Devil Within, The Cure Without: Securing Locally Private Graph Learning under Poisoning"（rfp0310，LDP）、"FeedGuard: Online Critic-Guided RL with Privacy-Preserving Feedback for Recommendation"（机制 [待核]）——均非密码组件或待核，不计入。 — [WWW 2026 accepted research tracks](https://www2026.thewebconf.org/accepted/research-tracks.html)
  - 短文 track 搜索摘要中最接近安全主题的是 "Access Controlled Website Interaction for Agentic AI with Delegated Critical Tasks"（srp205，访问控制，非密码学）。 — [WWW 2026 short papers](https://www2026.thewebconf.org/accepted/short-papers.html)
- 第三方博客称 WWW 2026 Security and Privacy track 有 88 篇、Search and Retrieval-Augmented AI track 有 60 篇（二手，未核）。 — [Bohrium blog](https://www.bohrium.com/en/blog/www-2026-accepted-papers/)
- 检索方式：WebSearch 针对 www2026.thewebconf.org/accepted.html、/accepted/research-tracks.html、/accepted/short-papers.html、ACM DL 10.1145/3774904（含 tocHeading=heading6 "Track 5: Security and Privacy"）、sigweb.org/toc/www26a.html（封锁）；industry track 列表未能读取。

**SIGIR 2024（47th SIGIR，Washington DC；proceedings DOI 10.1145/3626772）**

- **未找到**任何以 PIR/SSE/ORAM/HE/MPC 为机制的正会论文。
- 该届有专门 session "Privacy, Security and Federated Learning"（ACM DL 与 DBLP 均有此 session 标题）。搜索摘要可见的该 session 论文：
  - "Untargeted Adversarial Attack on Knowledge Graph Embeddings"（攻击，非密码）
  - "Poisoning Decentralized Collaborative Recommender System and Its Countermeasures"（攻击，非密码）
  - "ReFer: Retrieval-Enhanced Vertical Federated Recommendation for Full Set User Benefit"（pp. 1763–1773；垂直联邦 + 检索增强，摘要未提密码组件）
  - 一篇 LDP-Cd（local differential privacy 社区检测，纯 DP，排除）
  - "Revisit targeted model poisoning on federated recommendation"（攻击）
  — [SIGIR '24 proceedings 页](https://sigir-2024.github.io/proceedings.html)；[ACM DL SIGIR '24](https://dl.acm.org/doi/proceedings/10.1145/3626772)；[DBLP SIGIR 2024](https://dblp.org/db/conf/sigir/sigir2024.html)；[ReFer PDF](https://still2009.github.io/pdf/ReFer_SIGIR24_v0624.pdf)
- 检索方式：WebSearch 针对 sigir-2024.github.io/proceedings.html 与 accepted_papers.html、ACM DL、DBLP sigir2024、"SIGIR '24" + homomorphic/encrypted/secure computation/oblivious/secret sharing；DBLP API 被封锁。

**SIGIR 2025（48th SIGIR，Padua；proceedings DOI 10.1145/3726302）**

- **未找到**任何以 PIR/SSE/ORAM/HE/MPC 为机制的正会论文。
- 搜索摘要可见的隐私相关正会论文（均非密码组件）：
  - [short] "Private Preferences, Public Rankings: A Privacy-Preserving Framework for Marketplace Recommendations" — Guilherme Ramos, Ludovico Boratto, Mirko Marras；pp. 2577–2581；机制为 "seller representative users" 编码卖家排序 + 推荐机制（非密码学）；应用：多卖家市场的跨卖家隐私泄漏；实验：多个真实数据集用例的效果指标（具体指标 [待核]）。 — [DOI](https://doi.org/10.1145/3726302.3730237)；[SIGIR 2025 accepted papers](https://sigir2025.dei.unipd.it/accepted-papers.html)；[作者博客](https://www.ludovicoboratto.com/private-preferences-public-rankings-a-privacy-preserving-framework-for-marketplace-recommendations/)
  - [short] "A Comparative Study of Large Language Models and Traditional Privacy Measures to Evaluate Query Obfuscation Approaches" — De Faveri, Faggioli, Ferro 等；查询混淆评估，非密码。 — [SIGIR '25 proceedings 页](https://sigir2025.dei.unipd.it/proceedings.html)
  - [full] "FedCIA: Federated Collaborative Information Aggregation for Privacy-Preserving Recommendation"（联邦；是否含密码组件 [待核]，arXiv 2504.14208）。 — [SIGIR 2025 accepted papers](https://sigir2025.dei.unipd.it/accepted-papers.html)；[arXiv](https://arxiv.org/pdf/2504.14208)
  - 一篇关于 sentence embedding 泄漏/生成式嵌入反演攻击的 SIGIR '25 论文（攻击研究，题目 [待核]）。 — [搜索来源：arXiv 2507.04942](https://arxiv.org/html/2507.04942v2)
- 检索方式：WebSearch 针对 sigir2025.dei.unipd.it/accepted-papers.html 与 proceedings.html、"48th International ACM SIGIR" + homomorphic/encrypted/secure aggregation/TEE/secret sharing、"SIGIR 2025" + PIR/dense retrieval/private；页面本身被封锁。

**SIGIR 2026（49th SIGIR，Melbourne，2026-07-20~24；proceedings DOI 前缀 10.1145/3805712；ACM 页面称接收率 234/1271 = 18%，官方新闻称各 track 共 656 篇）**

- **未找到**任何以 PIR/SSE/ORAM/HE/MPC 为机制的正会论文。
- 搜索摘要可见的隐私相关正会论文（官方 accepted-papers 页带 track 标签）：
  - [pr, perspective] **"Privacy Preserving Information Retrieval: Defining Privacy Research Pillars for a Future Research Agenda"** — Francesco Luigi De Faveri, Guglielmo Faggioli, Asia J. Biega, Nicola Ferro（Padova + MPI-SP）。摘要：分析现代检索系统（含生成式流水线）中用户隐私保护现状，提出基于文献缺口、用户调查和专家访谈的"研究支柱"研究议程。**是否讨论 PIR/HE/SSE 等密码学路径 [待核]**（作者组既往工作集中于查询混淆与 DP）。 — [DOI](https://doi.org/10.1145/3805712.3808547)；[作者 PDF](https://www.dei.unipd.it/~ferro/papers/2026/SIGIR2026-DFBF.pdf)；[SIGIR 2026 accepted papers](https://sigir2026.org/en-AU/pages/program/accepted-papers)；[Biega 主页](https://asiabiega.github.io/)
  - [rr, resource] "A Sensitivity-Aware Test Collection for Search Among Personal Information" — McKechnie, McDonald, Macdonald（敏感性感知测试集，非密码）。 — [SIGIR 2026 accepted papers](https://sigir2026.org/en-AU/pages/program/accepted-papers)
  - [fp] "How Far Are We from Automatically Identifying Violations of the Data Minimization Principle in Privacy Policies?" — Ziyan Zhou 等（隐私政策 NLP，非密码）。 — [SIGIR 2026 accepted papers](https://sigir2026.org/en-AU/pages/program/accepted-papers)
- 第三方 gist 统计：668 篇会议论文中 "Fairness, Bias, Privacy & Safety" 类 10 篇、RAG 类 48 篇（该 gist 76 万字符，本会话只读到前 10 万字符，未能读到隐私类题目）。 — [gist](https://gist.github.com/rankun203/4f1a77c2705b2945417d0e1c581c4859/ed5a208b9b628e02bef68a0a809a9795de4f730a)
- 检索方式：WebSearch 针对 sigir2026.org accepted-papers、"SIGIR 2026"/"SIGIR '26" + privacy/encrypted/homomorphic/PIR/RAG、paperdigest SIGIR 2026（封锁）；`gh gist view` 被 403。

**"PIR" 字样甄别（SIGIR 2024–2026）**
- 三年 SIGIR 搜索中未命中任何题目含 "private information retrieval" 的正会论文；IR 社区中与 "PIR" 相关的命中仅为上述 perspective 论文的 "Privacy Preserving Information Retrieval"（缩写也可能写作 PPIR，非密码学 PIR）。 — [DOI](https://doi.org/10.1145/3805712.3808547)

### Inferences
- 在可见证据范围内，WWW 与 SIGIR 2024–2026 正会**没有**以 PIR/SSE/ORAM 为机制、以"私密检索/私密 RAG/加密文档搜索"为主语的论文；唯一两篇密码组件论文分别是 MPC 聚合选择（WWW 2024）和 FHE 框架（WWW 2026），两者都不属于候选家族 A/B/C。
- SIGIR 社区处理"隐私"的主流路径是查询混淆、DP、联邦学习与攻击/泄漏分析；SIGIR 2026 以 perspective 论文形式提出隐私研究议程，说明社区对"隐私保护 IR"有议程层面的关注，但是否把密码学私有读取列为支柱尚待核读原文。
- WWW 的 Security and Privacy track（2026 年约 88 篇，二手数据）接纳纯密码学应用论文（FHE 框架、浏览器 E2EE、匿名证明），说明 WWW 有接纳"密码组件为机制"论文的 track 通道；但本会话未找到该 track 中与检索/读取相关的样本。
- 若将阈值放宽到"含任何密码组件的隐私推荐/联邦论文"，SIGIR/WWW 侧的联邦推荐论文（ReFer、FedCIA、Verifiable Federated Representation Learning）需逐篇核读其是否使用 secure aggregation/HE——本会话无法核读，不能计入。

### Gaps
- **覆盖面的根本缺口**：DBLP、ACM DL、六个会议官网的 accepted/proceedings 页面全部被代理封锁，只能靠搜索引擎摘要读取片段；因此每一年的列表都不完整，"未找到"不能理解为"不存在"。建议后续在无封锁环境下执行：`https://dblp.org/search/publ/api?q=<kw>%20venue%3ASIGIR%3A&format=json`（kw ∈ {homomorphic, private information retrieval, searchable encryption, oblivious, encrypted, secure, trusted execution}）以及对 www2024/2025/2026.thewebconf.org 与 sigir 各年 accepted 页面做 Ctrl-F。
- WWW 2025 几乎无有效覆盖（只看到 1 个隐私题目）；WWW 2026 industry track 与 SIGIR 各年 industry/resource track 未能读取。
- 两篇入选论文的原文字段未核：Damgård 等的具体延迟/通信数值与安全定理陈述；Chen & Zhao 的 track 归属、信任模型原句、是否有形式化安全证明。
- SIGIR 2026 perspective 论文是否提及 PIR/HE/SSE 作为研究支柱——这是对"IR 社区是否会接纳密码学私有读取"最直接的态度证据，但 PDF 与 DOI 页均被封锁。
- 以下 2025–2026 arXiv/ePrint 论文主题高度相关（私密稠密检索 / 私密 RAG / 加密向量库），但**均未能确认 venue**，不计入 WWW/SIGIR 证据，仅作线索供其他 venue 家族的研究者核查："PIR-RAG: A System for Private Information Retrieval in Retrieval-Augmented Generation" ([arXiv 2509.21325](https://arxiv.org/html/2509.21325v1))；"Pointing the Way, Hiding the Destination: Practical Private Dense Retrieval at Scale" ([arXiv 2608.25735](https://arxiv.org/pdf/2608.25735))；"Spruce: Scalable Private Outsourced Retrieval Using Compact Embeddings" ([arXiv 2609.03376](https://arxiv.org/pdf/2609.03376))；"Quantization Enables Private Dense Retrieval against Malicious Service Providers" ([arXiv 2609.36376](https://arxiv.org/abs/2609.36376v1))；"GoldenRetriever: Non-Interactive Homomorphic Encrypted Retrieval for Privacy-Preserving RAG" ([arXiv 2607.29019](https://arxiv.org/abs/2607.29019))；"P²RAG: Efficient Privacy-Preserving RAG Service Supporting Arbitrary Top-k Retrieval" ([arXiv 2603.14778](https://arxiv.org/pdf/2603.14778))；"PRAG: End-to-End Privacy-Preserving Retrieval-Augmented Generation" ([arXiv 2604.26525](https://arxiv.org/abs/2604.26525)，comments 字段称期刊投稿)；"RAGtime-PIANO: Efficient Secure Remote RAG" ([ePrint 2026/231](https://eprint.iacr.org/2026/231.pdf))；"π-RAG: Oblivious Retrieval via Semantic Quantization and Transcendental Addressing for LLMs" ([arXiv 2606.22153](https://arxiv.org/html/2606.22153v1))；"Private Embedding Lookup with Encrypted Compact Queries under Fully Homomorphic Encryption" ([arXiv 2606.03191](https://arxiv.org/pdf/2606.03191))；"Hermes: High-Performance Homomorphically Encrypted Vector Databases" ([arXiv 2506.03308](https://arxiv.org/abs/2506.03308v2)，ACM 模板 venue 为占位符)；"FRAG: Toward Federated Vector Database Management for Collaborative and Secure RAG" ([arXiv 2410.13272](https://arxiv.org/abs/2410.13272v1))；"Hiding Your Awful Online Choices Made More Efficient and Secure: A New Privacy-Aware Recommender System"（HE+MPC 推荐，[arXiv 2405.20483](https://arxiv.org/pdf/2405.20483)，venue 未知）；"A Unified Benchmark for Privacy-preserving Vector Search" ([arXiv 2608.01192](https://arxiv.org/pdf/2608.01192))。
- 工作坊/同址会议（不计入正会）：ICTIR 2024 有一篇联邦学习保密性论文（题目 [待核]，[ICTIR '24 proceedings](https://dl.acm.org/doi/proceedings/10.1145/3664190)）；SIGIR eCom 2026 workshop 论文在 CEUR-WS（[Vol-4250](https://ceur-ws.org/Vol-4250/)）。

## 关键问题 2：对每篇入选论文的字段表（题目 / venue 年份 track / 应用问题 / 角色与信任模型 / 承重实验 / 形式化证明 / 家族映射）

### Takeaway
两篇入选论文的字段汇总如下；两者家族映射均为"无"。

### Cited Findings
| 题目 | venue / 年份 / track | 应用问题 | 机制 | 角色与信任模型 | 承重实验 | 形式化证明 | 家族 | URL |
|---|---|---|---|---|---|---|---|---|
| Differentially Private Selection from Secure Distributed Computing | WWW 2024, research track, pp. 1103–1114 | 多用户二值向量之和的近似 argmax（热门项推荐、最流行网站统计） | MPC（integer secret sharing）+ DP | ≥3 服务器，honest majority（不合谋多数）；用户上传秘密分享，无客户端密钥 [待核] | 效用（对比中心模型 DP 机制）与效率（通信量，对比 CCS'19 方案），理论+实证；具体数值 [待核] | ePrint 称 information-theoretic guarantees；定理原文 [待核] | 无 | [DOI](https://doi.org/10.1145/3589334.3645435)、[ePrint](https://eprint.iacr.org/2023/894) |
| Reliable Non-Leveled Homomorphic Encryption for Web Services | WWW 2026, pp. 3171–3182, research track（rfp2521；Security & Privacy track [待核]） | 面向 web 服务的可靠 FHE 计算（加密推理 MNIST/CIFAR-10、联邦聚合、流式分析）——主语偏机制 | 非 leveled CKKS 变体（Refresh 代替 Rescale）+ 置换交织纠错层 | 单服务器 HE 外包；客户端持 HE 私钥 [待核] | 端到端延迟 vs leveled CKKS（N=8192）；突发信道下准确率差 ≤0.5pp | [待核] | 无 | [DOI](https://doi.org/10.1145/3774904.3792476)、[arXiv](https://arxiv.org/html/2508.02943v2) |

- 补充（未入选但 venue 已确认、含密码学但非检索）："Trust on Reload: Securing Browser-Based End-to-End Encryption"，WWW 2026 Security and Privacy track，web 应用 E2EE 代码可信问题。 — [CISPA](https://cispa.de/en/research/publications/213253-trust-on-reload-securing-browser-based-end-to-end-encryption)

### Inferences
- 两篇论文都不能作为"WWW 接纳单服务器私有读取变长记录"的直接证据；它们只能证明 WWW 接纳"密码组件为机制"的论文（MPC/FHE），且评审接受的承重实验是延迟/通信量 + 效用/准确率对比。

### Gaps
- 见关键问题 1 的 Gaps；两篇论文的原文字段（信任模型原句、定理、数值）均待核。

## 关键问题 3：各 venue-年份"未找到"的声明与所用检索方式

### Takeaway
WWW 2025、SIGIR 2024、SIGIR 2025、SIGIR 2026 四个 venue-年份在受限检索下**未找到**满足条件的论文；WWW 2024 与 WWW 2026 各找到 1 篇密码组件论文但家族映射为"无"。

### Cited Findings
- WWW 2025：未找到。检索方式：WebSearch（www2025.thewebconf.org accepted-papers / research-tracks / short-papers；ACM DL 10.1145/3696410；"ACM Web Conference 2025" + 密码学关键词）。 — [WWW 2025 research tracks](https://www2025.thewebconf.org/research-tracks)；[ACM DL](https://dl.acm.org/doi/proceedings/10.1145/3696410?tocHeading=heading43)
- SIGIR 2024：未找到。检索方式：WebSearch（sigir-2024.github.io/proceedings.html 的 "Privacy, Security and Federated Learning" session；ACM DL 10.1145/3626772；DBLP sigir2024）。 — [proceedings 页](https://sigir-2024.github.io/proceedings.html)；[DBLP](https://dblp.org/db/conf/sigir/sigir2024.html)
- SIGIR 2025：未找到。检索方式：WebSearch（sigir2025.dei.unipd.it accepted-papers / proceedings；ACM DL 10.1145/3726302）。 — [accepted papers](https://sigir2025.dei.unipd.it/accepted-papers.html)；[proceedings](https://sigir2025.dei.unipd.it/proceedings.html)
- SIGIR 2026：未找到（仅 perspective/resource/full 三篇非密码隐私论文）。检索方式：WebSearch（sigir2026.org accepted-papers；第三方 gist）。 — [accepted papers](https://sigir2026.org/en-AU/pages/program/accepted-papers)；[gist](https://gist.github.com/rankun203/4f1a77c2705b2945417d0e1c581c4859/ed5a208b9b628e02bef68a0a809a9795de4f730a)
- 所有一手站点的封锁记录可由代理状态核实（dblp.org、dl.acm.org、arxiv.org、eprint.iacr.org、openreview.net 等均 CONNECT 403）。 — 本会话 `curl "$HTTPS_PROXY/__agentproxy/status"` 输出（本地，无 URL）

### Inferences
- "未找到"的可信度：对 WWW 2024/2026 与 SIGIR 2024/2025/2026 为中等（多条不同措辞的搜索均一致指向无密码组件论文，且 SIGIR 2024 的隐私 session 可见论文均非密码学）；对 WWW 2025 为低（覆盖极少）。

### Gaps
- 需要在无封锁环境下用 DBLP API / ACM DL / 会议官网做完整 Ctrl-F 级别扫描后，才能把"未找到"升级为"不存在"。
