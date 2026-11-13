# 90 · 参考资料与模板

> 文件名：`90-reference-and-templates.md` ｜ 上级索引：[README.md](README.md) ｜ 返回核心规范：[01-core-spec.md](01-core-spec.md)
> 用途：复制模板、追溯条款出处、查阅开源来源。按需查阅，不属每次会话必读。

---

## 附录 A：模板集

### A1 AGENTS.md 骨架（新项目起点，约 120 行预算）

~~~markdown
# AGENTS.md
> 维护：谁可改；最近更新 YYYY-MM-DD；机检命令：`pnpm docs:check`

## 0. 优先级与当前状态
权威顺序：用户指令 > 本文件 > docs/00-governance 红线 > docs 其他 > 机器索引 > 归档。
事实三级：已核验/推断/未知，一切结论必须标注（详见 §3.7 汇报格式）。
当前阶段：<一句话>。

## 1. 项目一句话
<做什么、给谁、技术栈、仓库类型（构建型/知识型）>

## 2. 必读顺序与阅读预算
1. 本文件 → 2. docs/README.md「按任务取文档」表 → 3. 任务相关文档。
预算：方案文档 ≤3 篇/任务；>300 行文件禁整读（grep 标题→分段）；归档默认不读。

## 3. 常用命令
| 命令 | 作用 | 期望输出/退出码 |
|---|---|---|
| <build> | … | 0＝成功 |

## 4. 目录与操作边界
<目录职责表>
| 可直接执行 | 须授权 | 始终禁止 |
|---|---|---|

## 5. 硬性规则
<一行一条，MUST，可检查>

## 6. 开发流程与验收门
<阶段门 + DoD 引用 docs/00-governance/…>

## 7. 禁止清单
<一行一条，MUST NOT>

## 8. 提交与评审
PR ≤400 行；附验证输出与未执行项。

## 9. 停止条件
冲突/越权/证据不足/预算尽/连败2次 → 停下按 §3.8 提问格式报告。

## 10. 机器可读摘要
```yaml
source_of_truth: { 指标: 文档路径, … }
exit_codes: { 0: VERIFIED, 3: SKIPPED, 1: FAILED }
forbidden: [ … ]
```

## 11. 本文件的维护
<谁改/何时改/改完跑什么校验>
~~~

### A2 docs/README.md 索引骨架

```markdown
# 文档索引（唯一权威索引）
## 按任务取文档
| 我要… | 顺序读 |
|---|---|
| 改数据库 | sql/README → docs/30-engineering/db-rules |
| 新功能 | docs/10-plan/prd → docs/adr 相关篇 |
## 文档地图
| 文档 | 规模 | 写给谁 | 什么时候读 |
## 单一事实源
| 事实类别 | 唯一出处 |
|---|---|
| 行数/工程数 | docs/10-plan/modernization-plan |
## 维护规则
1. 新建文档必须登记本表与 front matter；2. 移动文档全仓改引用；3. 改完跑 docs:check。
```

### A3 踩坑条目（见 §7.3，直接复制）
### A4 ADR（Nygard 五段＋状态流水，见 §2.5；重大决策加 MADR「备选方案对比」节）
### A5 交接笔记模板

```markdown
# 交接 YYYY-MM-DD <任务名>
目标 / 已完成(证据) / 进行中(卡点) / 未做(优先级) / 关键文件:行号 / 相关PIT / 下一步建议
```

### A6 汇报模板（§3.7 五段）｜ A7 DoD 清单（§3.5 五项）

---

## 附录 B：本地五项目经验对照表（条款出处速查）

