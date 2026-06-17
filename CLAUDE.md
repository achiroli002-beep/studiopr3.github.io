# Studio Àncora — Memoria di progetto

Sito vetrina per il poliambulatorio Studio Àncora di Milano. Pagina singola
autosufficiente: tutto in `index.html` (HTML + CSS + JS + SVG inline), zero
dipendenze esterne, apribile direttamente nel browser.

## Come aprire / testare

```bash
# Opzione 1 — doppio clic su index.html nel Finder
# Opzione 2 — server locale (evita limitazioni CORS sul form)
npx serve .
# poi apri http://localhost:3000
```

## Regola token

Non fare domande su quanto già definito qui. Una modifica per volta. I valori
in questa pagina sono la fonte di verità: qualsiasi modifica futura deve
rispettare i token, le convenzioni e le istruzioni qui sotto.

---

## Design token (`:root` in `index.html`)

| Token | Valore | Uso |
|---|---|---|
| `--bg` | `#FAF8F3` | Sfondo pagina |
| `--surface` | `#FFFFFF` | Card, header, modal |
| `--ink` | `#20302B` | Testo principale |
| `--muted` | `#5E6B65` | Testo secondario, didascalie |
| `--sage` | `#5C8374` | Colore primario, accenti |
| `--sage-soft` | `#E3ECE6` | Sfondi leggeri, sezione servizi |
| `--terracotta` | `#C2603E` | CTA, link azione |
| `--line` | `rgba(32,48,43,.12)` | Bordi sottili |
| `--radius` | `.75rem` | Angoli card |
| `--radius-sm` | `.375rem` | Angoli bottoni, input |
| `--max-w` | `1100px` | Larghezza max container |
| `--header-h` | `64px` | Altezza header sticky |

**Tipografia fluida** (`clamp()`): `--text-sm` → `--text-3xl`.  
**Font titoli**: `Georgia / "Iowan Old Style" / serif` (var `--font-serif`).  
**Font testo**: `system-ui / -apple-system / sans-serif` (var `--font-sans`).  
Blocco `@font-face` commentato per Fraunces + Public Sans self-hosted.

**Regole stilistiche**: niente gradienti, niente ombre pesanti (`box-shadow` max
`0 0 0 3px` per focus ring). Bordi `1px solid var(--line)`. Molto spazio bianco.

---

## Struttura HTML (sezioni con ancore)

```
header.site-header  → nav sticky + CTA Prenota
main
  section#hero       → titolo, CTA, SVG illustrativa (o foto TODO)
  div.trust-strip    → 4 statistiche
  section#equipe     → griglia 6 card professionisti
  section#servizi    → griglia 4 percorsi di cura
  section.cta-band   → fascia prenotazione
  section#contatti   → form + indirizzo + mappa
footer.site-footer  → logo, copyright, Privacy/Cookie modal
```

`scroll-margin-top: var(--header-h)` su `#equipe`, `#servizi`, `#contatti`.

---

## Convenzioni

- **Tutto in `index.html`**: non creare file CSS/JS separati.
- **HTML semantico**: `<section>`, `<article>`, `<address>`, `<nav>`, `<header>`, `<footer>`.
- **WCAG 2.1 AA**: skip link, `aria-label` su nav e modali, `aria-required`,
  `aria-live` sullo stato form, focus ring visibili, `alt` su immagini.
- **`prefers-reduced-motion`**: `[data-reveal]` non anima, `scroll-behavior: auto`.
- **Mobile-first**: breakpoint principali a `560px`, `680px`, `760px`, `900px`.
- **Contenuti in italiano**.
- **Nessuna richiesta esterna** di default (zero CDN, zero font remote).

---

## COME AGGIUNGERE UN PROFESSIONISTA

Trova in `index.html` il commento:
```html
<!-- TEMPLATE CARD: duplica questo blocco per aggiungere un professionista -->
```

Copia l'intero blocco `<article class="team-card">…</article>` e incollalo
dopo l'ultima card nella griglia `<div class="team-grid">`.

**Campi da modificare nella copia:**

| Elemento | Cosa cambiare |
|---|---|
| `<div class="team-specialty">` | Specialità (es. `Oculistica`) |
| `<h3>` | Nome del professionista (es. `Dr.ssa Nome Cognome`) |
| `<p>` | Breve bio (1-2 righe) |
| SVG avatar | Opzionale: sostituire con `<img src="foto.jpg" alt="Nome Cognome, specialità" width="88" height="88" style="object-fit:cover;border-radius:50%;">` |

Aggiorna anche il `<select id="specialita">` nel form contatti aggiungendo
`<option>Oculistica</option>` nell'elenco.

Aggiorna il JSON-LD nell'`<head>`: aggiungi un oggetto `Physician` nell'array
`employee`.

---

## COME MODIFICARE UN SERVIZIO (percorso di cura)

Trova il commento:
```html
<!-- TEMPLATE SERVIZIO: duplica questo blocco per aggiungere un percorso. -->
```

Copia l'intero `<article class="service-card">…</article>`.

**Campi da modificare:**

| Elemento | Cosa cambiare |
|---|---|
| SVG dentro `.service-icon` | Icona (28×28, colori `--sage`) |
| `<h3>` | Nome del percorso |
| `<p>` | Descrizione (2-3 righe) |
| `aria-label` del `.service-icon` | (opzionale) descrizione icona |

---

## TODO da completare prima della pubblicazione

| # | Dove | Cosa |
|---|---|---|
| 1 | `<head>` JSON-LD + meta OG | `og:image`, `og:url`, `telephone`, `streetAddress`, `postalCode`, nomi reali dei professionisti |
| 2 | Header hero `.hero-eyebrow` | Indirizzo reale (es. `Via Torino 12`) |
| 3 | `h1` hero | Eventuale personalizzazione tagline |
| 4 | Hero | Sostituire SVG illustrativa con `<img>` foto team reale (istruzioni nel commento HTML) |
| 5 | Avatar card équipe | Sostituire SVG con foto (`<img>`) per ogni professionista |
| 6 | Nomi e bio équipe | Sostituire tutti i `TODO` con dati reali |
| 7 | `#contatti` `.map-placeholder` | Sostituire con `<iframe>` Google Maps (istruzioni nel commento HTML) |
| 8 | Form `action` / fetch URL | Endpoint Formspree (o altro): `https://formspree.io/f/XXXXXX` |
| 9 | Modal prenota `.booking-area` | Iframe/widget gestionale prenotazioni |
| 10 | Footer / modal Privacy | Completare testo legale con DPO/legale |
| 11 | Modal Cookie | Aggiornare se si aggiungono analytics o widget terze parti |
| 12 | Telefono / email | Tutti i `TODO: +39 02...` e `TODO@studioancora.it` |

---

## Come pubblicare

Il sito è un singolo file statico: **carica `index.html` su qualsiasi hosting**.

```
Opzioni consigliate (piano gratuito disponibile):
- GitHub Pages  → push su branch main + Settings > Pages > Deploy from branch
- Netlify       → trascina la cartella nella UI di Netlify
- Vercel        → `npx vercel` nella cartella
- Hosting tradizionale → upload via FTP/SFTP nella cartella pubblica (public_html)
```

Per dominio custom: configura il CNAME/A record sul tuo provider DNS verso
il servizio scelto.
