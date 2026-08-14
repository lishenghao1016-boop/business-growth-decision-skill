# 企业增长经营决策助手

一个面向企业老板、创始人和经营管理团队的中文 Agent Skill，可用于 Codex、Claude Code 及其他兼容 `SKILL.md` 的 Agent；支持标准 Agent Skills 的 WorkBuddy 版本也可使用。它把模糊的增长、利润、客户、品牌、预算和经营计划问题，转化为可比较、可批准、可验证、可复盘的管理决策。

> 当前版本：`v0.1.0-preview`。适合试用、评审和迭代，不代表已经覆盖所有行业与复杂经营场景。

## 它解决什么问题

管理者经常收到“加预算、做品牌、降价格、扩销售、进新市场”的方案，但真正需要回答的是：问题到底在哪里、证据够不够、还有哪些选择、什么条件下批准、失败时如何退出。

这个 Skill 会帮助你：

- 区分事实、计算结果、待验证假设和行动建议；
- 找出一个主瓶颈，而不是把问题默认归因于流量、品牌或销售人数；
- 比较至少两个方案及不行动的代价；
- 把部门主张翻译成经营假设、验证指标和停损条件；
- 形成负责人、领先指标、结果指标、复盘日期和 30/60/90 天行动。

## 支持的决策场景

- 增长与利润异常
- 预算与投放审批
- 定价与折扣
- 客户质量与留存
- 渠道经营
- 销售效率
- 新市场与新业务
- 产品组合与新品
- 品牌战略与品牌升级
- 年度经营计划

支持快速判断、深度诊断、经营会、方案审查和决策复盘五种输出模式，并包含消费零售、B2B SaaS、专业服务和平台业务适配。

## 安装

### 方法一：使用通用安装器

已安装 Node.js 的用户，可以使用开源的 `skills` CLI。它会发现仓库里的 Skill，并让用户选择目标 Agent、安装范围和复制方式：

```bash
npx skills add lishenghao1016-boop/business-growth-decision-skill --skill business-growth-decision
```

安装到个人环境，在所有项目中使用：

```bash
npx skills add lishenghao1016-boop/business-growth-decision-skill \
  --skill business-growth-decision --global
```

一次指定多个 Agent：

```bash
npx skills add lishenghao1016-boop/business-growth-decision-skill \
  --skill business-growth-decision --global \
  --agent codex --agent claude-code --agent cursor
```

`skills` CLI 当前支持 Codex、Claude Code、Cursor、Gemini CLI、GitHub Copilot、OpenCode 等多种 Agent。它是第三方开源安装器；安装前请照常检查 Skill 内容和权限。

### 方法二：手动安装

先克隆仓库：

```bash
git clone https://github.com/lishenghao1016-boop/business-growth-decision-skill.git
cd business-growth-decision-skill
```

然后把完整的 `skill/business-growth-decision` 目录复制到目标位置，不能只复制 `SKILL.md`，否则行业规则、指标口径和输出协议不会随之安装。

| Agent | 个人级目录 | 项目级目录 |
|---|---|---|
| Codex | `~/.agents/skills/` | `<项目>/.agents/skills/` |
| Claude Code | `~/.claude/skills/` | `<项目>/.claude/skills/` |
| WorkBuddy兼容版本* | `~/.workbuddy/skills/` | `<项目>/.workbuddy/skills/` |
| Cursor | `~/.cursor/skills/` | `<项目>/.agents/skills/` |
| Gemini CLI | `~/.gemini/skills/` | `<项目>/.agents/skills/` |
| GitHub Copilot | `~/.copilot/skills/` | `<项目>/.agents/skills/` |
| OpenCode | `~/.config/opencode/skills/` | `<项目>/.agents/skills/` |

\* WorkBuddy 尚未列入 `skills` CLI 的正式目标列表，而且不同版本的自定义 Skill 格式可能不同。优先使用 WorkBuddy 的 Skill Marketplace 或产品内导入功能；只有在当前版本明确支持标准 `SKILL.md` 时，才使用上述社区兼容目录。

例如，在确认当前 WorkBuddy 版本支持标准 `SKILL.md` 后，安装到个人目录：

```bash
mkdir -p ~/.workbuddy/skills
cp -R skill/business-growth-decision ~/.workbuddy/skills/
```

安装到 Claude Code 的个人目录：

```bash
mkdir -p ~/.claude/skills
cp -R skill/business-growth-decision ~/.claude/skills/
```

安装后重新启动 Agent；部分 Agent 也会自动检测变更。

## 调用方式

- **Codex**：输入 `$business-growth-decision`，或先运行 `/skills` 再选择。该 Skill 在 Codex 中关闭了隐式调用。
- **Claude Code及其他兼容Agent**：直接说明“使用 business-growth-decision 分析……”。宿主是否自动触发取决于它自己的 Skill 策略。
- **WorkBuddy**：通过市场或产品内导入后，在新任务中说明“使用 business-growth-decision 分析……”。若产品版本不接受标准 `SKILL.md`，需要等待市场上架或制作 WorkBuddy 原生包。
- `agents/openai.yaml` 只提供 OpenAI 产品的界面和调用策略；其他 Agent 主要读取标准的 `SKILL.md` 和 `references/`，不受影响。

## 快速开始

```text
使用 $business-growth-decision 审查我们的品牌升级方案。
背景：公司年收入2亿元，增长放缓；市场部申请800万元做品牌焕新，
希望一年内提高知名度并带动销售。请告诉我现在必须决定什么、
还缺哪些证据，以及未来90天如何验证。
```

更多入口见 [examples](examples/)，可回归测试的场景见 [evals/test-cases.md](evals/test-cases.md)。

## 输出原则

- 先定义经营结果和约束，再讨论动作；
- 不用曝光、线索、GMV、签约额或归因 ROI 单独证明经营成功；
- 不虚构行业阈值或无法从输入推导的数字；
- 关键数据不足时输出 `not_computable` 或带假设的条件式判断；
- 对高成本、长期或不可逆投入优先采用分阶段验证。

## 使用边界

本项目提供经营分析与决策辅助，不自动批准预算、付款、合同、招聘裁员、投放修改或对外发布。法律、税务、会计、劳动及其他专业事项应由合格专业人士复核。它也不用于广告文案、内容创作或跨境平台运营执行。

## 仓库结构

```text
.
├── skill/business-growth-decision/  # 可安装的 Skill 本体
├── examples/                        # 示例问题
├── evals/test-cases.md              # 行为回归用例
├── LICENSE
└── README.md
```

## 参与改进

欢迎通过 Issue 提交真实但已脱敏的经营场景、失败案例和边界问题。请不要提交客户身份信息、订单明细、账号凭据或未公开的公司资料。

## 许可证

[Apache License 2.0](LICENSE)
