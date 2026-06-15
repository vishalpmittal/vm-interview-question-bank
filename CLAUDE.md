# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a personal **interview question bank** — a content repository, not a software project. There is no build, no tests, no runtime. All content is markdown.

## Structure

Questions are organized by category, one folder per category. Each category folder contains a `README.md` describing the category and conventions, plus one or more `.md` files holding the questions themselves.

Current categories:
- `behavioral/` — STAR-method behavioral questions, grouped by competency area within a single consolidated file
- `system-design/` — System design prompts
- `leadership/` — Engineering leadership and management questions
- `logical-reasoning/` — Estimation, probability, and analytical puzzles

## Conventions When Adding Content

- **Add new categories as top-level folders** with a `README.md` that mirrors the structure of existing category READMEs (purpose, suggested topics, file conventions).
- **Behavioral-style content** follows a fixed section template per competency: numbered question list, then `**What to listen for:**`, `**Follow-ups:**`, `**Red flags:**` bullet blocks. Preserve this pattern when extending `behavioral/behavioral-questions.md` or adding similar evaluator-facing question sets.
- **Update the root `README.md`** category list when adding a new top-level category.
- **One `.md` file per topic cluster or competency area** — avoid sprawling many tiny files or one monolithic file across categories.

## Source of Truth for Behavioral Questions

The behavioral questions in `behavioral/behavioral-questions.md` were imported from `/Users/vimittal/workspace/airs-gitlab/airs/shared-services/core-services/interviews/behavioral/`. If syncing future updates, that path is the upstream.
