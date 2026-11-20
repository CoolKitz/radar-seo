# Schema + sitemap + immagini

## 1. Dati strutturati (Schema.org)

Estrai tutti i blocchi `<script type="application/ld+json">` (anche microdata/RDFa se presenti, ma raccomanda JSON-LD).

**Validazione:**
- JSON sintatticamente valido (prova a farne il parse: un errore = blocco ignorato da Google) → High
- `@context` = `https://schema.org`, `@type` esistente
- Proprietà obbligatorie e raccomandate presenti per il tipo
- Coerenza con il contenuto visibile: dati nello schema che non compaiono in pagina (prezzi, recensioni, valutazioni inventate) → High (rischio azione manuale)
- URL assoluti; date ISO 8601

**Tipi attesi per tipo di pagina:**

| Pagina | Schema consigliato |
|---|---|
| Homepage | `Organization` (o `LocalBusiness`) con logo, url, sameAs; `WebSite` |
| Attività locale | `LocalBusiness` o sottotipo specifico, con indirizzo, orari, telefono, geo |
| Articolo | `Article` / `BlogPosting` con author (Person), datePublished, dateModified, image |
| Prodotto | `Product` con `offers` (price, priceCurrency, availability), `aggregateRating` solo se reale |
| Navigazione | `BreadcrumbList` |
| Video | `VideoObject` |
| Eventi | `Event` |

Assenza dello schema principale per il tipo di pagina → Medium (High per `Product` su e-commerce e `LocalBusiness` su attività locali).

**Regole fisse:**
- **Non raccomandare `HowTo`**: rich result ritirato. Se presente, Info.
- **Non raccomandare `FAQPage` per ottenere rich result**: non più mostrati per i siti normali. Se presente, segnala come Info senza chiederne la rimozione. Per domande e risposte degli utenti usa `QAPage`.
- Per ogni schema mancante fornisci un esempio JSON-LD pronto, compilato con i dati reali trovati in pagina (lascia segnaposto `[DA COMPLETARE]` per ciò che non è visibile; non inventare).

## 2. Sitemap XML

| Controllo | Priorità se fallisce |
|---|---|
| Sitemap esistente e raggiungibile | High (Medium per siti < 20 pagine) |
| Dichiarata in robots.txt | Low |
| XML valido, namespace corretto | High |
| Max 50.000 URL / 50 MB per file, altrimenti sitemap index | High |
| URL assoluti, stesso protocollo e host canonico | Medium |
| Nel campione: URL che restituiscono 3xx/4xx/5xx | High se > 10% del campione, Medium altrimenti |
| URL `noindex` o con canonical verso altro | Medium |
| `lastmod` presente e plausibile (non tutte con la data di oggi) | Low |
| Pagine importanti del menu assenti dalla sitemap | Medium |

`priority` e `changefreq` sono ignorati da Google: non segnalarne l'assenza.

Pagine locali/città generate in serie: oltre 30 → avviso Medium (verifica ≥ 60% di contenuto unico); oltre 50 → High, rischio doorway pages.

## 3. Immagini

Per ogni pagina analizzata:

| Controllo | Priorità se fallisce |
|---|---|
| `alt` presente sulle immagini di contenuto (decorative: `alt=""`) | Medium (High se > 50% delle immagini di contenuto ne è priva) |
| `alt` descrittivo, non nome file né keyword stuffing | Low |
| Formato moderno (WebP/AVIF) per foto | Medium |
| Peso: foto > 300 KB, hero > 500 KB | Medium (High se è l'immagine LCP) |
| `width`/`height` o aspect-ratio dichiarati | Medium (causa CLS) |
| `loading="lazy"` sotto la piega | Low |
| **Nessun** `loading="lazy"` sull'immagine LCP/hero | High |
| `srcset`/`sizes` per immagini responsive | Low |
| Nome file descrittivo (`scarpe-trail-uomo.webp` vs `IMG_2034.jpg`) | Low |

Per il peso usa la lista risorse più pesanti misurata in Chrome se disponibile; altrimenti `curl -sI <url-immagine>` per `content-length` (max 10 immagini).

## Punteggio d'area

Calcolalo con le regole di `scoring.md`.
