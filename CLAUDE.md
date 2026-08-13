# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 先读 AGENTS.md

**[AGENTS.md](AGENTS.md) 是本仓库的唯一规范来源（single source of truth）。** 目录结构与命名、四阶段调研 SOP（扇出调研 → 事实核验 → 建站 → 上架）、视觉语言、本地验证与发布流程，全部以 AGENTS.md 为准。动手前先读它；本文件只补充 Claude Code 视角下不重复的要点。

## 这个仓库是什么

零依赖纯静态站点，部署在 GitHub Pages。**每个深度调研主题一个自包含子站**（一个中文名目录），根 `index.html` 是"调研台"首页，按领域分区索引所有子站。无构建系统、无依赖树——直接编辑 HTML/CSS，`python3 -m http.server 8000` 本地预览，push `main` 即发布。

新调研通常由 `/rp <主题>` 技能触发（`~/.claude/commands/rp.md` 是薄入口，流程实体在 AGENTS.md）。

## 建站必须成立的几条硬约束

- **报告编号全局递增**：新站的 `RP NN` 编号必须在首页卡片、总览页 eyebrow、README 表格三处一致。开工前先看 README「已发布档案」表拿到当前最大编号。
- **Phase D 是硬性收尾**：站点建完默认立即 commit + push（Conventional Commit，如 `feat: RP 11 <主题>调研站上架`），不等用户确认——没推上 `main` 的站等于没交付。
- **开工前先 `git status`**：若发现上一会话遗留的完整未提交站点，先按其编号单独补交，本次主题编号顺延。
- **`.nojekyll` 永不删除**：它让 Pages 直接服务中文路径与 `assets/`。
- **不接受无出处断言**：写入站点的关键数字/日期/因果结论都要经 Phase B 事实核验（`fact-checker` agent），并在各站 `sources.html` 登记来源 URL 与访问日期。

## 站点文件留在仓库内

工作区全局的「Markdown 一律存 Obsidian vault」规则**不适用于本项目的站点文件**——各主题的 HTML/CSS 就是本仓库的源码，属于「项目源文件」这一例外，直接写进对应主题目录。
