# PRD — Agile Retro App

## 1. Contesto e problema

I team di sviluppo non fanno la retrospettiva con continuità. Il problema principale non è la mancanza di uno strumento, ma che:

- La retro viene percepita come una perdita di tempo
- Gli action item vengono scritti e poi abbandonati senza follow-up
- Non esiste un meccanismo che crei accountability tra una sessione e l'altra

**Obiettivo:** costruire un'app che faciliti la retro in sessione e garantisca continuità nel tempo, trasformando gli action item in impegni reali.

**Utenti target (primo cliente):**
- Sonia — responsabile funzionale
- Massimo — responsabile tecnico
- Team remoto, 6–10 partecipanti per sessione

---

## 2. Ruoli e permessi

| Ruolo | Capacità |
|---|---|
| **Admin** | Crea e gestisce progetti, invita collaboratori, assegna ruoli |
| **Facilitatore** | Crea retro board, gestisce le fasi della sessione |
| **Partecipante** | Aggiunge card, vota |

### Invito collaboratori
- L'admin invita via **link con scadenza temporale**
- I collaboratori accedono tramite il link e vengono associati al progetto con il ruolo assegnato

---

## 3. Struttura dati

### Gerarchia
```
Progetto
└── Board Retro (N per progetto)
    ├── Categorie (configurabili per board)
    └── Card
        └── Action Item (opzionale, da card selezionata)
```

### Progetto
- Nome, descrizione
- Membri con ruoli
- Cadenza retro configurabile (es. ogni 2 settimane)
- Configurazione notifiche (chi riceve il summary)

### Board Retro
- Titolo, data
- Stato: Draft / In corso / Completata
- Categorie configurabili (default: **Cosa è andato bene / Cosa è andato male / Come possiamo migliorare**)
- Template pre-compilato dalla sessione precedente

### Card
- Testo
- Owner (anonimo durante la raccolta, visibile dopo il reveal)
- Categoria
- Tematica/tag (per raggruppamento visivo)
- Voti (like)

### Action Item
- Testo
- Owner (qualsiasi membro del progetto, anche non presente)
- Stato: **Todo / In Progress / Done**
- Scadenza: default = prossima retro schedulata, override manuale

---

## 4. Flusso della sessione live

La board ha 4 fasi, gestite dal **Facilitatore**. Il passaggio tra fasi è manuale e controllato.

### Fase 1 — Raccolta (card anonime)
- Ogni partecipante scrive le proprie card
- Le card degli altri sono **nascoste** (no contatore)
- Il sistema **blocca il passaggio alla fase 2** finché ogni partecipante non ha scritto almeno 1 card
- Il facilitatore vede privatamente chi non ha ancora contribuito

### Fase 2 — Reveal
- Tutte le card diventano visibili con owner
- Il facilitatore può raggruppare card per tematica/tag

### Fase 3 — Votazione
- Ogni partecipante può mettere **like** alle card
- Nessun limite di voti

### Fase 4 — Discussion & Action Items
- Il facilitatore guida la discussione partendo dalle card più votate
- Per ogni card rilevante si possono creare **action item** con owner e scadenza
- Filtri disponibili: per **owner**, **tematica/tag**, **stato action item**

---

## 5. Continuità — funzionalità core

Queste 5 funzionalità attaccano il problema principale (retro percepita come inutile):

1. **Action item tracciati tra retro** — gli action item aperti della retro precedente compaiono automaticamente nella nuova board, con stato aggiornato
2. **Retro health score** — indicatore visivo per progetto che mostra la regolarità delle retro (es. "ultima retro: 3 settimane fa")
3. **Template pre-compilato** — la nuova board parte con le categorie della retro precedente e gli action item ancora aperti
4. **Timer in-meeting** — timer visibile a tutti durante la sessione, configurabile per fase, gestito dal facilitatore
5. **Summary post-retro via email** — generato automaticamente a fine sessione con: lista card più votate, action item, owner, scadenze. Destinatari configurabili per progetto (default: tutti i partecipanti + owner degli action item)

---

## 6. Notifiche e reminder

- **Reminder cadenza**: notifica al facilitatore quando si avvicina la data della prossima retro schedulata
- **Summary post-retro**: email automatica a fine sessione (configurabile per progetto)
- **Action item in scadenza**: notifica all'owner prima della scadenza

---

## 7. Login (MVP)

Per il prototipo iniziale: **pagina login statica** con form email/password, nessuna logica auth reale. Qualsiasi credenziale consente l'accesso.

---

## 8. Tech stack

| Layer | Tecnologia |
|---|---|
| Frontend | Next.js 16 + React 19 + TypeScript |
| UI | shadcn/ui + Tailwind CSS 4 |
| API | tRPC + Hono |
| Database | PostgreSQL (Neon) + Drizzle ORM |
| Auth | Better Auth (mock per MVP) |
| Email | Resend + React Email |
| Real-time | Ably o Pusher |
| Logging | Pino + OpenTelemetry |
| Testing | Bun test + Testing Library |

---

## 9. Fuori scope (MVP)

- Integrazione con Jira / Confluence / Notion
- Autenticazione reale (Better Auth completo)
- App mobile
- Export PDF/CSV della board
