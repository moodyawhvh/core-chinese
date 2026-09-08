> 🌐 本文档由 [home-assistant/core](https://github.com/home-assistant/core) 翻译,英文原版见原项目。

---
name: bump-dependency
description: Bumps a Python package dependency across Home Assistant Core integrations, regenerates core requirement files, runs verification tests and prek lint, and prepares a pull request with proper release/compare links.
---

# 在 Home Assistant Core 中升级 Python 依赖包

按以下系统性步骤,成功升级仓库中的 Python 包依赖,重新生成必要的派生文件,验证集成,并发起 pull request。

## 陷阱与隐蔽约束

- **PR 模板完整性**:严格遵循 Home Assistant 的 Pull Request 模板(`.github/PULL_REQUEST_TEMPLATE.md`),包括模板内部的指示。保留所有章节、注释和未勾选的复选框,除非模板明确另有说明;唯一允许的删除是在不适用时按模板指示移除 **Breaking change** 章节。
- **GitHub 标签易变性**:GitHub 上的发布标签格式非常不统一(如 `v1.2.3`、`1.2.3`、`release-1.2.3`)。在硬编码比较 URL 之前,务必先用自动解析器 `resolve_dependency.py` 检查 HEAD 状态,获取正确的标签。

## 分步工作清单

### 阶段 A:调研与规划
- [ ] **1. 确定目标**:记录要求升级的目标包和目标版本。
- [ ] **2. 发现代码引用**:搜索代码库,找到所有引用该包的 `manifest.json` 和 requirements 文件。
- [ ] **3. 解析版本/标签细节**:运行内置的校验辅助脚本,解析版本细节、GitHub 仓库、发布标签格式和格式化的 PR 链接:
  ```bash
  uv run --no-sync python3 ./.claude/skills/bump-dependency/scripts/resolve_dependency.py <package> <old_version> [--new-version <new_version>]
  ```
- [ ] **4. 计划-校验-执行(草案)**:修改任何文件之前,先写一份简短的结构化计划,列出要改的集成、旧版本、新版本和解析出的比较链接。把这份草案展示给用户。

### 阶段 B:执行与验证(本地改动)
- [ ] **5. 检查未提交改动**:检查仓库中是否有未提交改动。如有,询问用户是 stash、提交还是丢弃,然后再继续。
- [ ] **6. Git 分支准备**:从最新 `upstream/dev` 创建干净分支:
  ```bash
  git fetch upstream dev
  git checkout -b bump-<package>-to-<version> upstream/dev
  ```
- [ ] **7. 应用版本升级到 manifests**:更新所有相关 `manifest.json` 中的版本约束字符串(例如把 `"package==1.0.0"` 改为 `"package==1.1.0"`)。
- [ ] **8. 重新生成核心 requirements**:运行 requirements 生成器,更新所有派生的 requirements 和约束文件:
  ```bash
  uv run --no-sync python3 -m script.gen_requirements_all
  ```
- [ ] **9. 校验 requirements**:用 `git diff` 确认只有目标 `manifest.json` 文件和 `requirements_all.txt`(以及可能的标准约束文件)被修改。不得影响无关文件。
- [ ] **10. 本地 venv 验证**:在虚拟环境中直接安装精确的目标包版本:
  ```bash
  uv pip install "<package>==<version>"
  ```

### 阶段 C:验证循环(测试与 lint)
- [ ] **11. 运行集成测试**:对所有使用被升级包的集成执行 pytest:
  ```bash
  uv run --no-sync pytest tests/components/<integration_name>
  ```
  - *验证循环*:若测试失败,分析错误、应用合适的修复,重跑 pytest 直到全部干净通过。
- [ ] **12. 运行 prek lint 检查**:对修改的文件运行本地 prek 钩子:
  ```bash
  uv run --no-sync prek run
  ```
  - *验证循环*:若 prek 报告任何格式或 lint 违规,修复后重复 `uv run --no-sync prek run`,直到完全无错通过。

### 阶段 D:用户确认与 PR 创建
- [ ] **13. 提交改动**:提交干净的改动:
  ```bash
  git add <modified_files>
  git commit -m "Bump <package> to <version>"
  ```
- [ ] **14. 推送分支**:把本地分支推送到 origin 远端:
  ```bash
  git push origin bump-<package>-to-<version>
  ```
- [ ] **15. 准备 PR 描述**:基于 `.github/PULL_REQUEST_TEMPLATE.md` 生成 pull request 正文:
  - **Proposed change**:描述包名、旧版本、新版本、目标/源分支,并插入解析出的 PyPI、changelog 和比较 diff 链接。
  - **Type of change**:本节只勾选 1 项,勾选 `Dependency upgrade` 复选框:`[x] Dependency upgrade`。
  - **Breaking change**:可以从模板中整体删除 "Breaking change" 章节。
  - **验证清单**:勾选 `The code change is tested` 复选框:`[x] The code change is tested`。
  - **保留其余模板**:不要删除模板中任何其他被注释的块、标题或未勾选的复选框。
- [ ] **16. 强制评审展示**:用下面的 **PR 展示模板** 格式化 PR 提案并展示给用户。**停下并等待用户审阅、明确确认/批准 PR 模板和草稿细节后,再创建 PR。**
- [ ] **17. 发起 Pull Request**:用户批准后,用 GitHub CLI 创建 Pull Request:
  ```bash
  gh pr create --repo home-assistant/core --base dev --head <username>:bump-<package>-to-<version> --title "Bump <package> to <version>" --body-file <pr_body_file>
  ```

## PR 展示模板

```markdown
### 🚀 Dependency Bump Pull Request Draft Review

- **Package**: `<package_name>` (`<old_version>` → `<new_version>`)
- **PR Title**: `Bump <package_name> to <new_version>`
- **Target Branch**: `dev`
- **Head Branch**: `<fork_username>:bump-<package_name>-to-<new_version>`

#### 🔗 PyPI & GitHub Links
- **PyPI Release**: https://pypi.org/project/<package_name>/<new_version>/
- **Changelog Link**: `<changelog_url>`
- **Comparison Diff**: `<compare_url>`

#### 📁 Modified Files
- `<list_of_modified_files>`

#### 📝 Proposed PR Body
<render the complete filled PR template body here, showing all checks and modifications for user approval>
```
