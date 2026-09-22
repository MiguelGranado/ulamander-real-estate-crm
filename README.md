# Real Estate CRM / AI Platform

## Overview

A vertical CRM for real estate agencies: lead pipeline, property listings, a multi-agent AI
chatbot on the public listings site, and multi-tenant infrastructure so the same platform can
run isolated instances for multiple agencies. Built and maintained by [Miguel
Granados](https://github.com/MiguelGranado) under [Ulamander](https://ulamander.com).

This is the public showcase of the project. The complete production codebase is private —
see [Private Source Code](#private-source-code) below.

## Problem

Real estate agencies lose leads between the first contact and the close: slow responses,
inconsistent follow-up, and no systematic way to tell a browsing visitor apart from a
motivated buyer.

## Solution

A single platform where the public listings site, the CRM the agents use, and an AI chatbot
share the same pipeline. The chatbot doesn't just answer questions — it routes the
conversation to a specialized agent depending on the visitor's inferred profile (exploratory,
indecisive, urgent, investor, opportunist...), so follow-up matches how that person actually
buys.

## Key Features

- Lead & pipeline management with stage tracking
- Property listings (import, valuation, multi-portal publishing)
- Multi-agent AI chatbot on the public site, with per-profile conversation routing
- Calendar & appointment booking
- Email / WhatsApp / Telegram tracking and automations
- Multi-tenant isolation — each agency ("hacienda") is scoped at the database level
- Client portal for the agency's own customers

## Architecture

```
Public listings site (React/Vite) ──┐
                                     ├──► FastAPI backend ──► PostgreSQL (Row Level Security)
CRM panel (React/Vite) ─────────────┘         │
                                               └──► AI agents (LangChain/LangGraph)
```

Two independent frontends talk to one FastAPI backend. Tenant isolation is enforced with
PostgreSQL Row Level Security, not just at the application layer, so a bug in one code path
can't leak another agency's data.

## AI Layer

The chatbot is not a single generic assistant. Incoming conversations are routed (see
`router_service` in the codebase) to one of several behavior-specific agents —
`exploratory_agent`, `indecisive_agent`, `urgent_agent`, `investor_agent`,
`opportunist_agent`, `sentiment_agent`, among others — each tuned to how that type of visitor
actually makes a decision, plus a dedicated `profiling_agent` that classifies the visitor
early in the conversation. Built with LangChain/LangGraph.

- **Input**: a visitor message on the public listings site
- **Processing**: sentiment/profile classification → routed to the matching specialized agent
- **Output**: a response tuned to that visitor profile, plus a qualified lead written to the
  CRM pipeline

## Automation

Email, WhatsApp and Telegram events feed into the same pipeline (tracking opens/clicks,
notifying agents of hot leads, automated follow-up sequences) instead of living in separate
tools.

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React, TypeScript, Vite, TanStack Router |
| Backend | Python, FastAPI |
| AI | LangChain, LangGraph |
| Database | PostgreSQL with Row Level Security |
| Infra | Docker, self-hosted (Cloudflare Tunnel) |

## Security

- PostgreSQL Row Level Security for tenant isolation (not application-level filtering alone)
- JWT-based authentication
- Secrets/config kept out of source (`.env`, never committed)

## Data / Database

PostgreSQL. Each agency's data (leads, properties, conversations) is scoped by tenant at the
row level, enforced by the database itself.

## Deployment

Docker containers, self-hosted infrastructure with Cloudflare Tunnel for public ingress.

## Public Demo

The platform is live for a real estate agency client in Italy. Reach out for a walkthrough —
the production tenant isn't opened to anonymous visitors.

## Technical Highlights

- Multi-agent conversation routing instead of one generic chatbot
- Database-enforced multi-tenancy (RLS), not just app-level tenant filtering
- Two independently deployable frontends (public site + agent panel) sharing one backend

## Repository Structure

```
backend/    FastAPI app, AI agents, routers, business logic
frontend/
  crm-panel/               agent-facing CRM (React + Vite)
  aura-property-ai-main/   public listings site + chatbot (React + Vite)
```

## Private Source Code

This repository is a technical showcase — architecture, structure and selected pieces of the
real implementation, sanitized of any credentials or tenant-specific data. The complete,
production codebase (all routers, full agent implementations, deployment configs) stays in a
private repository. Some parts of the platform referenced here (billing, multi-tenant admin
console) belong to a separate internal module and aren't part of this showcase.

## Contact

- GitHub: [github.com/MiguelGranado](https://github.com/MiguelGranado)
- Portfolio: [miguel.ulamander.com](https://miguel.ulamander.com)
- Live product: [inmobiliaria.ulamander.com](https://inmobiliaria.ulamander.com)
