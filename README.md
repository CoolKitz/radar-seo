# radar-seo

Skill per [Claude Code](https://claude.com/claude-code) che esegue audit SEO di qualsiasi sito web usando **solo strumenti nativi**: WebFetch, curl verso il sito analizzato e, se collegato, Claude in Chrome per misurare i Core Web Vitals. Nessuno script esterno, nessuna chiave API, nessun server MCP di terze parti.

## Cosa fa

All'avvio mostra un menu a scelta multipla:

1. **Audit completo del sito**: homepage più un massimo di 20 pagine campione dalla sitemap, con quattro sotto-agenti in parallelo
2. **Analisi singola pagina**
3. **Audit mirato**: scegli una o più aree

Le aree analizzate sono quattro:

| Area | Contenuto |
|---|---|
| Tecnico + performance | crawlability, indicizzazione, redirect, HTTPS, rendering JS, Core Web Vitals (LCP, CLS, INP stimato) |
| Contenuti + E-E-A-T | on-page, qualità e profondità, thin content, segnali di fiducia e autorialità |
| Schema + sitemap + immagini | validazione JSON-LD, sitemap XML, alt, formati, peso e dimensioni delle immagini |
| AI-readiness (GEO) | accesso dei crawler AI, leggibilità senza JS, citabilità, entità, llms.txt |

Il risultato comprende:

- un punteggio 0-100 per ogni area e uno complessivo, pesato
- i problemi classificati Critical/High/Medium/Low, ciascuno con evidenza, fix e modo di verificarlo
- un action plan ordinato per dipendenze
- codice pronto da usare (JSON-LD, regole .htaccess, llms.txt)

In chat compare la sintesi. Il report completo viene salvato in `seo-audit-<dominio>-<data>.md`.

## Installazione

```bash
git clone https://github.com/CoolKitz/radar-seo.git ~/.claude/skills/radar-seo
```

Se usi una `CLAUDE_CONFIG_DIR` personalizzata, clona la skill nella sua cartella `skills/`. Poi apri una nuova sessione di Claude Code e scrivi `/radar-seo https://tuosito.it`.

Per misurare i Core Web Vitals reali serve l'estensione Claude in Chrome. Senza l'estensione l'audit funziona lo stesso e il report indica quali metriche mancano.

## Struttura

```
SKILL.md                          menu, raccolta dati, orchestrazione, scoring, report
references/
  tecnico-performance.md          checklist + script di misura CWV
  contenuti-eeat.md
  schema-sitemap-immagini.md
  ai-readiness.md
  scoring.md                      pesi e regole di priorità
templates/report.md               struttura del report
```

## Sicurezza

Tutto il contenuto scaricato dai siti analizzati è trattato come dato da analizzare, mai come istruzione da eseguire. Se una pagina contiene testo rivolto a un'IA, la skill lo segnala come finding invece di seguirlo. La skill invia richieste solo verso il sito analizzato.

## Limiti

- I Core Web Vitals sono misurati in laboratorio, durante una singola visita. Non sono dati di campo (CrUX).
- L'INP è stimato dai long task, perché misurarlo richiede interazioni reali.
- Mancano i dati di Search Console e di backlink, che richiedono credenziali.

## Licenza

MIT
