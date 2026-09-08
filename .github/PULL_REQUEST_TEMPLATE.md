> 🌐 本文档由 [home-assistant/core](https://github.com/home-assistant/core) 翻译,英文原版见原项目。

<!--
  You are amazing! Thanks for contributing to our project!
  Please, DO NOT DELETE ANY TEXT from this template! (unless instructed).
-->
## 破坏性变更(Breaking change)
<!--
  If your PR contains a breaking change for existing users, it is important
  to tell them what breaks, how to make it work again and why we did this.
  This piece of text is published with the release notes, so it helps if you
  write it towards our users, not us.
  Note: Remove this section if this PR is NOT a breaking change.
-->


## 拟议修改
<!--
  Describe the big picture of your changes here to communicate to the
  maintainers why we should accept this pull request. If it fixes a bug
  or resolves a feature request, be sure to link to that issue in the
  additional information section.
-->


## 变更类型
<!--
  What type of change does your PR introduce to Home Assistant?
  NOTE: Please, check only 1! box!
  If your PR requires multiple boxes to be checked, you'll most likely need to
  split it into multiple PRs. This makes things easier and faster to code review.
-->

- [ ] 依赖升级
- [ ] Bug 修复(非破坏性,修复某个问题)
- [ ] 新集成(谢谢!)
- [ ] 新功能(为现有集成添加功能)
- [ ] 弃用(将来会发生的破坏性变更)
- [ ] 破坏性变更(导致现有功能失效的修复/功能)
- [ ] 现有代码的质量改进或新增测试

## 补充信息
<!--
  Details are important, and help maintainers processing your PR.
  Please be sure to fill out additional details, if applicable.
-->

- 本 PR 修复或关闭 issue: fixes #
- 本 PR 关联 issue: 
- 文档 pull request 链接: 
- 开发者文档 pull request 链接: 
- 前端 pull request 链接: 

## 检查清单
<!--
  Put an `x` in the boxes that apply. You can also fill these out after
  creating the PR. If you're unsure about any of them, don't hesitate to ask.
  We're here to help! This is simply a reminder of what we are going to look
  for before merging your code.

  AI tools are welcome, but contributors are responsible for *fully*
  understanding the code before submitting a PR. Please follow our AI policy:
  https://developers.home-assistant.io/docs/ai_policy
-->

- [ ] 我理解所提交的代码,并能解释其工作原理。
- [ ] 代码改动已测试且本地运行正常。
- [ ] 本地测试通过。**测试不通过 PR 将无法合并**
- [ ] 本 PR 中没有被注释掉的代码。
- [ ] 我已遵循[开发检查清单][dev-checklist]
- [ ] 我已遵循[完美 PR 建议][perfect-pr]
- [ ] 代码已使用 Ruff 格式化(`ruff format homeassistant tests`)
- [ ] 已添加测试以验证新代码正常工作。
- [ ] 任何生成的代码都经过认真审阅,确认正确并符合项目规范。

如果新增/修改了面向用户的功能或配置变量:

- [ ] 已为 [www.home-assistant.io][docs-repository] 添加/更新文档

如果代码与设备、Web 服务或第三方工具通信:

- [ ] [manifest 清单文件][manifest-docs]的所有字段均已正确填写。  
      已运行 `python3 -m script.hassfest` 更新并包含派生文件。
- [ ] 新增/更新的依赖已加入 `requirements_all.txt`。  
      已通过 `python3 -m script.gen_requirements_all` 更新。
- [ ] 对于更新的依赖,已在 PR 描述中附上库版本间的 diff,最好附上变更日志/发布说明链接。

<!--
  This project is very active and we have a high turnover of pull requests.

  Unfortunately, the number of incoming pull requests is higher than what our
  reviewers can review and merge so there is a long backlog of pull requests
  waiting for review. You can help here!
  
  By reviewing another pull request, you will help raise the code quality of
  that pull request and the final review will be faster. This way the general
  pace of pull request reviews will go up and your wait time will go down.
  
  When picking a pull request to review, try to choose one that hasn't yet
  been reviewed.

  Thanks for helping out!
-->

为分担涌入的 PR 压力:

- [ ] 我已审阅了本仓库中另外两个[开放的 pull request][prs]。

[prs]: https://github.com/home-assistant/core/pulls?q=is%3Aopen+is%3Apr+-author%3A%40me+-draft%3Atrue+-label%3Awaiting-for-upstream+sort%3Acreated-desc+review%3Anone+-status%3Afailure

<!--
  Thank you for contributing <3

  Below, some useful links you could explore:
-->
[dev-checklist]: https://developers.home-assistant.io/docs/development_checklist/
[manifest-docs]: https://developers.home-assistant.io/docs/creating_integration_manifest/
[quality-scale]: https://developers.home-assistant.io/docs/integration_quality_scale_index/
[docs-repository]: https://github.com/home-assistant/home-assistant.io
[perfect-pr]: https://developers.home-assistant.io/docs/review-process/#creating-the-perfect-pr
