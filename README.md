# Complaion - DORA Monitor

Strumento interno Complaion per il monitoraggio automatico delle pagine ufficiali relative al **Regolamento (UE) 2022/2554 - Digital Operational Resilience Act (DORA)**.

## Fonti monitorate

**Autorità nazionali (Italia):**
- Banca d'Italia — sezione DORA
- CONSOB — DORA
- IVASS — DORA

**Autorità europee (ESAs):**
- EBA — European Banking Authority
- ESMA — European Securities and Markets Authority
- EIOPA — European Insurance and Occupational Pensions Authority

**Fonti normative:**
- EUR-Lex — Regolamento (UE) 2022/2554

## Architettura

- **Scraper Python**: `scripts/scrape_dora.py` — esegue lo scan quotidiano, calcola hash e diff, estrae scadenze, chiama Gemini per generare un riassunto delle variazioni
- **GitHub Actions**: `.github/workflows/monitor.yml` — schedulato ogni giorno alle 06:00 UTC + lancio manuale disponibile
- **Dashboard**: `docs/index.html` + `app.js` + `style.css` — sito statico pubblicato via GitHub Pages
- **Dati persistiti**: `docs/data/documents.json`, `changes.json`, `scadenze.json`

## Come applicarlo (setup iniziale)

1. Creare un nuovo repo GitHub `complaion-dora-monitor` (pubblico, per usare GitHub Pages gratis)
2. Caricare i file di questo pacchetto nel repo (mantenendo la struttura delle cartelle)
3. In **Settings → Secrets and variables → Actions** aggiungere `GEMINI_API_KEY` (può essere lo stesso valore già usato per il monitor ACN NIS2)
4. In **Settings → Pages** attivare GitHub Pages dalla cartella `docs/` del branch `main`
5. Attendere che il workflow parta la prima volta (schedulato alle 06:00 UTC) o lanciarlo manualmente dal tab **Actions**
6. La dashboard sarà disponibile a `https://<username>.github.io/complaion-dora-monitor/`

## Manutenzione

- **Aggiungere URL da monitorare**: modificare la lista `TARGETS` in `scripts/scrape_dora.py`
- **Aggiungere scadenze note**: modificare la lista `SEED_DEADLINES` in `scripts/scrape_dora.py`
- **Frequenza scan**: modificare la stringa `cron` in `.github/workflows/monitor.yml`

## Note sulle URL delle autorità

Le URL delle pagine DORA delle autorità nazionali/europee possono cambiare. Se al primo scan alcune fonti risultano `HTTP 404` o `fetch_error`, aggiornare la lista `TARGETS` con le URL correnti.
