# Agent Skills by Mostapha Kalami Heris

A collection of agent skills that give AI agents structured, reusable workflows for tackling complex problems. These skills go beyond simple prompting to deliver consistently higher-quality results.

*More skills will be added over time.*

---

## Table of Contents

- [Agent Skills by Mostapha Kalami Heris](#agent-skills-by-mostapha-kalami-heris)
  - [Table of Contents](#table-of-contents)
  - [Skills](#skills)
    - [`evo-search`](#evo-search)
    - [`tournament-search`](#tournament-search)
    - [`prompt-decorators`](#prompt-decorators)
  - [Installation](#installation)
    - [CLI Tools (Claude Code, Gemini CLI, VS Code, etc.)](#cli-tools-claude-code-gemini-cli-vs-code-etc)
    - [Online AI Assistant Platforms (ChatGPT, Claude, Gemini, etc.)](#online-ai-assistant-platforms-chatgpt-claude-gemini-etc)
  - [How to Cite](#how-to-cite)
  - [License](#license)

---

## Skills

> **Note on token usage:** Skills like `evo-search` and `tournament-search` generate and evaluate multiple candidate responses across several rounds. Certain decorators in `prompt-decorators` also add extra reasoning or refinement steps. This increases token consumption significantly compared to a single plain prompt. The benefit is higher response quality, but it is not free. If you are on a metered subscription, running these skills can cost noticeably more. Consider using lighter configurations (for example, fewer iterations, a smaller population, or simpler decorators) when your token budget is limited.

### `evo-search`

Solves complex, open-ended problems using a **genetic-algorithm-style search loop**.

It generates a diverse initial population of candidate answers, scores each one against a weighted rubric, and then repeatedly applies selection, crossover, and mutation to produce stronger solutions. The final output includes the top 3 solutions with scores, a contrast table, and an evolution trace.

**When this skill is triggered:**
- The user asks for an "evolutionary search", "genetic algorithm", "evolve an answer", or "breed solutions"
- The user says "give me multiple approaches and refine the best one"
- The problem is complex or open-ended with no single correct answer, and the user wants a deeply explored result

**Key parameters** (set in plain language):

| Parameter | Default | Effect |
|-----------|---------|--------|
| `population` | 6 | Candidates per generation |
| `iterations` | 5 | Evolution cycles |
| `mutation_rate` | 0.2 | Fraction of offspring mutated |
| `top_k` | 3 | Final solutions shown in full |

**Install:**
- **CLI tools:** run in your terminal:
  ```sh
  npx skills@latest add smkalami/skills/evo-search
  ```
- **AI assistant platforms:** download and upload [`evo-search.skill`](dist/evo-search.skill)

**Example usage:**
```
Use evo-search to invent a new programming paradigm that blends constraint propagation
with biological growth patterns. Something that does not exist yet but could realistically
be implemented. Define its core model, syntax philosophy, and a killer use case.
```

```
Evolve an answer: design a radically new way for human teams to make high-stakes decisions
under uncertainty, combining ideas from mechanism design, cognitive science, and swarm
intelligence, without relying on voting, consensus, or hierarchy.
```

---

### `tournament-search`

Solves complex, open-ended problems using a **knockout bracket tournament**.

It generates a diverse set of candidate answers, draws a random bracket, and runs head-to-head matches. Each match produces a hybrid solution that combines the strengths of both competitors, and the strongest of the three advances. A 3rd-place playoff is also included. This skill is faster than `evo-search` and works best when you want a clear winner quickly.

**When this skill is triggered:**
- The user asks for a "tournament", "bracket-style search", "knockout search", or says "let solutions compete"
- The user says "find the best answer by elimination" or "which solution wins?"
- The problem is open-ended and the user wants a clear winner without deep multi-generation refinement

**Key parameters** (set in plain language):

| Parameter | Default | Effect |
|-----------|---------|--------|
| `population` | 8 | Initial candidates (must be a power of 2) |
| `top_k` | 3 | Podium solutions shown in full |
| `trait_logging` | off | Log best ideas from eliminated candidates |

**Install:**
- **CLI tools:** run in your terminal:
  ```sh
  npx skills@latest add smkalami/skills/tournament-search
  ```
- **AI assistant platforms:** download and upload [`tournament-search.skill`](dist/tournament-search.skill)

**Example usage:**
```
Run a tournament to find the best architecture for a city-scale autonomous mobility system
that handles mixed traffic of human drivers, autonomous vehicles, cyclists, and pedestrians.
Each candidate must address safety, latency, failure modes, and rollout strategy.
```

```
Let solutions compete: propose a new economic model for funding open-source software
that does not rely on donations, corporate sponsorship, or government grants.
Each proposal must be self-sustaining and implementable within 5 years.
```

---

### `prompt-decorators`

> The original (non-skill) version of this framework is available at [smkalami/prompt-decorators](https://github.com/smkalami/prompt-decorators).
>
> **Paper:** Mostapha Kalami Heris, "Prompt Decorators: A Declarative and Composable Syntax for Reasoning, Formatting, and Control in LLMs", arXiv preprint [arXiv:2510.19850](https://arxiv.org/abs/2510.19850) (2025).

Activates a **structured decorator framework** that controls how the AI reasons, responds, and formats its output, using lightweight `+++` markers.

Decorators are placed at the top of a prompt and applied at the same time. Each one controls a different aspect of the response, such as reasoning style, tone, output format, or verification. They can be applied to a single message or kept active for an entire chat session.

**When this skill is triggered:**
- The prompt contains one or more `+++` markers (for example, `+++Reasoning`, `+++StepByStep`, or `+++Tone(style=formal)`)
- The user asks to "activate", "list", or "clear" decorators
- The user references the "prompt decorator framework" or says "use decorator" / "apply +++"

**Decorator categories:**

| Category | Decorators |
|----------|------------|
| Reasoning and structure | `+++Reasoning`, `+++StepByStep`, `+++Socratic`, `+++Debate` |
| Evaluation and refinement | `+++Critique`, `+++Refine(iterations=N)`, `+++Candor(level=L)` |
| Exploration and planning | `+++Brainstorm`, `+++Planning`, `+++Interactive`, `+++Import(topic=T)` |
| Rewriting and clarification | `+++Rewrite` |
| Output control | `+++OutputFormat(format=F)`, `+++Tone(style=S)` |
| Verification | `+++FactCheck`, `+++CiteSources`, `+++Validate` |

**Session management:**
- `+++ChatScope`: persist decorators across the entire session
- `+++Clear`: remove all active session-scoped decorators
- `+++ActiveDecs`: list currently active decorators
- `+++AvailableDecs`: list all available decorators

**Install:**
- **CLI tools:** run in your terminal:
  ```sh
  npx skills@latest add smkalami/skills/prompt-decorators
  ```
- **AI assistant platforms:** download and upload [`prompt-decorators.skill`](dist/prompt-decorators.skill)

**Example usage:**
```
+++Rewrite
+++Reasoning

Please design a teaching plan for a 10-week course on machine learning
to be taught to college students with no prior experience in the subject.
```

```
+++Brainstorm
+++Refine(iterations=3)

Propose novel interaction paradigms for human-AI collaboration in creative work
that go beyond chat and prompt-response. Think beyond current interfaces.
```

---

## Installation

Each skill above lists both installation options. There are two approaches depending on how you use AI tools.

### CLI Tools (Claude Code, Gemini CLI, VS Code, etc.)

Run the `npx` command listed beside the skill you want:

```sh
npx skills@latest add smkalami/skills/<skill-name>
```

This downloads the skill files and registers them with your CLI tool automatically. No further configuration is required. The skill is picked up the next time you start a session.

### Online AI Assistant Platforms (ChatGPT, Claude, Gemini, etc.)

Download the `.skill` file linked beside the skill you want and upload it directly to your platform.

- **Simple skills** (single `SKILL.md`): you can also upload the raw `SKILL.md` file instead of the `.skill` archive.
- **Complex skills** (multiple reference files, such as `prompt-decorators`): use the `.skill` archive, which bundles all required files together.

---

## How to Cite

If you use these skills in your work or research, please cite this repository:

```bibtex
@misc{kalami-heris-2026-agent-skills,
  author       = {Kalami Heris, Mostapha},
  title        = {Agent Skills by Mostapha Kalami Heris},
  year         = {2026},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/smkalami/skills}},
}
```

If you use the `prompt-decorators` skill or framework, please also cite the paper:

```bibtex
@misc{kalami-heris-2025-prompt-decorators,
  author        = {Kalami Heris, Mostapha},
  title         = {Prompt Decorators: A Declarative and Composable Syntax for Reasoning, Formatting, and Control in LLMs},
  year          = {2025},
  eprint        = {2510.19850},
  archivePrefix = {arXiv},
  primaryClass  = {cs.AI},
  url           = {https://arxiv.org/abs/2510.19850},
}
```

---

## License

MIT. See [LICENSE](LICENSE).
