# Ulamander Real Estate CRM

**A vertical, AI-native CRM for real estate agencies — live in production for a real client.**

[![Backend](https://img.shields.io/badge/backend-FastAPI-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Frontend](https://img.shields.io/badge/frontend-React%20%2B%20Vite-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![Language](https://img.shields.io/badge/language-TypeScript%20%2F%20Python-3178C6?logo=typescript&logoColor=white)](#technology-stack)
[![Database](https://img.shields.io/badge/database-PostgreSQL%20%2B%20RLS-4169E1?logo=postgresql&logoColor=white)](#security--multi-tenancy)
[![AI](https://img.shields.io/badge/AI-LangChain%20%2F%20LangGraph-1C3C3C)](#ai-layer)
[![Status](https://img.shields.io/badge/status-live%20in%20production-2E7D32)](#public-demo)

Built and maintained by [Miguel Granados](https://github.com/MiguelGranado), founder of
[Ulamander](https://ulamander.com).

This is the **public showcase** of the project — architecture, structure and selected code,
sanitized of credentials and tenant data. The full production codebase is private; see
[Private Source Code](#private-source-code).

---

## Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [AI Layer — Multi-Agent Routing](#ai-layer--multi-agent-routing)
- [Automation](#automation)
- [Technology Stack](#technology-stack)
- [Security & Multi-Tenancy](#security--multi-tenancy)
- [Repository Structure](#repository-structure)
- [Public Demo](#public-demo)
- [Private Source Code](#private-source-code)
- [Contact](#contact)

---

## Overview

A CRM built specifically for real estate agencies: lead pipeline, property listings, a
multi-agent AI chatbot on the public site, and multi-tenant infrastructure so the same
platform runs isolated instances for multiple agencies from one codebase.

## The Problem

Real estate agencies lose leads in the gap between first contact and close: slow responses,
inconsistent follow-up, and no systematic way to tell a browsing visitor apart from someone
ready to sell or buy.

## The Solution

One platform where the public listings site, the CRM the agents use, and an AI chatbot share
the same pipeline. The chatbot doesn't just answer questions — it routes each conversation to
a specialized agent based on the visitor's inferred profile (exploratory, indecisive, urgent,
investor, opportunist...), so the follow-up actually matches how that person makes decisions.

## Key Features

| Category | What it does |
|---|---|
| **Lead & Pipeline** | Stage-tracked pipeline, from first contact to closed deal |
| **Property Listings** | Import, valuation, multi-portal publishing |
| **AI Chatbot** | Multi-agent, profile-aware conversation on the public site |
| **Calendar** | Appointment booking synced with agent availability |
| **Automations** | Email / WhatsApp / Telegram tracking and follow-up sequences |
| **Multi-Tenant** | Each agency isolated at the database row level |
| **Client Portal** | Self-service portal for the agency's own customers |

## Architecture

```
┌─────────────────────────────┐
│  Public listings site        │  React + Vite
│  (chatbot entry point)       │
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐        ┌───────────────────────┐
│        FastAPI backend        │◄──────►│   AI agents            │
│   (API, auth, business logic) │        │   LangChain / LangGraph│
└──────────────┬───────────────┘        └───────────────────────┘
               │
┌──────────────▼───────────────┐
│  PostgreSQL                   │
│  Row Level Security per tenant│
└────────────────────────────────┘
               ▲
┌──────────────┴───────────────┐
│  CRM panel (agents)           │  React + Vite
└────────────────────────────────┘
```

Two independently deployable frontends — the public listings site and the agent-facing CRM
panel — talk to one FastAPI backend. Tenant isolation is enforced with PostgreSQL Row Level
Security, not application-level filtering alone, so a bug in one code path can't leak another
agency's data.

## AI Layer — Multi-Agent Routing

The chatbot isn't a single generic assistant. Every incoming conversation is classified by a
`profiling_agent` and routed (via `router_service`) to one of several behavior-specific
agents — `exploratory_agent`, `indecisive_agent`, `urgent_agent`, `investor_agent`,
`opportunist_agent`, `sentiment_agent`, among others — each tuned to how that type of visitor
actually makes a decision.

```
visitor message → profiling_agent → router_service → specialized agent → qualified lead
```

- **Input**: a visitor message on the public listings site
- **Processing**: sentiment/profile classification → routed to the matching agent
- **Output**: a response tuned to that visitor's profile, plus a qualified lead written
  straight into the CRM pipeline

Built with LangChain/LangGraph.

## Automation

Email, WhatsApp and Telegram events feed into the same pipeline — tracking opens and clicks,
notifying agents of hot leads, running automated follow-up sequences — instead of living in
separate, disconnected tools.

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React, TypeScript, Vite, TanStack Router |
| Backend | Python, FastAPI |
| AI | LangChain, LangGraph |
| Database | PostgreSQL with Row Level Security |
| Infra | Docker, self-hosted (Cloudflare Tunnel) |

## Security & Multi-Tenancy

- **Database-enforced isolation** — PostgreSQL Row Level Security, not just app-level tenant
  filtering
- **Authentication** — JWT-based
- **Secrets** — kept entirely out of source control (`.env`, never committed)

## Repository Structure

```
backend/    FastAPI app — AI agents, routers, business logic
frontend/
  crm-panel/               agent-facing CRM (React + Vite)
  aura-property-ai-main/   public listings site + chatbot (React + Vite)
```

## Public Demo

The platform is live for a real estate agency client in Italy. The production tenant isn't
open to anonymous visitors — reach out for a walkthrough.

## Private Source Code

This repository is a technical showcase: architecture, structure and selected pieces of the
real implementation. The complete production codebase — all routers, full agent
implementations, deployment configs — stays in a private repository. Some parts of the
platform referenced here (billing, multi-tenant admin console) belong to a separate internal
module and aren't part of this showcase.

## Contact

- **GitHub** — [github.com/MiguelGranado](https://github.com/MiguelGranado)
- **Portfolio** — [miguel.ulamander.com](https://miguel.ulamander.com)
- **Live product** — [inmobiliaria.ulamander.com](https://inmobiliaria.ulamander.com)

---

**Miguel Granados** — Founder, [Ulamander](https://ulamander.com)
