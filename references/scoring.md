# Scoring e priorità

## Priorità

| Livello | Significato | Tempi |
|---|---|---|
| **Critical** | Blocca indicizzazione o espone a penalizzazioni | Subito |
| **High** | Incide in modo significativo su ranking o visibilità | Entro 1 settimana |
| **Medium** | Opportunità di ottimizzazione concreta | Entro 1 mese |
| **Low** | Rifinitura | Backlog |
| **Info** | Da sapere, nessuna azione necessaria | — |

## Punteggio d'area (0-100)

1. Parti da 100.
2. Sottrai per ogni problema distinto: Critical −25 · High −10 · Medium −4 · Low −1 · Info 0.
3. Lo stesso problema su più pagine conta **una volta**, con l'elenco degli URL coinvolti. Se riguarda più del 50% del campione, alza la priorità di un livello (max Critical).
4. Minimo 0.

Fasce: **90-100** ottimo · **75-89** buono · **50-74** da migliorare · **< 50** critico.

## Punteggio complessivo

Pesi base:

| Area | Peso |
|---|---|
| Tecnico + performance | 30 |
| Contenuti + E-E-A-T | 30 |
| Schema + sitemap + immagini | 20 |
| AI-readiness (GEO) | 20 |

Se sono state analizzate solo alcune aree, ricalcola i pesi proporzionalmente sulle aree presenti (es. solo Tecnico + Contenuti → 50/50).

`complessivo = Σ (punteggio_area × peso_area) / Σ pesi_aree_analizzate`, arrotondato all'intero.

**Tetto di sicurezza:** se esiste almeno un Critical che blocca l'indicizzazione (noindex involontario, robots che blocca il sito, errori 5xx sulla homepage), il punteggio complessivo non può superare **40**, qualunque sia il resto.

## Misurato vs stimato

Per ogni area indica se il punteggio si basa su dati **misurati** (raccolti davvero) o in parte **stimati** (es. INP dedotto dai long task, Chrome non disponibile). Un'area con verifiche mancanti va marcata "parziale" nel report.
