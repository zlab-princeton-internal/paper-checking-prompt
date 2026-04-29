[中文版](README_CN.md)

# Paper Checking Prompt

A prompt for checking academic ML papers with [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Built by [Zhuang Liu](https://liuzhuang13.github.io/).

**Related**: [Writing Guide](https://github.com/zlab-princeton-internal/writing-guide) · [Figure Guide](https://github.com/zlab-princeton-internal/figure-guide) · [Peer Review System](https://github.com/zlab-princeton-internal/peer-review)

> This guide is intended for [Zhuang Liu](https://liuzhuang13.github.io/)'s group members. Others are welcome to adopt or adapt it for their own use.

## Usage

```bash
git clone https://github.com/zlab-princeton-internal/paper-checking-prompt.git
```

Open Claude Code (**Opus model, max effort**) in your paper's project folder and say:

```
Read both paper-checking-prompt/PROMPT.md and paper-checking-prompt/PROMPT_CN.md, then check this paper.
```

That's it. Claude Code handles everything else.

P.S. You can try other coding agents (Codex, Gemini CLI, etc.) and compare results, but make sure you at least run it with **Claude Code + Opus + max effort** for the best quality.
