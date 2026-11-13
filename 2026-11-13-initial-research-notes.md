# 2026-11-13 · 初始调研记录（规范 v1.0 的证据底稿）

> 文件名：`2026-11-13-initial-research-notes.md` ｜ 类别：日期时点快照
> 本文件是《AGENT 通用项目规范》的调研过程存档：4 路并行调研的原始报告汇总。
> 性质：时点快照（2026-11-13）。结论已沉淀进规范正文（现 `01-core-spec.md` 等），本文件仅作溯源用，默认不读。

---

## 报告一：survey-haydn —— haydn-server 知识沉淀体系

**概况**：由 Redmi K40 Pro 实战升级而成的「Android 手机→家用服务器」通用方法论知识库，明确「为长期由 Agent 迭代设计」。核心是四层知识（methodology 规范 / guardrails 红线 / pitfalls 证据 / toolkit 工具）+ cases 实证层 + 冻结旧证据（docs/、scripts/、android-bridge/）。【事实：D:\CODE\haydn-server\README.md L3-16】

### 1. AGENTS.md 骨架与关键规则（D:\CODE\haydn-server\AGENTS.md，287 行）
骨架 11 节：§0 十分钟启动清单（唯一权威读序+阅读预算）｜§1 角色目标｜§2 分层与权威优先级（含 Agent 操作边界三级表：可直接做/须授权/始终禁止）｜§3 红线 8 条自检｜§4 命名/ID/front matter/链接约定｜§5 任务协议（单 Agent 七步、多 Agent 写范围互斥、报告必含证据）｜§6 五类扩展 SOP 速查｜§7 验证与 DoD｜§8 机器可读接口（含 backlog 最小结构）｜§9 当前阶段与缺口｜§10 本文件维护。

最有价值规则（原文）：
1. 「单任务最多读 3 份规范/主题文档……禁止整读超过 300 行的文件：先用 grep/标题定位，再按 offset 分段」
2. 停止条件「与红线/规范冲突、需要越权/真机/凭据授权、证据不足无法验证、阅读预算用完、同一问题连续两次失败且原因不明——立即停下并说明」
3. 「禁止把『工作区里文件存在』当成『已交付』」（untracked 不在 git diff，须 add -A 后查 --cached）
4. 「先诚实，再完整：未实测不标完成；目标值/实测值分开；未知标『待验证』」
5. 权威七级「用户指令 > AGENTS.md > guardrails(MUST NOT) > methodology > pitfalls > toolkit > 冻结案例」，且「Guardrail 的 MUST NOT 永远压过 Method 的应当」
6. 「模板不写 TODO 占位；宁可标注『待验证』也不要留空」

