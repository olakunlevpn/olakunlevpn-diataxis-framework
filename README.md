# Diátaxis Framework

[![GitHub stars](https://img.shields.io/github/stars/olakunlevpn/olakunlevpn-diataxis-framework?style=flat-square)](https://github.com/olakunlevpn/olakunlevpn-diataxis-framework/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)

A systematic framework for technical documentation, structured around four distinct user needs: tutorials (learning-oriented), how-to guides (goal-oriented), reference (information-oriented), and explanation (understanding-oriented).

This is an **agent skill** that works with Claude Code, Cursor, Cline, Gemini CLI and 40+ other AI coding agents.

## Installation

```bash
npx skills add olakunlevpn/olakunlevpn-diataxis-framework
```

## The Problem

Documentation often fails not because the author lacked knowledge, but because different kinds of documentation were blended together. A tutorial that pauses to explain theoretical architecture loses the learner. A reference that gives step-by-step instructions frustrates someone who just needs an API signature. An explanation buried inside a how-to guide gets skipped by someone trying to solve an urgent task.

When forms blur together, every form gets worse.

## The Solution

Diátaxis (from Greek *dia* "across" + *taxis* "arrangement") provides a systematic compass based on two fundamental axes:
- **Action** (doing) vs **Cognition** (thinking)
- **Acquisition** (learning/study) vs **Application** (working)

This yields four distinct forms of documentation:
1. **Tutorials** — Learning-oriented lessons that take the beginner by the hand.
2. **How-To Guides** — Goal-oriented directions for competent users solving a problem.
3. **Reference** — Information-oriented technical descriptions of the machinery.
4. **Explanation** — Understanding-oriented discussions that illuminate concepts and context.

Includes the `--sudo` flag to enforce real, working, verified code across every section.

## How to Use

The skill auto-triggers when your agent writes, reviews, or audits technical documentation. You can also invoke it directly:

```
Review our docs structure against the Diátaxis framework
```
```
Write a how-to guide for setting up webhook verification --sudo
```
```
Restructure this API reference to separate reference from explanation
```

## Files

```
SKILL.md        # Main skill file (loaded by AI agents)
LICENSE.md      # MIT License
README.md       # Documentation and usage guide
references/     # Complete 18-page reference library for the framework
```

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
