# 版本记录与同步规范 (CONVENTIONS)

> 目标：**每次修改都可追溯、可回退**。本仓库即档案的版本系统，不额外引入别的表。

## 1. 分支与合并

| 项 | 约定 |
|---|---|
| 默认分支 | `main`（唯一发布分支） |
| 日常改动 | 建分支 `docs/<简短主题>` 或本地直接改后提 PR |
| 合并方式 | PR → 本人审阅 → Squash merge（保持历史线性、一改动一 commit） |
| 禁止 | 直接 force push 覆盖 `main`；无 commit message 的空提交 |

## 2. Commit Message 规范

格式：`<类型>: <一句话说清改了什么>`

| 类型 | 用途 |
|---|---|
| `docs` | 档案正文增删改（最常用） |
| `chore` | 模板、配置、gitignore 等维护 |
| `refactor` | 章节结构调整，不改事实 |
| `revert` | 回退某次改动 |

示例：
```
docs: 补充「常用工具与技术栈」中的本地模型网关条目
docs: 将沟通风格偏好改为结论前置、一句一事
revert: 回退「长期目标」的副业阶段数值（数据未确认）
```

## 3. 追溯（查谁改了什么、何时改的）

```bash
git log --oneline --decorate          # 全部改动一览
git log -- README.md                  # 只看 README 的改动历史
git blame README.md                   # 逐行看每行最后是谁改的
git show <sha>                        # 看某次改动的完整 diff
```

网页同样可查：仓库 → **Commits** → 任意 commit 看 diff。

## 4. 回退（改错了怎么撤）

```bash
# 推荐：生成一次「反向提交」，历史完整保留，可再回退
git revert <sha>
git push

# 仅本地未推送时：撤销最近一次提交但保留改动
git reset --soft HEAD~1

# 恢复某个文件到历史版本
git checkout <sha> -- README.md
git commit -m "revert: 恢复 README 到 <sha> 版本"
```

**铁律**：已推送到 `main` 的改动，只用 `git revert`，不用 `git reset --hard`。

## 5. 里程碑标记（可选）

阶段填充完成后打 tag，便于一键定位：

```bash
git tag -a v0.1 -m "档案骨架建立"
git tag -a v1.0 -m "六章内容填充完成"
git push --tags
```

## 6. 跨 AI / 跨设备同步

- 任一设备开工先 `git pull`，避免覆盖别人的改动。
- 任一 AI 改完档案，必须 `git commit` + `git push`，并在会话里回报 commit 短 sha。
- 各 AI 只读 `main` 的最新版；有分歧以仓库为准，不以聊天记录为准。

## 7. 待办跟踪

- 用 **Issues**，模板已配 `.github/ISSUE_TEMPLATE/todo.md`。
- 一个待办 = 一个 Issue；完成即 close，close 时在评论里写对应 commit sha。
- 不在 README 正文里塞临时待办清单。

## 8. 公开仓库隐私红线

- 禁止写入：密码 / token / 密钥 / 身份证 / 内部系统地址 / 未公开经营数据 / 他人姓名与联系方式。
- 拿不准是否敏感 → 用占位符，或改放私有仓库。
- 一旦误提交敏感信息：立即从历史中清除并视情况重置凭证，**不要只删文件了事**。
