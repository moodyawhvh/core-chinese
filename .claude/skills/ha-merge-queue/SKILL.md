> 🌐 本文档由 [home-assistant/core](https://github.com/home-assistant/core) 翻译,英文原版见原项目。

---
name: ha-merge-queue
description: Finds open Home Assistant pull requests that are genuinely ready to merge, checking CI, the merge-gate statuses, code-owner approval, merge conflicts, requested changes and unresolved review threads. Use when looking for PRs to merge, doing merge-queue triage, or asking for "quick wins" from the open PR backlog.
---

# 找出可合并的 Pull Request

产出一个维护者可以立即批准并合并的开放 PR 短名单,外加一份简短的"差一点"名单——每个都只需一次具体推动即可就绪。默认给 10 个候选,除非用户要求其他数量。

## 收集候选

搜索开放 PR,排除结构性不可合并的:

```
repo:home-assistant/core is:open is:pr draft:false status:success -review:changes_requested
  -label:"awaiting-frontend" -label:"stale" -label:"cla-needed"
```

要找"速赢"可以加 `label:"small-pr"`;要找已有批准的 PR,可加 `label:"code-owner-approved"` / `review:approved`。多跑几组搜索并合并结果——单一查询覆盖不了全部。

搜索索引比现实滞后数小时。把 `status:` 当作粗过滤器,绝不当证据;每个入围 PR 都要用下面的逐项检查来核实。

## 逐项核实入围 PR

检查全部六项条件。第 1 到第 5 项任何一项失败,该 PR 就不能进入短名单。

对每个列表响应都要翻页后再下结论。check runs、reviews 和 review threads 都是分页的,通常每页 30 条,而一个全量 Home Assistant PR 会有 40 多个 check run——所以第一页可能只显示绿色子集,失败项或现存 review 却在第二页。把返回数量与报告的总数比较,持续取页直到两者一致。

1. **合并门禁状态(merge-gate statuses)** —— 获取组合提交状态。这是最便宜、信息量最大的一次调用,也正是真正卡住合并按钮的东西。有五个 context 关键:`code-owner-approval`(Platinum 级集成必需)、`required-labels`(作者没勾任何 "Type of change" 时变红)、`docs-missing`(面向用户的改动没有文档 PR 时变红)、`cla-bot`,以及 `blocking-label-awaiting-frontend`。

2. **Check runs** —— 提交状态不覆盖 GitHub Actions。获取 check runs,并要求每一个都达到可接受的终态:`status` 必须是 `completed`,且 `conclusion` 必须是 `success`、`skipped` 或 `neutral`。其余一律不合格——`failure`、`cancelled`、`timed_out`、`action_required`、`stale`,以及仍在 `queued` 或 `in_progress` 的任何运行。组合状态绿色、底下却压着一个红色或仍在跑的测试任务是常态,所以只看状态列表永远不够。

3. **合并冲突** —— 读取 `mergeable_state`。GitHub 异步计算它:一次请求若没有缓存答案会返回 `unknown` 并触发计算,紧接着的下一次请求仍可能返回 `unknown`。带有限次数的重试去轮询——试几次、中间停顿——把一直无法解析的 `unknown` 当作未就绪,而不是默认没问题。
   - `clean` —— 所有合并条件都满足;立即合并
   - `blocked` —— 某个分支保护条件未满足。它并不特指"在等批准",所以默认不要那样报告:用其余四项检查确认到底缺哪个条件,只有全部都干净时才能说它在等审阅。
   - `behind` —— 基分支前进了。可操作且通常一键更新,要呈现出来而不是丢弃该 PR。
   - `unstable` —— 一个非必需检查失败。进入短名单前先查明是哪一个。
   - `dirty` —— 存在合并冲突。作者需要把 `dev` 合入分支。不要让他们 rebase:`AGENTS.md` 禁止在 PR 开启后重写 PR 分支历史,因为审阅者需要看到自上次审阅以来的变化。`homeassistant/generated/integrations.json` 经常冲突,所以新增集成的 PR 很快就会变 stale。
   - 其他任何值(`draft`、`has_hooks`、未列出的值)—— 不要猜测含义。把该 PR 视为未就绪,并报告拿到的值。

4. **Review 提交记录** —— 获取 reviews 本身,而不只是 threads。`CHANGES_REQUESTED` 的 review 只会出现在这里,别处都没有;而且审阅者可以只写顶层 body、不加任何行内评论就请求变更——这样的 PR 没有任何开放的 review thread,在上面的检查里看起来很干净。每个审阅者取最新一次提交记录:同一人后来没有新的 `APPROVED` 取代的、仍然存续的 `CHANGES_REQUESTED` 会使 PR 不合格,无论其行内 thread 之后是否被解决——解决 thread 并不撤回 review。也不要依赖 `-review:changes_requested` 这个搜索限定词——索引是过期的,而且 `mergeable_state: blocked` 区分不了"缺一个批准"和"有人请求了变更"。

5. **Review threads** —— 获取 review threads 并读取 `is_resolved`。评判一个 thread 要看它是否已解决或实质已处理,而不是看谁写的:`copilot-pull-request-reviewer` 提出的未解决发现就是一份 bug 报告,可能是真实缺陷,所以要先就事论事地读它,再决定是否忽略。每个仍开放的 thread 都计入 PR 的负担,直到你读完并确认它不需要改动——问题已被回答、建议已被考虑并拒绝、或该问题已在 diff 的其他地方修复。对每个开放 thread 都要说明它是什么、为什么阻塞或不阻塞。绝不要按它表面上所属的类别来打折;一个未被处理的缺陷就是阻塞项,无论有没有人在争论它。

6. **同行评审清单核验(优先级加成)** —— 检查 PR 正文中的模板复选框:`- [x] I have reviewed two other [open pull requests][prs] in this repository.`
   - **已勾(`[x]`):** 该 PR 在排序中给更高优先级,以奖励帮助清理审阅积压的贡献者。
   - **未勾(`[ ]` 或缺失):** 按标准优先级留在队列中(不取消资格)。

## 报告

候选排序首先按合并就绪状态(`clean` 在前,然后是其余全绿的 `blocked`),**其次按同行评审参与度**:

1. 勾选了 `[x] I have reviewed two other open pull requests...` 复选框的 **clean PR**。
2. 未勾选该复选框的 **clean PR**。
3. 勾选了该复选框的 **blocked/差一点 PR**。
4. 未勾选该复选框的 **blocked/差一点 PR**。

对每个 PR:给出完整 markdown 链接的编号、涉及的集成或核心领域、一行内容说明、阻塞状态,并明确标注作者是否勾了同行评审框(例如 "⭐ *贡献者已评审 2 个 PR*")。

很多 `home-assistant/core` PR 涉及 helper、框架、recorder 或仓库工具链,根本不涉及任何集成——此时直接说明它改了什么,不要丢弃它,也不要编造一个集成名。

然后单独列出"差一点"名单,每项给出解除阻塞的那一个动作。常见情况:

- 与 diff 无关的测试失败——点名该测试,建议重跑 job。
- `required-labels` 变红——指出应添加的标签(`bugfix`、`new-feature` 等)。
- 等待 code-owner 批准——从 `manifest.json` 点名 code owner。
- 存续的 `CHANGES_REQUESTED` review——点名审阅者,说明其要求。
- `dirty` ——作者必须合入 `dev` 并重新生成生成文件。

提醒维护者值得注意的不一致之处:PR 正文声明了破坏性变更却没有 `breaking-change` 标签,会悄悄错过发布说明。

## 重要

- 只在控制台报告。不要在 GitHub 上执行任何操作——不发评论、不做 review、不合并、不向贡献者分支推送。依据 `AI_POLICY.md`,决定和执行都由人来完成。
- 绝不要仅凭 CI 全绿就断言 PR 就绪。对入围的每个 PR 都要读 diff;CI 无法告诉你改动是否正确、是否被需要。
