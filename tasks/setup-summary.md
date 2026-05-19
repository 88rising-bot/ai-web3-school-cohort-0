# Setup Summary — AI × Web3 School 技术栈搭建

> 记录人：88rising-bot (子悦)
> 日期：2026-05-19

---

## 1️⃣ 服务器 — Tencent Cloud (Singapore)
- 海外新加坡节点，Linux (Ubuntu)
- 用于运行 Hermes Agent 网关，24h 在线

## 2️⃣ Hermes Agent — 核心 Agent 框架
- 开源 AI Agent 框架（Nous Research）
- 支持多平台接入、Cron 定时任务、持久记忆、技能系统
- 当前模型：DeepSeek V4 Flash
- 已配置技能可扩展

## 3️⃣ Telegram — 已接入 ✅
- Bot：`cendrine_bot`
- 当前对话通道，支持语音、文字、文件收发

## 4️⃣ 微信 — 已接入 ✅
- WeChat 通道已连接
- Home channel 设为微信端

## 5️⃣ GitHub — 已接入 ✅
- 用户名：`88rising-bot`
- 学习仓库：`ai-web3-school-cohort-0` (public)
- 本地路径：`~/ai-web3-school-cohort-0`
- 已配置 gh CLI 认证

## 6️⃣ 定时任务 — 已配置
- 早上 10:00 (UTC+8) — Web3 School 学习提醒
- 傍晚 16:00 (UTC+8) — 打卡检查提醒
- 每周日 — 配置健康检查

## 7️⃣ 记忆系统 — 已优化
- MEMORY.md: ~1KB，只存客观事实
- USER.md: 精简，存用户偏好
- agent_persona.md: 已自定义人设

## 8️⃣ Skills — 已初始化
- `~/.hermes/skills/` 已接入 Git 版本管理
- 可通过 SkillHub 扩展第三方技能

---

**下一步学习计划：SQL → Dune → DeFi 实战 → BD Dashboard**
