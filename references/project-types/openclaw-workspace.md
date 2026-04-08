# OpenClaw Workspace Projects

适用于以 OpenClaw workspace 文件驱动行为的个人 agent 项目。

这类项目的改动主面通常不是传统前后端模块，而是 workspace 内不同类型的用户操作面。看 `scope` 时，与其贴着某一个文件名走，不如先想清楚用户这次感知到的变化发生在哪一层能力面。

## 推荐写法
- `summary` 语言：默认中文
- 提交格式：继续使用 `type(scope): summary`
- `scope` 选择顺序：先看能力面，再看长期任务域，最后再看文件落点

## 推荐 scopes

用户从使用视角感知 OpenClaw workspace 时，通常会落到下面几类：

- `heartbeat`：心跳、轮询节奏、主动执行频率、唤醒与暂停策略
- `agent`：面向 agent 的运行规则、身份边界、工具策略、用户偏好等整体行为面；可覆盖 `AGENTS.md`、`SOUL.md`、`TOOLS.md`、`USER.md` 这一组文件
- `skill`：skills 的定义、选择、编排、引用方式、技能文档和技能约束
- `memory`：`MEMORY.md`、记忆摘要、长期记忆整理、阶段结论沉淀

## 选择感觉
- 先看这次变化更像是在调节节律、收敛 agent 行为、组织 skill，还是在沉淀 memory。
- 改动跨了 `AGENTS.md`、`SOUL.md`、`TOOLS.md`、`USER.md` 也没关系，只要用户看到的是 agent 整体行为在变，`scope` 仍然可以自然落在 `agent`。
- 节律、主动唤醒、周期行为、停机恢复这类变化，放在 `heartbeat` 会更清楚。
- 技能定义、技能目录、技能路由、技能调用边界这类变化，放在 `skill` 会更顺手。
- 阶段认识、记忆压缩、长期结论回写这类变化，放在 `memory` 会更贴近它的长期价值。
- 个人 agent 往往还会长期承接几条稳定主线，例如邮件、翻译、开发、研究或日程处理。这些任务域本身也是用户直接感知的工作面。
- 当一次提交明显服务于某条长期任务线，而且这个任务域会持续复用、名字也足够直观时，直接使用该任务域做 `scope` 会更像真实工作流。等改动重心回到 agent 行为、技能能力或记忆沉淀，再回到 `agent`、`skill`、`memory`。

## 不推荐 scopes
- `core`
- `system`
- `prompt`
- `task`
- `notes`
- `misc`

这些 scope 过泛，无法从提交历史中快速看出到底是心跳节律、agent 行为面、skill 编排，还是记忆沉淀发生了变化。

## 使用提示
- `agent` 比较适合承接整体行为约束，所以哪怕改动分散在多个 workspace 文件中，只要感知上是在改 agent 自身，落到 `agent` 就很自然。
- `heartbeat` 单独拿出来会更容易读出主动执行策略怎么演化。
- `memory` 的 `summary` 更适合直接写沉淀了什么认识、保留了什么经验、修正了什么判断。
- `skill` 的 `summary` 更适合直接写新增了哪类能力、整理了哪条入口、收拢了哪种调用方式。
- 说明类文档即使放在 `docs/` 下，也还是可以跟着它真正服务的对象走；目录只是落点，用户感知才更接近 `scope`。

## 示例
- `docs(agent): 增加诚实回答与不装作已完成的 Soul 要求`
- `docs(agent): 补充遇到高风险操作先暂停确认的执行边界`
- `feat(mail): 新增未回复邮件优先级判断与延迟回复规则`
- `fix(mail): 修正邮件归档后仍重复跟进的问题`
- `docs(translation): 明确专有名词保留原文与双语输出顺序`
- `refactor(dev): 收敛生成补丁前先读上下文再动代码的流程`
- `feat(research): 新增先列信息缺口再展开检索的研究步骤`
- `feat(heartbeat): 增加晨间唤醒、午间静默与失败后退避策略`
- `feat(skill): 新增开发任务拆解 skill 与交付前检查清单`
- `chore(memory): 沉淀本阶段常见失误与对应恢复策略`

## 反例
- `docs(agent): 更新一些规则`
- `chore(system): 调整记忆和任务`
- `refactor(prompt): 优化提示词`
- `feat(task): 更新当前主线`

更好的写法应直接落到具体职责：
- `docs(agent): 明确当前阶段的协作边界与交接条件`
- `chore(memory): 沉淀上一阶段的关键决策与遗留问题`
- `feat(heartbeat): 增加主动轮询与静默期控制`
- `refactor(skill): 收敛研究与翻译任务的技能入口`
