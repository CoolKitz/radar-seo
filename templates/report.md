# Audit SEO — {dominio}

**Data:** {AAAA-MM-GG} · **Tipo:** {Audit completo | Singola pagina | Audit mirato: aree} · **Pagine analizzate:** {n}
**Misure browser:** {sì, desktop + mobile | no — Chrome non collegato}

## Punteggio complessivo: {NN}/100 — {fascia}

| Area | Punteggio | Stato | Dati |
|---|---|---|---|
| Tecnico + performance | {NN}/100 | {fascia} | {misurato / parziale} |
| Contenuti + E-E-A-T | {NN}/100 | {fascia} | {misurato / parziale} |
| Schema + sitemap + immagini | {NN}/100 | {fascia} | {misurato / parziale} |
| AI-readiness (GEO) | {NN}/100 | {fascia} | {misurato / parziale} |

{Ometti le righe delle aree non analizzate. Se scatta il tetto di sicurezza, spiegalo qui in una riga.}

## In sintesi

{3-5 frasi: stato generale, il problema più grave, il guadagno più facile, cosa fare per primo.}

## Core Web Vitals (laboratorio)

| Pagina | Dispositivo | LCP | CLS | Long task (rischio INP) | TTFB | Peso |
|---|---|---|---|---|---|---|
| {url} | desktop | {s} | {n} | {count / totale ms} | {ms} | {KB} |

{Nota: misure di laboratorio da una singola visita, non dati di campo. INP stimato dai long task.}
{Ometti la sezione se l'area tecnica non è stata analizzata.}

## Problemi per priorità

### Critical
{finding nel formato:}
- **{Titolo}**
  - Dove: {URL / elemento}
  - Trovato: {valore reale} · Atteso: {valore corretto}
  - Perché conta: {una riga}
  - Fix: {azione concreta}
  - Verifica: {come controllare che è risolto}

### High
### Medium
### Low
### Info

{Sezioni vuote: scrivi "Nessuno".}

## Action plan

### Subito (Critical)
1. {fix} — sblocca: {cosa dipende da questo} — verifica: {controllo}

### Entro 1 settimana (High)
### Entro 1 mese (Medium)
### Backlog (Low)

## Codice pronto

{Snippet JSON-LD, meta tag, regole robots.txt o redirect generati con i dati reali del sito. Segnaposto [DA COMPLETARE] per i dati non visibili.}

## Limiti di questo audit

- {Verifiche non eseguite e perché (Chrome assente, pagine bloccate, timeout).}
- Analisi basata su un campione di {n} pagine, non sull'intero sito.
- Nessun dato di campo: per traffico reale, query e CWV di campo servono Google Search Console e CrUX.
