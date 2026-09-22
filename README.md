# Real Estate CRM — Piattaforma AI per il settore immobiliare

> Repository vetrina. Mostra l'architettura e le funzionalità del progetto; il codice
> sorgente completo è privato. Per una demo dal vivo o l'accesso al repository completo,
> [contattami](#contatti).

## Il progetto

Piattaforma CRM verticale per agenzie immobiliari, sviluppata da zero: gestione lead e
pipeline di vendita, schede immobili, agenda/appuntamenti, un chatbot AI che qualifica i
contatti in tempo reale sul sito pubblico degli annunci, tracking multicanale (email,
WhatsApp, Telegram) e un livello enterprise multi-tenant che permette di attivare la stessa
piattaforma per più agenzie clienti in modo isolato.

Il sistema è in produzione per clienti reali del settore immobiliare in Italia.

## Architettura

```
┌─────────────────────────┐      ┌──────────────────────────┐
│  Sito pubblico annunci   │      │   Pannello CRM (agenti)   │
│  React + Vite            │      │   React + Vite            │
│  chatbot AI integrato    │      │   pipeline, agenda, task   │
└────────────┬──────────────┘      └────────────┬───────────────┘
             │                                    │
             └───────────────┬────────────────────┘
                              ▼
                 ┌─────────────────────────┐
                 │   Backend FastAPI         │
                 │   agenti AI (LangGraph)    │
                 │   routing multi-tenant     │
                 └────────────┬──────────────┘
                              ▼
                 ┌─────────────────────────┐
                 │   PostgreSQL (RLS)         │
                 └─────────────────────────┘
```

## Funzionalità principali

- **Gestione lead e pipeline** — dalla prima richiesta alla chiusura, con scoring automatico
- **Chatbot AI multi-agente** — instrada la conversazione a "personalità" specializzate
  (esplorativo, indeciso, urgente, investitore...) per qualificare il contatto
- **Schede immobili** — importazione, valutazione, pubblicazione multi-portale
- **Agenda e appuntamenti** — sincronizzazione calendario, promemoria automatici
- **Tracking multicanale** — email (apertura/click), WhatsApp, Telegram, con automazioni
- **Multi-tenant enterprise** — ogni agenzia cliente ("hacienda") isolata a livello di
  dominio, branding e dati (Row Level Security su PostgreSQL)
- **Portale clienti** — area riservata per i clienti finali dell'agenzia

## Stack tecnico

| Livello | Tecnologie |
|---|---|
| Frontend | React, TypeScript, Vite, TanStack Router, shadcn/ui, Tailwind |
| Backend | Python, FastAPI, LangChain / LangGraph |
| Database | PostgreSQL con Row Level Security |
| Integrazioni | Telegram Bot API, WhatsApp Business API, SMTP/IMAP, Google OAuth |
| Infrastruttura | Docker, deploy self-hosted con Cloudflare Tunnel |

## Cosa NON è in questo repository

Questo repository pubblico contiene solo la descrizione del progetto, non il codice. Il
codice sorgente (backend + frontend completi) è mantenuto in un repository privato.

## Contatti

Sviluppato e mantenuto da **Miguel Granados** — Ulamander Corporation.

- Sito: [ulamander.com](https://ulamander.com)
- GitHub: [@MiguelGranado](https://github.com/MiguelGranado)

Se vuoi una demo dal vivo, discutere una collaborazione o richiedere l'accesso al codice
completo, scrivimi.