| 本规范条款 | 直接来源（项目/文件） |
|---|---|
| §0.3 权威七级 | haydn AGENTS.md §2 |
| §1.2 章节骨架 | FC/ERP-R/haydn/QX 四份 AGENTS.md 并集；≤200 行取自 CC |
| §1.4 日期任务区 | ERP-R AGENTS.md §4（制度化）；FC 2026-11-12/（自发反例） |
| §2.3 索引四表 | FC docs/README（按任务取文档＋单一事实源表）；ERP-R GOV-000；QX §9 文档地图 |
| §2.5 ADR 修订流水 | ERP-S docs/adr/ADR-001（R1-R18） |
| §2.6 联动矩阵 | ERP-R AGENTS.md §5 |
| §3.2 先搜后读/300 行 | haydn AGENTS.md §0 |
| §3.4 三级边界表 | FC §4.1＋haydn §2.1；GRD-012 |
| §3.5 三态退出码/证据卡 | QX 退出码契约；ERP-R acceptance-card |
| §3.7 事实分级/禁用语 | ERP-R §0.1；haydn methodology/08 §6 |
| §3.10 写范围互斥/backlog | haydn §5.2＋backlog.json |
| §4.2 阅读预算 | FC §2＋haydn §0 |
| §4.4 压缩/状态外置 | CE；CL |
| §5.2 双源核验 | ERP-R §0.1 |
| §5.4 口径勘误 | ERP-S 行数口径双标注；ERP-R 数字口径表 |
| §6.2 反例自证 | QX 把规矩变成机制.md 五步法 |
| §6.4 停止条件 | haydn §0＋12F #9/#10 |
| §7.2 触发 T-1~T-8 | ERP-R GOV-005 |
| §7.3 六字段 | ERP-R ENG-003＋FC known-pitfalls＋haydn pitfalls/README |
| §7.5 升级链 | ERP-S（教训→审计册→SKILL）＋haydn（case→PIT→GRD→toolkit）＋QX 五步法 |
| §7.6 防膨胀退役 | FC（≤40 条）＋ERP-R（季度复核/失效不删） |
| §8.2 五步归档 SOP | QX 文档/文件归档制度.md |
| §8.3 窗口化 | FC＋haydn changelog |
| §9.5 机检清单 | FC docs:check＋ERP-R docs_check＋QX 结构校验＋haydn validate |

## 附录 C：开源参考

- AGENTS.md 标准：https://agents.md/ （OpenAI/Anthropic 系工具广泛支持；AAIF 托管）
- Claude Code Best Practices：https://code.claude.com/docs/en/best-practices
- Building Effective Agents：https://www.anthropic.com/research/building-effective-agents
- Effective Context Engineering for AI Agents：https://www.anthropic.com/research/effective-context-engineering-for-ai-agents
- Cline Memory Bank：https://docs.cline.bot/best-practices/memory-bank
- Cursor Rules：https://cursor.com/docs/rules
- GitHub Spec Kit：https://github.com/github/spec-kit
- AWS Kiro specs/steering：https://kiro.dev/docs/specs/ ｜ https://kiro.dev/docs/steering/
- BMAD-METHOD：https://github.com/bmad-code-org/BMAD-METHOD
- Aider conventions：https://aider.chat/docs/usage/conventions.html
- Diátaxis：https://diataxis.fr/
- ADR：https://adr.github.io/
- 12-Factor Agents：https://github.com/humanlayer/12-factor-agents
- Codex AGENTS.md 指南（未核实的部分未引用）：https://github.com/openai/codex/blob/main/docs/agents_md.md

### 附册 D：项目推进与交付治理的开源参考

> 本组链接用于 [03-project-delivery.md](03-project-delivery.md)，只借鉴机制，不照抄组织规模或版本窗口。

- Kubernetes KEP template（需求提案/目标/非目标/测试/生产就绪/毕业门）：https://raw.githubusercontent.com/kubernetes/enhancements/master/keps/NNNN-kep-template/README.md
- Kubernetes Version Skew Policy（兼容矩阵与升级先后）：https://kubernetes.io/releases/version-skew-policy/
- Django release/deprecation policy（版本类型、支持、弃用窗口）：https://docs.djangoproject.com/en/6.1/internals/release-process/
- OpenTelemetry client versioning and stability（稳定性状态、兼容、弃用、遥测契约）：https://opentelemetry.io/docs/specs/otel/versioning-and-stability/
- GOV.UK user research in discovery（需求探索与用户研究）：https://www.gov.uk/service-manual/user-research/user-research-in-discovery
- NIST Privacy Framework（自愿隐私风险管理；当前页面同时列出 PF 1.1 草案）：https://www.nist.gov/privacy-framework
- SLSA requirements v1.2（现行版本，来源证明等供应链要求）：https://slsa.dev/spec/v1.2/requirements
- OpenSSF Scorecard checks（分支保护/依赖/许可证/SBOM/签名等检查）：https://github.com/ossf/scorecard/blob/main/docs/checks.md
- GitHub CODEOWNERS（代码路径责任与评审路由）：https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners
- Google SRE Workbook incident response 页面尝试抓取失败，本版未引用其内容：https://sre.google/workbook/incident-response/

---

*本规范由 4 路调研（本地五项目一手阅读＋12 个开源来源一手核实，调研记录存 `2026-11-13-initial-research-notes.md`）整合而成；条款均有出处（附录 B/C），未核实信息未引入正文。*
*定位说明：本文件是**元规范**，供人类维护者与 Agent 按需查阅与裁剪，不作为每次会话必读入口，因此不适用 §1.2 的 200 行容量红线；项目落地时按 §10 从本文件裁剪生成各自的 AGENTS.md（≤200 行）。阅读本文件请走「grep 标题定位 → 分段精读」（§3.2）。后续修订走版本号＋变更说明，修订记录参考 §2.5 ADR 状态流水做法。*

