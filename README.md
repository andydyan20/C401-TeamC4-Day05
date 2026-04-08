# VinUni Onboarding Agent

Lab 5 submission for "AI Thuc Chien - Ngay 5", focused on designing an AI product that handles uncertainty safely.

## Team

Team name: `C4`

- Dang Tien Dung - `2A202600024`
- Pham Huu Hoang Hiep - `2A202600415`
- Pham Viet Cuong - `2A202600420`
- Pham Tran Thanh Lam - `2A202600270`

## Project Overview

`VinUni Onboarding Agent` is an augmentation-first AI assistant for:

- New students (`Student`)
- New employees (`Staff`)
- Support units (Student Affairs / HR)

The product goal is to reduce onboarding confusion caused by fragmented information (emails, handbook, portal, internal forms), while keeping humans in control for high-stakes decisions.

## Problem We Are Solving

Onboarding users at VinUni often:

- Ask repetitive questions across multiple channels
- Miss deadlines or required documents
- Receive inconsistent answers from different sources

This project proposes an AI agent that provides:

- Context-aware Q&A with citations (RAG-based)
- Personalized onboarding checklist by role and timeline
- Safe fallback actions: `Bao sai` (report incorrect), `Chuyen bo phan` (handoff/escalation)

## Why Augmentation (Not Full Automation)

The system is intentionally designed as augmentation:

- AI suggests next steps and references official sources
- Users or responsible departments make final decisions
- Confidence and citations help users verify responses

This reduces risk when model outputs are uncertain or source data is outdated.

## Repository Structure

This repository currently contains planning and product-design artifacts for the MVP:

- `nhomC4-roomC401/spec-vinuni-onboarding.md`  
  Product/problem specification (value, trust, feasibility, learning signal, ownership)
- `nhomC4-roomC401/implementation-plan.md`  
  Detailed implementation roadmap, architecture, sprint plan, risk controls
- `nhomC4-roomC401/team-deliverables.md`  
  Team deliverables, role-based responsibilities, evaluation and demo plan

## Proposed MVP Scope

- Chat assistant for `Student` and `Staff` onboarding intents
- Setup questions at session start (role, unit/faculty, term, key context)
- RAG answer generation with citations
- "Next step" guidance (form link, portal path, or contact)
- Feedback capture (`helpful/not helpful`) and escalation path

## Suggested Tech Stack

- Frontend: Next.js + TypeScript + Tailwind CSS
- Backend: FastAPI (Python)
- LLM + embeddings: OpenAI API
- Vector store: Chroma (local dev), upgradable later
- Relational DB: PostgreSQL + SQLAlchemy
- Observability: basic logging/tracing + dashboarding

## Success Metrics (v1)

- Grounded answer rate (with valid citations): `>= 90%`
- Checklist task success proxy: `>= 80%`
- Median response latency: `<= 5s`
- Escalation accuracy (to correct department): `>= 85%`

## How To Use This Repo (Current State)

This repo is currently documentation-first. To review the full submission:

1. Read the spec in `nhomC4-roomC401/spec-vinuni-onboarding.md`
2. Review implementation details in `nhomC4-roomC401/implementation-plan.md`
3. Check ownership and deliverables in `nhomC4-roomC401/team-deliverables.md`

## Notes

- This README reflects the project planning and MVP proposal stage.
- Data source curation (official policy/FAQ/forms/deadlines) is expected to be iteratively added.
