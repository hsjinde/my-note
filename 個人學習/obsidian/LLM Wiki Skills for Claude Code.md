---
title: "LLM Wiki Skills for Claude Code"
source: "https://gumroad.com/d/4a4eccda937bc1df3cb16edc85b68220"
author:
  - "[[HoChien Chang]]"
published:
created: 2026-06-03
description:
tags:
  - "clippings"
---
Create an account to access all of your purchases in one place

Receipt

[View receipt](https://gumroad.com/purchases/GXVIzfupi_LbAduS7_8Y5w==/receipt?email=g109029122%40gmail.com)

## LLM Wiki for Claude Code

A persistent, interlinked markdown knowledge base skill for Claude Code, inspired by Andrej Karpathy's LLM Wiki pattern.

## Installation

### Global (all projects)

```
mkdir -p ~/.claude/skills && git clone --depth 1 https://github.com/Changhochien/claude-wiki.git /tmp/claude-wiki && cp /tmp/claude-wiki/skills/llm-wiki/SKILL.md ~/.claude/skills/llm-wiki/
```

### Project-specific

```
mkdir -p .claude/skills/llm-wiki && git clone --depth 1 https://github.com/Changhochien/claude-wiki.git /tmp/claude-wiki && cp /tmp/claude-wiki/skills/llm-wiki/SKILL.md .claude/skills/llm-wiki/
```

## What It Does

- **Ingest** sources (URLs, files, notes) into a layered wiki structure
- **Query** the compiled knowledge base
- **Lint** for consistency, orphans, broken links, stale content
- Works with Obsidian as a vault

## Wiki Structure

```
~/wiki/               # or custom path
├── SCHEMA.md        # conventions & rules
├── index.md         # content catalog
├── log.md           # action history
├── raw/             # immutable sources
├── entities/        # people, orgs, products
├── concepts/        # topics & ideas
├── comparisons/     # side-by-side analysis
└── queries/        # filed Q&A
```

## Quick Start

1. Install the skill
2. Say "create a wiki" or "build a wiki for \[topic\]"
3. Claude will initialize the structure and guide first ingest

## Configuration

Set wiki path via environment variable:

```
export WIKI_PATH=~/my-knowledge-base
```

Or add to your `.omc-config.json`:

```
{ "wiki": { "path": "~/my-knowledge-base" } }
```