---
name: "git-commit"
description: "智能 Git 提交流程，自动分析变更、生成语义化提交信息并推送到远程仓库；当用户需要提交代码、推送变更或自动化 Git 工作流时使用"
---

# 自动 Git Commit

这个 skill 帮助你自动化 git add 和 git commit 流程，无需手动编写 commit message。

## 功能特性

1. **自动 Stage 所有变更**: 执行 `git add .` 添加所有修改、新增和删除的文件
2. **智能生成 Commit Message**: 基于 `git diff` 分析代码变更，自动生成描述性的 commit message
3. **一键提交**: 自动完成整个 commit 流程

## 使用场景

当用户说：
- "提交代码"
- "commit 这些改动"
- "git commit"
- "自动提交"
- "保存这些修改"
- 或任何表示想要提交代码的意图时

## 工作流程

Step1: 检查 Git 状态
   - 运行 `git status` 查看当前变更
   - 如果没有变更，提示用户并退出

Step2: 查看变更详情
   - 运行 `git diff --staged` (如果有已 stage 的文件)
   - 运行 `git diff` (查看未 stage 的文件变更)
   - 获取文件列表和变更统计

Step3（可选步骤）: 切出新分支
   - 仅当用户确认需要切换分支时执行
   - 根据变更详情中主要的变更类型，生成遵循 Conventional Commits 格式的类型作为分支前缀（如 `feat/`），然后生成新分支名称（如 `feat/create-new-skill`）
   - 运行 `git branch checkout -b <branch-name>` 切换到新分支,这里的 `<branch-name>` 是根据变更详情生成的新分支名称

Step4: 自动 Stage 所有变更
   - 执行 `git add .` 添加所有修改、新增和删除的文件

Step5: 分析并生成 Commit Message
   - 基于 diff 内容，分析代码变更
   - 生成清晰、描述性的 commit message
   - Commit message 应该：
     - 使用简洁的中文描述（如果代码库主要是中文项目）
     - 遵循 Conventional Commits 规范（如 feat:, fix:, refactor:, chore: 等）
     - 准确反映主要变更内容
     - 如果有多个不相关的变更，在 message body 中分点说明
   - 使用生成的 commit message 执行 `git commit -m "message"`
   - 显示 commit 结果和 commit hash

Step6（可选步骤）: 提交到远程仓库
   - 如果用户确认需要 push：
     - 检查是否有远程仓库配置
     - 执行 `git push` 推送到远程仓库
     - 显示 push 结果
   - 如果用户不需要 push：
     - 提示用户稍后可以手动执行 `git push`

## Commit Message 格式规范

遵循 Conventional Commits 格式：

```
<type>: <subject>

[optional body]
```

### Type 类型：
- `feat`: 新功能
- `fix`: 修复 bug
- `refactor`: 重构代码
- `style`: 代码格式调整（不影响功能）
- `docs`: 文档更新
- `test`: 测试相关
- `chore`: 构建工具、依赖等杂项
- `perf`: 性能优化

### 示例：

简单变更：
```
feat: 添加用户登录功能
```

复杂变更：
```
feat: 实现用户认证系统

- 添加登录和注册页面
- 实现 JWT token 认证
- 添加密码加密功能
```

## 注意事项

1. **运行前检查**: 
   - 确保在 git 仓库中运行
   - 确保有未提交的变更

2. **安全考虑**:
   - 不要 commit 敏感信息（密钥、密码等）
   - 检查 diff 中是否包含不应提交的内容

3. **最佳实践**:
   - 鼓励原子性提交（每次 commit 只关注一个功能或修复）
   - 如果变更过多过杂，建议用户分多次提交

## 错误处理

- **没有变更**: 提示用户当前没有需要提交的变更
- **不在 git 仓库**: 提示用户当前目录不是 git 仓库
- **Commit 失败**: 显示错误信息，建议用户检查问题
- **有冲突文件**: 提示用户先解决冲突再提交
- **没有远程仓库**: 如果用户选择 push 但没有配置远程仓库，提示用户先运行 `git remote add origin <url>`
- **Push 失败**: 显示错误信息（如没有权限、网络问题等），建议用户检查并稍后手动 push
