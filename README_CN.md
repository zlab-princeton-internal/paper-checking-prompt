[English](README.md)

# AI 论文检查

用 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 检查学术 ML 论文的 prompt。由 [Zhuang Liu](https://liuzhuang13.github.io/) 开发。

**相关**: [Writing Guide](https://github.com/zlab-princeton-internal/writing-guide) · [Figure Guide](https://github.com/zlab-princeton-internal/figure-guide) · [Peer Review System](https://github.com/zlab-princeton-internal/peer-review) · [Writing Self-Review](https://github.com/zlab-princeton-internal/paper-rating)

> 本指南面向 [Zhuang Liu](https://liuzhuang13.github.io/) 组内成员。欢迎其他人参考或改编使用。

## 使用方法

```bash
git clone https://github.com/zlab-princeton-internal/ai-paper-checking.git
```

在你论文的项目文件夹下打开 Claude Code（**Opus 模型，max effort**），说：

```
读 ai-paper-checking/PROMPT.md 和 ai-paper-checking/PROMPT_CN.md，然后检查这篇论文。
```

就这样。剩下的全部交给 Claude Code。

P.S. 你可以尝试其他编程助手（Codex、Gemini CLI 等）并比较结果，但请确保至少用 **Claude Code + Opus + max effort** 跑一遍以获得最佳质量。
