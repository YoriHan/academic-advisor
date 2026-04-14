# 我导skill.md

**一个虚拟答辩委员会。** 粘贴论文，20多位学术大佬轮流找你的漏洞，然后帮你改。

> 本技能不产生鼓励性反馈。被真实审稿人喷，痛，被导师喷，更痛，所以我来先喷你一遍。

[English](#english) | 中文

---

## 安装

### Claude Code（推荐）

```bash
# 克隆到 Claude Code 技能目录
git clone https://github.com/YoriHan/academic-advisor ~/.claude/skills/academic-advisor

# 或者只下载 SKILL.md
mkdir -p ~/.claude/skills/academic-advisor
curl -o ~/.claude/skills/academic-advisor/SKILL.md \
  https://raw.githubusercontent.com/YoriHan/academic-advisor/main/SKILL.md
```

安装完成后，在 Claude Code 里直接说触发词即可激活，无需其他配置：

```
帮我review这篇论文：[粘贴论文内容]
```

### Cursor / Windsurf / 其他支持自定义 System Prompt 的工具

将 `SKILL.md` 的全部内容粘贴到你的项目 `.cursorrules` 文件，或工具对应的 system prompt 配置位置：

```bash
# Cursor：放入项目根目录
cp SKILL.md .cursorrules

# 或手动粘贴到 Cursor Settings → Rules for AI
```

### API 直接调用（Claude / OpenAI 兼容接口）

将 `SKILL.md` 内容作为 `system` 消息，论文内容作为 `user` 消息：

```python
import anthropic

with open("SKILL.md") as f:
    skill_prompt = f.read()

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-opus-4-6",
    max_tokens=8192,
    system=skill_prompt,
    messages=[{
        "role": "user",
        "content": "帮我review这篇论文：\n\n[你的论文内容]"
    }]
)
```

### 纯对话工具（Claude.ai / ChatGPT / 豆包等）

1. 打开 `SKILL.md`，全选复制
2. 在对话框里先发送这段内容（作为 system prompt 的替代）
3. 然后发送你的论文 + 触发词

> 注意：纯对话工具没有持久化 system prompt，每次新对话都需要重新粘贴。

---

## 功能

### 1. 导师之间会吵架

普通AI审稿工具是多个视角并排列条目。这里不一样——Phase 2.5A 是强制执行的圆桌讨论，委员们看完彼此意见之后，会对同一篇论文产生分歧，然后当场辩论。

四种互动形态：

**意外结盟** — Chomsky 和 Labov 路线相反，但如果你的方法论太弱，他们可能罕见地一起批你"连数据收集的理论依据都没有"。

**正面冲突** — Deaton 说"你的RCT证明了因果"，Pearl 立刻反驳"你没有DAG，你怎么知道你控制的不是 collider"。你在旁边看着他们吵，同时意识到自己的论文两边都没想清楚。

**追击延伸** — Rubin 点出你的 SUTVA 假设有问题，Greenland 接着说"而且你也没算 E-value，所以就算 SUTVA 成立，结论对未测混淆也极度敏感"。一个问题被两个人从不同角度钉死。

**方向分歧** — Feynman 说这个结论不够普遍、不值得发 Nature，Hattie 说效应量 d=0.3 其实够大了。你发现自己面对的不是一个标准答案，而是一个真实的学术判断问题。

---

### 2. 主动找最致命的那个人

`单一导师：[论文内容]` — 技能沿四个维度分析：声明类型、最脆弱的点、子领域归属、你最怕被问的那种问题。然后推荐一个人，并解释为什么是他而不是别人。

同样是经济学论文声称用 PSM 控制了混淆因子，技能会选 Rubin 而不是 Deaton——因为核心漏洞是"潜在结果根本没定义清楚"，这是 Rubin 的主场。

---

### 3. P0/P1/P2 强制分级

每位委员的批评都标注优先级：

- **P0**（致命，≤5条）— 不解决就不能投
- **P1**（应该修）— 从"能过"到"过得好"
- **P2**（可以改）— 加分项

P0 硬性上限5条。有20个问题，你只会看到5条最致命的，而不是20条让你不知道从哪下手的建议。

---

### 4. 目标期刊决定审查标准

```
投 AER       → 因果推断团全员出动，识别策略逐条拆解
投 NeurIPS   → LeCun/Hinton 前移，benchmark 和 ablation 被重点审
投 Nature    → Feynman 声音加重，普遍性是门槛
投 JAMA      → CONSORT 规范逐条检查
没说投哪     → 根据论文质量推断，Phase 1 直接告诉你
```

---

### 5. 摘要和正文有没有对齐，静默检查

Phase 0.5 在你看不见的地方跑完：摘要每个声明在正文是否有支撑，正文核心发现摘要是否覆盖。评级 A/B/C/D，B 以下才显示具体问题，不打扰你。

---

### 6. 文献推荐不瞎编

AI 推荐文献的幻觉率高达 6–55%。这里只推三类：经典著作、高引论文、教科书级文献。标注 🟢（可直接引用）或 🟡（建议验证后引用）。

---

## 委员阵容

| 领域 | 委员 |
|------|------|
| 语言学 | Chomsky · Labov · Halliday · Lakoff · Goldberg · Traugott · Evans · Bresnan · Eckert · 沈家煊 · 陆俭明 · 袁毓林 · 刘丹青 |
| 社科/心理/实验 | Kahneman · Gelman · Cialdini · Simonsohn · Ioannidis · Nosek · 彭凯平 |
| 经济学 | Duflo · Deaton · Acemoglu · 林毅夫 |
| 因果推断 | Pearl（DAG法庭）· Rubin（潜在结果）· Greenland（E-value）· Athey（ML因果） |
| 生物/医学 | 颜宁 · 钟南山 · Gawande |
| 理工/通用 | Feynman · 杨振宁 · 杨强 |
| 教育/管理/人文 | Hattie · 顾明远 · Christensen · Mintzberg · 陈春花 · Harold Bloom · 钱钟书 · 叶嘉莹 |

---

## 使用方式

```
# 全文审查（默认）
帮我review这篇论文：[粘贴]

# 指定目标期刊
投NeurIPS，帮我review：[粘贴]

# 降AI润色（不审查，直接改写）
降AI：[粘贴段落]

# 单一导师 — 点名
Pearl视角：[粘贴]
Chomsky会怎么批：[粘贴]

# 单一导师 — 自动推荐最致命的那位
单一导师：[粘贴]

# 跨学科混合双打
混合双打：[粘贴]

# Phase 3 修改（审查后触发）
快速改：[指定段落]          → 段落级改写
深度改：[指定章节]          → 节级框架重组
答辩模式：[审稿意见]        → 生成 Rebuttal Letter
```

---

## 审查流程

```
Phase 0.5  摘要双向验证    静默执行，摘要与正文对不上就报警
Phase 1    快速扫描        核心主张 + 方法 + 红旗 + Mermaid 论证图
Phase 2    多维火力        每位委员攻击，必须引用论文具体位置，P0/P1/P2 分级
Phase 2.5A 圆桌讨论        委员之间辩论：结盟、冲突、追击、分歧
Phase 3    建设性修改       快速 / 深度 / 答辩 三档可选
Phase 3.5  可验证文献推荐   安全等级标注，不推幻觉文献
```

---

## 版本历史

| 版本 | 新增 |
|------|------|
| v2.5.0 | Pearl/Rubin/Greenland/Athey 因果推断团 · 目标期刊校准 · 摘要双向验证 · Mermaid 论证图 |
| v2.4.0 | 圆桌讨论 · 三档修改模式 · 可验证引用推荐 |
| v2.3.0 | P0/P1/P2 强制分级 |
| v2.2.0 | Mode C 自动推荐最致命导师 |
| v2.1.0 | +10 位同行评审专家 |
| v2.0.0 | 多委员虚拟答辩委员会 · 降AI润色 |

---

## 参考项目

- [PHY041/claude-skill-citation-checker](https://github.com/PHY041/claude-skill-citation-checker) — Phase 3.5 可验证引用推荐集成，用于检测幻觉文献

---

---

<a name="english"></a>

## English

**A virtual dissertation defense committee.** Paste your paper. 20+ academic figures tear it apart, then help you fix it.

### Install

**Claude Code**
```bash
git clone https://github.com/YoriHan/academic-advisor ~/.claude/skills/academic-advisor
```
Then trigger with any phrase like `review my paper:` or `help me find holes in my argument:`.

**Cursor / Windsurf**
```bash
cp SKILL.md .cursorrules
```
Or paste the content into Settings → Rules for AI.

**API (Claude / OpenAI-compatible)**
```python
with open("SKILL.md") as f:
    system = f.read()

# Use as system prompt, paper as user message
```

**Claude.ai / ChatGPT (no persistent system prompt)**
Copy the full `SKILL.md` content, paste it at the start of a new conversation, then send your paper.

### What makes it different

- **Critics argue with each other** — mandatory roundtable after individual critiques. Unexpected alliances, direct conflicts, pile-ons, genuine disagreements. Simulates what actually happens at a defense.
- **Auto-selects the most dangerous reviewer** — analyzes your paper's weakest point and picks the critic most likely to destroy it. Explains why.
- **P0/P1/P2 triage** — hard cap of 5 P0 (fatal) issues. You get the 5 things that will kill your paper, not 20 things that paralyze you.
- **Journal calibration** — say "submit to NeurIPS" and the ML critics get louder. Say "submit to AER" and the causal inference team moves to the front.
- **Silent abstract consistency check** — before Phase 1, quietly checks whether every abstract claim is supported in the body, and whether key findings made it into the abstract. Only surfaces if there's a real problem.
- **Citation safety** — only recommends classics, high-citation papers, and textbooks. Labels each 🟢 (cite directly) or 🟡 (verify first). Never fabricates.

### Trigger phrases (English)
```
review my paper: [paste]
submit to Nature, review: [paste]
find holes in my argument: [paste]
Chomsky perspective: [paste]
single advisor: [paste]    ← auto-selects most lethal reviewer
anti-AI rewrite: [paste]
```
