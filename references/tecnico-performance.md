# Tecnico + performance

## 1. Crawlability e indicizzazione

| Controllo | Come | Priorità se fallisce |
|---|---|---|
| Status HTTP pagina | header da curl: deve essere 200 | Critical (4xx/5xx su pagine importanti) |
| `noindex` involontario | meta robots + header `x-robots-tag` | Critical |
| robots.txt blocca sezioni importanti | `Disallow` su `/`, cartelle di contenuto, CSS/JS | Critical (se blocca contenuti), High (se blocca CSS/JS) |
| Canonical | presente, assoluto, self-referenziale o coerente, punta a 200 indicizzabile | High (assente/errato), Critical (canonical tutto verso homepage) |
| Coerenza canonical ↔ sitemap ↔ link interni | gli URL in sitemap devono essere quelli canonici | Medium |
| Pagine orfane nel campione | pagina in sitemap non linkata dal menu/interni | Medium |
| Soft 404 | pagina 200 con contenuto "non trovato" | High |

## 2. Redirect e URL

- Una sola versione canonica: http→https, www↔non-www con **un solo salto 301**. Catene > 1 salto: Medium; loop: Critical; 302 per redirect permanenti: Medium.
- URL leggibili, minuscole, trattini, senza parametri di sessione: Low/Medium.
- Trailing slash coerente: Low.

## 3. Sicurezza e protocollo

- HTTPS valido su tutte le pagine: Critical se assente.
- Mixed content (risorse http in pagina https): High.
- HSTS presente: Low.
- Header utili: `content-type` corretto con charset; compressione (gzip/br) attiva: Medium se assente.

## 4. Mobile e rendering

- `<meta name="viewport" content="width=device-width, initial-scale=1">`: High se assente.
- Contenuto principale presente nell'HTML iniziale (WebFetch) vs solo dopo rendering JS (Chrome): se title, H1, testo principale o link compaiono solo con JS → High (indicizzazione ritardata/incerta, crawler AI che non eseguono JS non li vedono).
- `lang` sull'elemento html: Medium.

## 5. Performance — Core Web Vitals

Soglie ufficiali (75° percentile):

| Metrica | Buono | Da migliorare | Scarso |
|---|---|---|---|
| LCP | ≤ 2,5 s | ≤ 4,0 s | > 4,0 s |
| INP | ≤ 200 ms | ≤ 500 ms | > 500 ms |
| CLS | ≤ 0,1 | ≤ 0,25 | > 0,25 |

Usa **INP**, mai FID. Priorità: Scarso → High; Da migliorare → Medium.

### Misura con Chrome

Se gli strumenti `mcp__claude-in-chrome__*` sono disponibili (caricali con ToolSearch in un'unica chiamata: `tabs_context_mcp, tabs_create_mcp, navigate, javascript_tool, resize_window, computer, tabs_close_mcp`):

1. `tabs_context_mcp`, poi apri una **nuova** scheda con `tabs_create_mcp` (non riusare schede dell'utente).
2. `navigate` sull'URL, attendi il caricamento.
3. Con `javascript_tool` esegui:

```js
(async () => {
  const out = {};
  const nav = performance.getEntriesByType('navigation')[0];
  if (nav) {
    out.ttfb_ms = Math.round(nav.responseStart - nav.requestStart);
    out.dom_content_loaded_ms = Math.round(nav.domContentLoadedEventEnd);
    out.load_ms = Math.round(nav.loadEventEnd);
    out.transfer_kb = Math.round((nav.transferSize || 0) / 1024);
  }
  out.lcp_ms = await new Promise(r => {
    let v = null;
    new PerformanceObserver(l => { const e = l.getEntries(); v = e[e.length - 1]; })
      .observe({ type: 'largest-contentful-paint', buffered: true });
    setTimeout(() => r(v ? { ms: Math.round(v.startTime), element: v.element ? v.element.tagName + (v.element.className ? '.' + String(v.element.className).split(' ')[0] : '') : null } : null), 1500);
  });
  out.cls = await new Promise(r => {
    let s = 0;
    new PerformanceObserver(l => { for (const e of l.getEntries()) if (!e.hadRecentInput) s += e.value; })
      .observe({ type: 'layout-shift', buffered: true });
    setTimeout(() => r(Math.round(s * 1000) / 1000), 1500);
  });
  out.long_tasks = await new Promise(r => {
    const t = [];
    try {
      new PerformanceObserver(l => { for (const e of l.getEntries()) t.push(Math.round(e.duration)); })
        .observe({ type: 'longtask', buffered: true });
    } catch (e) {}
    setTimeout(() => r({ count: t.length, total_ms: t.reduce((a, b) => a + b, 0), max_ms: t.length ? Math.max(...t) : 0 }), 1500);
  });
  const res = performance.getEntriesByType('resource');
  out.requests = res.length;
  out.heaviest = res.sort((a, b) => (b.transferSize || 0) - (a.transferSize || 0)).slice(0, 5)
    .map(x => ({ url: x.name.slice(0, 120), kb: Math.round((x.transferSize || 0) / 1024), type: x.initiatorType }));
  out.render_blocking = res.filter(x => x.renderBlockingStatus === 'blocking').map(x => x.name.slice(0, 120));
  return JSON.stringify(out);
})()
```

4. Ripeti in vista mobile: `resize_window` a 390×844, ricarica, ripeti lo script; fai uno screenshot con `computer` per valutare above-the-fold e leggibilità. Controlla `innerWidth` nel risultato: le finestre Chrome spesso non scendono sotto ~500-570 px, quindi riporta nel report la larghezza reale ottenuta.
   Lo script va eseguito con `await` in testa (`await (async () => { ... })()`), altrimenti il tool restituisce un oggetto vuoto.
5. Chiudi la scheda creata con `tabs_close_mcp`.

**Limiti da dichiarare nel report:**
- Sono misure di **laboratorio** su una singola visita dalla connessione dell'utente, non dati di campo (CrUX).
- **INP non è misurabile senza interazioni reali**: riporta i long task (count, totale, massimo) come **indicatore stimato** del rischio INP (long task > 200 ms o totale > 600 ms → rischio INP alto).
- Nell'audit completo misura homepage + max 3 pagine di tipo diverso, non tutte e 20.

Cause tipiche da indicare nei fix: immagine LCP non prioritaria (manca `fetchpriority="high"`/preload, oppure ha `loading="lazy"`), CSS/JS render-blocking, font senza `font-display`, immagini e iframe senza dimensioni (CLS), script di terze parti pesanti (long task), TTFB > 800 ms (hosting/cache).

## Punteggio d'area

Calcolalo con le regole di `scoring.md`.
