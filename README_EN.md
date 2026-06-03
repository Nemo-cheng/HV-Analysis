[中文](README.md) | **English**

# HV-Analysis — A Personal Knowledge Base That Compounds

> Research anything deeply with the HV-Analysis method, then let that knowledge persist and connect with everything you've learned before.

---

## Why This Exists

When you use AI to research something, the experience usually goes like this:

You ask a great question, the AI gives a great answer, and then the conversation ends. The next time you research something else, that previous knowledge is gone — or rather, it never truly *existed*. It was assembled once, temporarily, and discarded.

This project is about making every deep research session **actually stick** — not just archived, but woven into a growing network of connected knowledge.

---

## Three Ideas, One System

This project combines three independent ideas:

**1. [KKKKhazix's hv-analysis skill](https://github.com/KKKKhazix/khazix-skills)**

The Horizontal-Vertical Analysis method: trace the complete historical arc of a subject from origin to present (vertical), then compare it against its contemporaries and competitors at a single point in time (horizontal), and synthesize both into new judgments. One research session produces a 10,000–30,000 word deep report.

This solves the problem of *how to research something thoroughly*.

**2. [Karpathy's llm-wiki framework](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)**

Karpathy's core insight: most people use AI for knowledge work via RAG — every query re-retrieves and re-assembles from raw documents from scratch. Nothing accumulates.

The llm-wiki approach is different: let the LLM actively maintain a set of structured, interlinked wiki pages. Every time a new source is ingested, the LLM doesn't just archive it — it updates related pages, builds cross-document connections, and flags contradictions. Knowledge **compounds over time** instead of being rediscovered from scratch every session.

**3. Obsidian**

All wiki files are standard Markdown + frontmatter, with `[[WikiLink]]` syntax that works natively in Obsidian. The Obsidian graph view makes the connections between knowledge nodes visible — you can literally watch a knowledge network grow.

---

## How It Works

```
One hv-analysis research session
        ↓
Deep report (vertical + horizontal + synthesis)
        ↓
Auto-archived into a three-tier knowledge structure:
  Tier 1  raw/comparisons/   Full reports (immutable after writing)
  Tier 2  wiki/concepts/     A relationship page for each key concept
           wiki/entities/    A relationship page for each company/person/product
  Tier 3  wiki/overview/     A knowledge map generated after 3+ reports in the same domain
        ↓
Next time you research the same domain, concept and entity pages update automatically
— you can see "what role OpenAI played across all 5 projects I've analyzed"
```

As research accumulates, your knowledge isn't a pile of isolated files — it's a network that grows denser with every session.

---

## Installation

Tell your Agent directly:

```
Install this skill for me: https://github.com/Nemo-cheng/HV-Analysis
```

Works with Claude Code, Codex, OpenCode, and any tool compatible with the Agent Skills spec.

**If your Agent can't install automatically, use the command line:**

Codex (Windows PowerShell):
```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" --url https://github.com/Nemo-cheng/HV-Analysis
```

Claude Code (manual git clone):
```bash
git clone https://github.com/Nemo-cheng/HV-Analysis ~/.claude/skills/hv-analysis
```

---

## Usage

**Start a research session:**

```
I want to learn about [subject]
```

**Direct mode** (if you already have a report file, skip research and go straight to PDF + archive):

```
Here's a report I already wrote: /path/to/report.md — generate a PDF and archive it
```

**First-time setup**: when the skill reaches the archive step (Step 6), it will ask for your wiki root directory path and automatically scaffold the full directory structure.

---

## Knowledge Base Structure

```
your-wiki-root/
├── TheSchema.md        ← Operational contract (LLM reads this before every archive)
├── raw/
│   └── comparisons/    ← Tier 1: full reports (immutable after writing)
├── wiki/
│   ├── index.md        ← Navigation hub
│   ├── concepts/       ← Tier 2: concept relationship pages
│   ├── entities/       ← Tier 2: entity relationship pages
│   └── overview/       ← Tier 3: knowledge maps (triggered at 3+ reports per domain)
└── _meta/
    └── taxonomy.md     ← Controlled tag taxonomy
```

---

## Credits

- **HV-Analysis methodology**: [KKKKhazix/khazix-skills](https://github.com/KKKKhazix/khazix-skills)
- **llm-wiki framework inspiration**: [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

---

MIT License
