> 🌐 本文档由 [home-assistant/core](https://github.com/home-assistant/core) 翻译,英文原版见原项目。

# GitHub Copilot 与 Claude Code 使用说明

本仓库包含 Home Assistant 的核心代码,一个基于 Python 3 的家庭自动化应用。

## Git 提交规范

- **PR 开启后,不要对已推送到 PR 分支的提交执行 amend、squash 或 rebase** —— 审阅者需要跟踪提交历史,并查看自上次审阅以来发生了哪些变化

## Pull Request

- 开启 pull request 时,使用仓库的 PR 模板(`.github/PULL_REQUEST_TEMPLATE.md`)。绝对不要删除模板中的任何内容。
- 不要删除未勾选的复选框——保留所有未勾选的复选框,让审阅者能看到哪些选项未被选择。

## 开发命令

- 在当前虚拟环境中运行 "python3",确保测试使用正确的 Python 版本。
- 进入新环境或 worktree 时,运行 `script/setup` 来搭建包含全部开发依赖(pylint、pre-commit 钩子等)的虚拟环境。提交代码前必须完成这一步。如果 uv 提示找不到所需 Python 版本的下载,说明环境中的 uv 版本过旧;用 `curl -LsSf https://astral.sh/uv/install.sh | sh` 升级后重新运行 `script/setup`。
- `.vscode/tasks.json` 包含开发常用的实用命令。
- 每次写完代码后,运行 `uv run --no-sync prek run --all-files` 检查 lint 和格式问题。

## Python 语法注意事项

- Home Assistant 官方支持的最低版本是 Python 3.14。不要把依赖 Python 3.14 的语法或特性标记为问题,也不要建议针对旧版 Python 的变通方案。
- Python 3.14 明确允许不带括号的 `except TypeA, TypeB:` 写法。绝不要将其标记为问题。
- Python 3.14 对注解采用惰性求值(PEP 649)。注解中的前向引用不需要加引号——注解可以直接引用模块中后面才定义的名字,无需加引号或使用 `from __future__ import annotations`。不要把注解中未加引号的前向引用标记为问题。

## 测试

- 使用 `uv run --no-sync pytest` 运行测试
- 修改某个集成的 `strings.json` 后,运行测试前先重新生成英文翻译文件:`python3 -m script.translations develop --integration <integration_name>`。测试从生成的 `translations/en.json` 加载翻译,而不是直接读取 `strings.json`。
- 编写或修改测试时,确保所有测试函数参数都有类型注解。
- 优先使用具体类型(例如 `HomeAssistant`、`MockConfigEntry` 等)而不是 `Any`。
- 如果参数不会被用到,优先使用 `@pytest.mark.usefixtures` 而不是函数参数。
- 避免在测试中使用条件分支。应当拆分测试,或调整参数化方式,使所有用例都无需分支即可覆盖。
- 如果多个测试大部分代码相同,用 `pytest.mark.parametrize` 把它们合并成一个参数化测试,而不是复制函数体。用带 `id` 参数的 `pytest.param` 为测试用例清晰命名。
- 我们使用 Syrupy 做快照测试。尽量利用 `.ambr` 快照,而不是在 Python 代码里反复冗长地构造测试数据。
- 测试中硬编码 `entity_id` 没问题。如果同一个值反复出现,提取为常量。

## 最佳实践

- 在集成质量量表(Integration Quality Scale)中达到 Platinum 或 Gold 级别的集成,体现了较高的代码质量和可维护性标准。寻找参考示例时,这些集成是很好的起点。级别标注在集成的 manifest.json 中。
- 审阅实体动作时,不要为已经被 Home Assistant 的服务/动作 schema 和实体选择过滤器校验过的输入字段建议额外的防御性检查。只有当数据绕过这些校验器、或被转换成更不安全的形式时,才建议补充防护。
- 当校验已保证某个 dict 键存在时,优先直接按键访问(`data["key"]`)而不是 `.get("key")`,让契约违规暴露出来,而不是被悄悄掩盖。
- 注释保持简洁。最好用一行短句说明非显而易见的约束,或者干脆不写注释。
- 不要添加只是复述下一行(几)代码的注释(例如在 `if self.initialized:` 上方写 `# Check if initialized`)。注释只应解释"为什么"(非显而易见的约束、出人意料的行为或变通手段),永远不要解释"是什么"。绝不要添加通过引用改动前代码样貌来为变更辩护的注释。测试中解释某个函数调用或断言缘由的注释是可以的。
- 不要在函数内部或外部添加分节/分隔线注释(例如 `# --- XYZ Triggers ---`),这类注释很容易过时并造成误导。
- 捕获异常时,try 子句应尽量小,即避免把大段代码包进 try 子句,也避免捕获不预期抛出该异常的函数的异常。
- 敏感的服务动作,即可能更改配置或涉及安全的动作,应要求管理员用户。使用 `async_register_admin_service` 服务助手注册,它会自动做这项检查。

## AI 政策

本项目遵循 [Open Home Foundation AI 政策](AI_POLICY.md)。
不接受自主智能体的贡献:每处修改在提交前都必须由人工审阅、理解并能解释。
不要自主开启 issue 或 pull request,也不要在未经用户审阅的情况下以用户名义发表评论。
