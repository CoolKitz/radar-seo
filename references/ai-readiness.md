# AI-readiness (GEO)

Obiettivo: il sito può essere letto, compreso e **citato** da motori con risposte generative (Google AI Overviews / AI Mode, ChatGPT search, Perplexity, Copilot, Gemini, Claude).

## 1. Accesso dei crawler AI (robots.txt)

Verifica le regole per questi user-agent:

| Crawler | Di chi | Uso |
|---|---|---|
| `Googlebot` | Google | Indicizzazione **e** AI Overviews (bloccarlo blocca tutto) |
| `Google-Extended` | Google | Solo addestramento/grounding Gemini; non influisce sul ranking Search |
| `Bingbot` | Microsoft | Indice usato da Copilot e da molti motori AI |
| `OAI-SearchBot` | OpenAI | Risultati di ChatGPT search |
| `ChatGPT-User` | OpenAI | Visite su richiesta di un utente |
| `GPTBot` | OpenAI | Addestramento |
| `PerplexityBot` | Perplexity | Indice di ricerca |
| `ClaudeBot` / `Claude-SearchBot` / `Claude-User` | Anthropic | Addestramento / ricerca / visite utente |
| `Applebot-Extended` | Apple | Addestramento |
| `CCBot` | Common Crawl | Dataset usati per addestramento |

Distingui **ricerca** da **addestramento**: bloccare i bot di addestramento è una scelta legittima; bloccare quelli di ricerca (`OAI-SearchBot`, `PerplexityBot`, `Claude-SearchBot`, `Bingbot`) toglie visibilità nelle risposte AI → High se il sito vuole quella visibilità. Non dare per scontato cosa voglia l'utente: riporta lo stato e le conseguenze.

Blocchi a livello WAF/CDN (es. 403 verso user-agent bot) non sono visibili da robots.txt: se curl con user-agent di un bot riceve 403 mentre quello browser riceve 200, segnalalo.

## 2. Leggibilità senza JavaScript

Molti crawler AI **non eseguono JavaScript**. Se il contenuto principale è visibile solo dopo il rendering (confronto WebFetch vs Chrome dell'area tecnica, o assenza di testo nell'HTML iniziale) → High.

## 3. Citabilità dei contenuti

Un passaggio è citabile quando risponde da solo a una domanda.

- Risposta diretta nelle prime 1-2 frasi sotto un heading formulato come domanda o tema chiaro → assenza diffusa: Medium.
- Paragrafi autosufficienti (40-150 parole circa), senza riferimenti tipo "come detto sopra".
- Dati concreti: numeri, date, nomi, definizioni, con fonte → Medium se il contenuto è solo generico.
- Tabelle ed elenchi per confronti, specifiche, passaggi.
- Data di aggiornamento visibile su contenuti informativi → Low.

## 4. Chiarezza delle entità

- Nome del brand/organizzazione coerente in title, contenuto, schema e footer.
- `Organization`/`LocalBusiness` con `sameAs` verso profili ufficiali (LinkedIn, Wikipedia/Wikidata se esistenti, social, Google Business Profile) → Medium se assente.
- Pagina Chi siamo che dice chiaramente chi siete, cosa fate, dove operate.
- Autori come entità (`Person` con `sameAs`) sui contenuti informativi.

## 5. llms.txt e file di scoperta

- `llms.txt` è **opzionale**: Google dichiara di non usarlo. Assenza → Low (mai più grave). Se presente, verifica che sia markdown valido, con titolo, descrizione e link a pagine esistenti.
- Non promettere benefici di ranking da llms.txt.

## 6. Snippet e controlli di anteprima

`nosnippet`, `max-snippet:0` o `data-nosnippet` estesi impediscono di usare il testo nelle risposte AI di Google → High se involontari.

## Punteggio d'area

Calcolalo con le regole di `scoring.md`.
