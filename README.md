[中文版](README_CN.md)

# Paper Checking Prompt

A prompt for checking academic ML papers with [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Built by [Zhuang Liu](https://liuzhuang13.github.io/).

## Usage

```bash
git clone https://github.com/zlab-princeton-internal/paper-checking-prompt.git
```

Open Claude Code (**Opus model, max mode**) in your paper's project folder and say:

```
Read paper-checking-prompt/PROMPT.md (or PROMPT_CN.md for Chinese), then check this paper.
```

That's it. Claude Code handles everything else.

P.S. You can try other coding agents (Codex, Gemini CLI, etc.) and compare results, but make sure you at least run it with **Claude Code + Opus + max mode** for the best quality.