### 2. 知识体系全景
- **pitfalls/**（PIT-000..021，22 条）：证据层，「每条 PIT 回答：怎么坏掉的/为什么坏/怎么避免和修/怎么验证修好了」；严重度 S1-S4；四途径检索（ID/严重度/域/机器索引）。**条目模板原文**（D:\CODE\haydn-server\pitfalls\README.md §3 条 5）：「文件名 ASCII；front matter 是**文件头部**（首行 `---` 到下一个 `---`）；必填 8 键 + PIT 额外必需 `severity`、`domain`；正文固定七个 H2：`症状 / 触发场景 / 根因 / 规避与修复 / 验证 / 关联 / 证据`；只用相对链接、禁止 `#anchor`、禁止绝对路径/IP/凭据/个人信息；建议 40–80 行。」实例 PIT-002：症状段给可 grep 关键字（RescueParty、`Can't find service: package`），证据段全部回链冻结案例；PIT-019 示范「安全承诺 vs 已落地实现」逐条核对。新增五步：找证据→取号（只增不复用）→按模板建文件→交叉链接→跑校验。
- **guardrails/**（GRD-000..014）：约束层。front matter 额外必需 `severity`(blocker/warning)+`scope`(device/server/agent/repo)；正文固定五节：红线(MUST NOT)→为什么→允许的替代→检查方式→关联。核心：「每条红线都必须能被检查：命令可复现，或给出可逐项勾选的人工核对清单」（guardrails\README.md §5）。GRD-012 为 Agent 行为总则（R1 先只读核对/R3 不伪造验收/R5 不改校验器通过验收/R6 失败即回滚/R7 不删偏差记录）。
- **methodology/**（MTH-000..011，12 篇）：规范层；门户给「人类 30 分钟/Agent 分层最小」双读径+文档地图表。关键篇 08-agent-handoff.md：**五级事实分级**（事实=已验证附证据／案例实证=链接案例注明设备版本时间／假设=写明验证方式与证伪时机／目标=与实测分栏／待验证=进 backlog 不得静默消失）+ 禁止用语「应该没问题/肯定可以/前文写过所以肯定对」+ 变更传播矩阵。
- **inventory/**（61 文件，gitignored 除 README）：运行期采集快照，「给文档提供可追溯实测证据；是证据，不是状态源」。命名 `<类型>-<时间戳>.<ext>` + `-latest` 指针；成对产出 json（可回滚）+ md/txt（可读）。
- **cases/**：实证层，每设备一目录，验收后冻结仅事实纠错。反馈闭环：「案例事实 → PIT（证据）→ MTH 修订（规范）→ GRD 补充（红线）→ toolkit 校验（实现）→ 下一个案例复验」（cases\README.md §2）。
- **changelog/**：按月归档 YYYY-MM.md；CHANGELOG.md 当前窗口 ≤8 条，第 9 条触发最旧归档；归档默认不读；「不把归档旧数字当当前事实」。
- **toolkit/**：零第三方依赖 Python 包（detect/probe/assess/plan/new-profile/index/validate）+ templates/ 九模板 + schema/ 五 schema + tests/ 离线 fixture 单测；「改契约必须同步 tests+schema+README」。

### 3. agent-manifest 与 backlog 机制
- **agent-manifest.json**（根摘要 ≤160 行）+ **agent-manifest.d/** 四分片：由工具从各层 front matter 生成、禁手改。根摘要含 entrypoints/reading_order/counts/shards/tools/invariants/commands。「根摘要→按需一个分片→单篇」的分层加载是防上下文膨胀核心。
- **backlog.json**：Agent 机器可读任务队列。每条：id(BL-NNN)/title/priority/area/status/acceptance/outputs/depends/notes/agent_hint/requires/owner/updated/result。

### 4. 亮点（8 条）
①§0 唯一权威读序+启动清单+停止条件；②阅读预算硬约束（≤3 篇/禁整读>300 行/默认不读归档/分层加载）；③五级事实分级+禁止用语表；④PIT 七段模板+「无证据只能标『通用风险，待案例验证』」；⑤GRD 五节结构+「每条红线必须能被检查」；⑥manifest 生成物机制（工具重建、validate 校验）；⑦反馈闭环+变更传播矩阵；⑧front matter `agent_hint` + ID 只增不复用。

### 5. 问题与教训（附证据）
1. backlog 语义一致性无机器校验（AGENTS.md §8 自认）。
2. 同一读序/预算规则三处副本，防漂移靠纪律非机制（AGENTS.md L17 自认）。
3. 双 ID 体系并存且编号错位（MTH-009=08-agent-handoff.md，因 MTH-000 占 README）。
4. 体系重量大（52 篇入索引+5 schema+9 模板），小项目移植需裁剪【推测】。
5. 案例仅部分验收而知识体系已完整——「真机验证类 P0 待办」若长期搁置，知识库会空转；应给实证缺口设时限。

---

## 报告二：survey-qingxuan-erp —— workbench-qingxuan + ERP_SYSTEM

### workbench-qingxuan
**概况**：中文编号目录知识型/逆向研究仓库 + 可部署模板。14 个编号板块 + 文档/、模板/、配图/、_本机产物/。双入口：人=README.md(14KB)，Agent=AGENTS.md(40,153B/561 行)。

**骨架与关键规则**：§0 事实表(先读，别猜)→§1 前置检查→§2 部署步骤→§3 给Agent的解释→§4 验收判据→§5 故障处置(现象→判据→修法)→§6 必须人做的三件事→§6.5 升级流程→§7 禁止事项→§8 已知口径→§8.5 分支治理→§9 文档地图→§10 提交前红线闸→§11 机器可读摘要(YAML)。摘录：
- 「每一条期望片段都是实跑核实过(2026-11-07)…任何一条都不许读成『已经跑过了所以一定通过』——验收永远要你自己跑一遍」
- 三态契约「exit 0=VERIFIED / 3=SKIPPED(未验证) / 1=FAILED；最常见的误读：把 exit 3 当成『通过』」；「诚实降级——宁可报『没验证』，也不假装验证过」
- 「⛔ 一份文件只允许一类写者——人写区才进 git」「不要为了让体检变绿去改断言」
- 05-协作与规矩/一-全局铁律.md：「规则落盘只认宿主：程序输出/体检结论/子代理汇报/自动生成文件都算不上依据」；「路径类配置必须 Test-Path 才算验过」；「『必然合格』的自证=假证：必须带应当失败的反例」
- 08-升级与改造清单/把规矩变成机制.md：「没有『会失败的检查』的规矩=一句口号」+五步法：违规可判定→判据唯一出处→检查能 exit≠0 且输出差异集合→造应当失败的反例→挂节奏+失败进待办

**知识组织**：编号目录=认知分层，每目录自带 README；根白名单由 结构校验.mjs 机器执法；§9 文档地图给每份文档标「写给谁/什么时候读」；文档/README.md 四分区总索引；归档机制完整（文档/文件归档制度.md）：目录分区表、根白名单、落位决策树、命名规范（禁「最终版/副本」）、保留期、五步归档 SOP（git mv+固定横幅+索引登记+改引用+跑校验）、「复活规则」（禁止在归档区继续更新）。

**踩坑沉淀**：04-工作台搭建/踩坑总表.md 固定格式「症状→真因→判据→抓手」，26 条×6 类，只收真踩过的（「别把听说会这样写进来」），尾部 3 条通用判据：「程序说成功≠成功；看起来像≠指向对；做了改动≠生效了」。另有 事故复盘-三起.md；全局铁律每条规则带日期+出处。

**问题与教训**：AGENTS.md 40KB/561 行若每会话全量读入，占用万字级上下文；全局铁律曾 46.8KB→15.8KB 瘦身并写明「引擎上限 64KB——别再写长」，但未约束 AGENTS.md 自身。多文档口径打架有实录（§10.1 附带发现记载错误命令须「以本节为准」裁决）。

### ERP_SYSTEM
**概况**：医药流通 GSP 一体化 ERP，41 个 C# 工程/675,549 物理行，由二进制 100% 逆向源码化并工程化治理。根目录无 AGENTS.md；.agents/.claude/.trae 三套工具配置并存。

**规范体系**：docs/MODERNIZATION_PLAN.md（23.4KB「主上下文文档」：五条硬约束「违反即返工」+事实档案带实测日期+技术债三档+进度看板）；docs/adr/ADR-001（36.3KB 标准 ADR：状态表带 R1-R18 修订流水、被否决备选、风险登记册）；.agents/rules/database-environments.md（双库拓扑、.env 凭证红线、只读白名单）；.trae/skills/dead-code-cleanup/SKILL.md（红线 7 条+判定口径表+六阶段流程）。

**知识组织**：docs/ 六分区（方案/adr/audit/deploy/sp_extract/窗口分析）；README 顶部即路由+单一事实源声明「工程数/行数等指标以该方案文件为准」。audit 审计册含「证据与复现产物」「审计局限」诚实章节。

**踩坑沉淀**：无独立踩坑台账，教训内嵌四处；最有价值的是「教训→审计册回写→技能红线」升级链：XdwlGifts 误判（367 处外部调用曾被误列退役，dependency-inventory.md:136 复核更正）→ 审计册教训「删除整工程前必须做 csproj 引用扫描…『零挂载』不等于『零调用』」→ SKILL.md 固化为红线 3。

**问题与教训**：52KB「全记录」=对话导出（按时间追加不按主题、无索引无归档、与 docs/ 数字重复、含本机路径与会话 id）；README:15 写「.NET Framework 3.5」与 ADR-001 R18 记「全工程 v4.8」疑似口径漂移（未核）；推测：全记录属过程档案，宜移入归档区。

---

## 报告三：survey-fc-erprebuild —— Financial Compensation + erp-rebuild

### Financial Compensation（FC）
**概况**：约100人集团内网报销系统；pnpm monorepo，NestJS12+Prisma+PostgreSQL+Vue3.5。

**AGENTS.md（258 行）骨架**：0优先级与当前状态→1项目一句话→2必读顺序（阅读预算）→3常用命令→4目录与边界（4.1 Agent操作边界）→5硬性规则→6阶段门（6.1踩坑沉淀）→7禁止清单→8提交评审→9停止条件→10问题定位表→11参考。关键规则：「单个任务最多读三份方案文档，AGENTS.md、代码、模板与任务卡不计入」；「发生冲突时立即停止并向用户确认，不得自行裁决」；Agent操作边界三级表；PR≤400行，必须附 `pnpm verify` 输出与未执行项。

**文档体系**：docs/ 两位数主题目录（00-governance…15-reference）+adr/（001–029）+changelog/（按月）+90-archive/（默认不读）。docs/README.md 唯一索引：按任务取文档表、按阶段取文档、文档地图（规模+何时读）、单一事实来源表、7条维护规则。决策双轨：decisions.md（28项 D-01 台账）与 ADR。CHANGELOG.md 只留8条、每条≤3行（变更/验证/未完成）。`pnpm docs:check` 机检链接、脚本存在性、数字与 OpenAPI 操作数。

**踩坑机制**：唯一位置 02-engineering/known-pitfalls.md。触发三条件（规范外根因/花额外代价定位/同类重复）；五字段：现象/根因/做法/自动兜底/关联；单条≤6行、全文≤40条、同问题只留一条；超限退役到 90-archive/<YYYY-MM>/known-pitfalls-retired.md。「自动兜底」三态（有/部分/无）。典型条目 KP-05（日期区间用联动控件）。

**亮点**：①阅读预算+按任务路由表；②决策台账+ADR 双层；③known-pitfalls 防膨胀与退役制度；④「自动兜底」三态量化人防/机防；⑤单一事实源表+「先改事实源再改引用方」；⑥docs:check 机检数字漂移；⑦五层 Agent 框架（14-agent/agent-framework.md：决策/入口/执行/工具/证据）。

**问题与教训**：①AGENTS.md 第17行塞入数百字当前状态与原则有张力；②多条 KP「自动兜底：无」仅靠人防；③工具事故真实发生（09-ops/trae-sandbox-recycle-bin.md 专记命令沙箱回收站异常——Agent工具副作用须入坑册）；④2026-11-12/ 未写入任何规范（自发惯例，与 erp-rebuild 制度化成对比）。

### erp-rebuild（ERP-R）
**概况**：C# WinForms+SQL Server 老 ERP 重写为 Spring Boot3.4+Vue3；初期100%穿透复用存储过程，金标准快照证明落库一致。

**AGENTS.md（327 行）骨架**：头部元数据（编号AGENT-001+机检命令）→0优先级与事实等级（0.1事实三级、0.2踩坑红线）→1一句话→2阅读分层L0–L4→3常用命令（期望输出/退出码）→4操作边界（4.1本地门禁钩子）→5变更与文档联动矩阵→6硬规则→7禁止清单→8验收门→9提交评审→10停止条件→11兼容入口→12YAML机器可读摘要。关键规则：事实三级（已核验/推断/未知，任何结论必须标注并给证据位置；「禁止用可能/大概/应该/看起来收尾」）；「字段与表结构必须双源核验；只有单源时只能标注推断」；「没有命令输出，不得写通过/已验证/已修复」；联动矩阵「联动失败即视为本单元未完成」；兼容入口「其他Agent工具只允许一行指针文件指向 AGENTS.md，禁止形成第二套协议」。

**文档体系**：00-governance/10-plan/20-verification/30-engineering/50-ops/90-archive；documentation-standard.md 定每目录「放/不放什么」；稳定编号 GOV/PLAN/VER/ENG/AGENT/OPS/REF-NNN（只增不复用、与文件名解耦）+元数据表+状态四态枚举；docs/README.md（GOV-000）总索引，编号路径以 scripts/facts.json 的 documents.registry 为准；`python scripts/docs_check.py` 十类机检（编号连续、状态枚举、根白名单、进度数字、SP登记、禁止清单、明文凭证扫描）随 pre-commit 强制执行。

**踩坑机制**：制度与正文分离——GOV-005（experiential-learning-standard.md）管流程，ENG-003（pitfalls-and-guardrails.md）放条目。触发 T-01~T-08（返工/回滚/补丁出错/文档被证伪/口径不符/单源被推翻/工具副作用/重复劳动）；判定原则「原本以为A、实际是B」；当次单元提交前写入；落位分流：技术→ENG-003、流程→GOV-005、业务口径→唯一事实源、可重复步骤→skills、高危重复→升级为 AGENTS.md 硬规则一行+指针。六字段（现象/根因/正确做法/如何验证/反例/来源=日期+提交哈希+事实等级）。机检：编号不跳号、六字段非空、来源含日期。退役：季度+里程碑复核；失效不删除、标注替代条目；条目总数异常减少视为删除嫌疑。典型条目 SP-01（存储过程空列名互相覆盖）。

**亮点**：①事实三级+UNKNOWN 强制输出；②docs_check 十类机检+版本化 pre-commit 钩子；③金标准快照+验收证据卡（目标/实测分开、三态判定、「不把用户当测试员」）；④经验沉淀全链路；⑤README「数字口径」表逐数写含义+采集命令，废弃口径（532）显式标注禁用；⑥失效条目不删、编号不复用；⑦YAML 机器可读摘要。

**问题与教训**：①无服务端 CI，pre-commit 只管文档一致性，编译/测试靠单独执行留证；②sql/README.md 曾含作者机绝对路径、README 曾指向已删除目录——跨机绝对路径须机检；③「532菜单」无证据口径曾长期存在——数字必须带采集命令；④VER-006 建索引前全仓0入链——未入索引的文档等于丢失；⑤GOV-004 将凭证脱敏/轮换显式延期并登记决策时点——敏感事项允许推迟但须留痕。

---

## 报告四：research-oss —— 开源社区调研（12 来源 + OpenAI 未核实项）

> web_search 后端认证故障不可用，全部改用 web_fetch 直接核实官方原文；除标注「未核实」外均为一手阅读。

1. **AGENTS.md 标准**（https://agents.md/ ）：「给 agent 的 README」，纯 Markdown、无 schema；仓库根目录，monorepo 可嵌套——离被编辑文件最近的文件优先，显式用户指令覆盖一切；60k+ 项目采用；OpenAI Codex/Amp/Jules/Cursor/Factory 联合发起，现归 Linux 基金会 AAIF 托管；Codex/Jules/Aider/goose/Zed/Warp/Cursor/RooCode/Gemini CLI 支持。
2. **Claude Code Best Practices**（https://code.claude.com/docs/en/best-practices ）：确定性二分——模型读的指令不保证执行，必须 100% 发生的规则交给 hooks/deny/CI；CLAUDE.md <200 行，只放代码里查不到、每次会话都要重挖的信息；局部规则放 .claude/rules/ 带 paths 过滤；团队流程写 skill；「完成」要有可运行 pass/fail 检查并索要证据；堆 IMPORTANT/大写强调已被官方列为无效。
3. **Building Effective Agents**（https://www.anthropic.com/research/building-effective-agents ）：workflows vs agents；五模式（提示链/路由/并行/orchestrator-workers/evaluator-optimizer）；agent 循环须有停止条件（最大迭代数）；三原则：简单、透明、像雕琢 HCI 一样雕琢 ACI。
4. **Effective Context Engineering**（https://www.anthropic.com/research/effective-context-engineering-for-ai-agents ）：context rot——token 越多召回越差；最小高信号 token 集；系统提示要「对的海拔」；JIT 检索只维护轻量标识符；长任务三技术：compaction（保留架构决策/未决 bug）、结构化笔记（NOTES.md/todo）、子 agent（只回传 1-2k token 摘要）。
5. **Cline Memory Bank**（https://docs.cline.bot/best-practices/memory-bank ）：6 核心文件按更新频率分层：projectbrief→productContext/systemPatterns/techContext→activeContext+progress；声明「记忆每次会话完全重置，Memory Bank 是唯一纽带」；上下文快满时先 update 再开新会话。
6. **Cursor Rules**（https://cursor.com/docs/rules ）：.cursor/rules 下 .mdc 带 frontmatter，四种触发（alwaysApply/globs/description 自选/@手动）；单条 <500 行、大规则拆分；引用文件而非复制内容；反模式：整拷风格指南（该用 linter）；「同一错误犯两次→写规则」。
7. **GitHub Spec Kit**（https://github.com/github/spec-kit ）：constitution→specify（只写 WHAT/WHY 禁 HOW）→plan→tasks（标 [P] 可并行）→implement→converge；歧义处强制标 [NEEDS CLARIFICATION] 不许猜；检查清单当 spec 的单元测试。
8. **AWS Kiro**（https://kiro.dev/docs/specs/ ｜ https://kiro.dev/docs/steering/ ）：spec 三文件 requirements/design/tasks；steering 三件套 product/tech/structure 默认全量加载，自定义文件四种 inclusion（always/fileMatch/manual/auto）。
9. **BMAD-METHOD**（https://github.com/bmad-code-org/BMAD-METHOD ）：多角色 agent + skills；核心循环 Clarify→Plan→Build/verify→Learn；「把未言明的假设变成显式决策并作为持久上下文携带」。
10. **Aider conventions**（https://aider.chat/docs/usage/conventions.html ）：CONVENTIONS.md 小文件只读载入；两行约定即可显著改变生成行为。
11. **Diátaxis**（https://diataxis.fr/ ）：四象限 tutorial/how-to/reference/explanation；两轴 action/cognition × study/work；相邻象限互相吸引导致「坍塌」是文档腐化主因；compass 两问定位归属。
12. **ADR**（https://adr.github.io/ ）：一条 ADR 一个决策+理由+权衡；Nygard 五段式；MADR 增备选方案对比；Y-statement 一句话版；决策变更以 superseded 显式取代，不原地改写。
13. **12-Factor Agents**（https://github.com/humanlayer/12-factor-agents ）：#2 own your prompts（提示词是一等公民代码）；#3 own your context window（已解决的错误可从上下文移除，敏感信息可过滤）；#7 结构化工具联系人类；#8 高危调用在「工具选择与执行之间」断开等待审批；#9 连续错误约 3 次后停止并升级人类；#10 small focused agents——每个 agent 限 3-10 步（最多约 20 步）。
14. **OpenAI**：AGENTS.md 官网确认 Codex 为联合发起者；openai/codex 仓库自身含 88 个 AGENTS.md（官方嵌套示范）。**未核实**：《A Practical Guide to Building Agents》仅 PDF、官方域被 403，相关细节未引用。

**Top 10 最值得吸收的实践**：
1. 根指令文件 <200 行，只写代码里查不到的信息（CC）。
2. 渐进披露 + JIT 检索：先定位再精读，禁止一次加载全量（CE）。
3. 确定性二分：可例外的写文档，零例外的交给工具强制（CC）。
4. 完成 = 可运行的 pass/fail 检查 + 证据输出（CC）。
5. Memory Bank 六文件分层，activeContext 每会话更新（CL）。
6. 模糊点强制 [NEEDS CLARIFICATION]，禁止静默假设（SK）。
7. 小而专注：单 agent 3-10 步预算（12F #10）。
8. 连续错误 3 次即停止升级人类（12F #9）。
9. 文档四象限 + compass，混类即坏味道（DX）。
10. 决策入 ADR：五段式 + 序号 + superseded 链（ADR）。
