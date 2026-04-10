[English](README.md)

# 论文检查 Prompt

用 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 检查学术 ML 论文的 prompt。由 [Zhuang Liu](https://liuzhuang13.github.io/) 开发。

## 使用方法

```bash
git clone https://github.com/zlab-princeton-internal/paper-checking-prompt.git
```

在你论文的项目文件夹下打开 Claude Code（**Opus 模型，max effort**），说：

```
读 paper-checking-prompt/PROMPT.md 和 paper-checking-prompt/PROMPT_CN.md，然后检查这篇论文。
```

就这样。剩下的全部交给 Claude Code。

P.S. 你可以尝试其他编程助手（Codex、Gemini CLI 等）并比较结果，但请确保至少用 **Claude Code + Opus + max effort** 跑一遍以获得最佳质量。
