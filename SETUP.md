# 📸 Instagram Collections - Sistema di Automazione Completo

> Automatizza la pubblicazione delle tue collezioni Instagram salvate su GitHub Pages

## 🌐 Il Tuo Sito

**URL Pubblico**: https://aletrentin-blip.github.io/instagram-collections/instagram_collezioni.html

---

## 📋 Panoramica Sistema

Questo sistema automatizza completamente il processo di:
1. Export dati Instagram (manuale, 1 click)
2. Estrazione automatica ZIP
3. Parsing collezioni e post
4. Generazione HTML
5. Deploy automatico su GitHub Pages

### ⏱ Workflow Completo

```
Instagram App → Richiedi Export (1 click ogni 1-2 settimane)
    ↓
Email/Download → Salva ZIP in cartella monitorata
    ↓
Power Automate → Rileva file ZIP automaticamente  
    ↓
Python Script → Estrae dati + Genera HTML
    ↓
Git Push → Deploy automatico su GitHub
    ↓
GitHub Pages → Sito aggiornato live! 🎉
```

**Tempo totale**: 30 secondi di lavoro manuale + 2-3 minuti automatici

---

## 🛠 Setup Iniziale (Una Tantum)

### Prerequisiti

- ✅ Python 3.8+ installato
- ✅ Git installato
- ✅ Account GitHub (già configurato)
- ✅ Power Automate Desktop (gratuito con Windows 11)
- ✅ OneDrive o Google Drive

### Step 1: Installazione Dipendenze Python

```bash
pip install beautifulsoup4 lxml python-dotenv
```

### Step 2: Struttura Directory

Crea questa struttura sul tuo computer:

```
C:/Users/TuoNome/InstagramCollections/
├── scripts/
│   ├── auto_deploy_instagram.py
│   ├── generate_instagram_pages.py (già esistente)
│   └── setup_github_token.py
├── input/                    # Cartella monitorata per ZIP
├── output/                   # HTML generati
└── temp/                     # File temporanei
```

### Step 3: GitHub Personal Access Token

1. **Vai su**: https://github.com/settings/tokens
2. **Click**: "Generate new token" → "Generate new token (classic)"
3. **Nome**: `Instagram Collections Deploy`
4. **Scadenza**: `No expiration` (oppure 1 anno)
5. **Seleziona Scopes**:
   - ✅ `repo` (tutti i sotto-permessi)
   - ✅ `workflow`
6. **Genera** e COPIA il token (inizia con `ghp_`)

7. **Salva il token**:

```bash
# Esegui lo script di setup
python setup_github_token.py
# Incolla il token quando richiesto
```

Il token sarà salvato in: `C:/Users/TuoNome/.instagram_collections.env`

### Step 4: Test Manuale

```bash
# Test deploy manuale
python auto_deploy_instagram.py
```

Se funziona, vedrai:
```
✅ Deploy completato!
🌐 Sito disponibile su: https://aletrentin-blip.github.io/instagram-collections
```

---

## 🤖 Setup Power Automate

### Opzione A: Import Flow (Raccomandato)

1. Apri **Power Automate Desktop**
2. **Import** → Seleziona `instagram_automate_flow.json`
3. **Configura**:
   - Cartella monitorata: `C:/Users/TuoNome/OneDrive/Instagram`
   - Script Python: `C:/Users/TuoNome/InstagramCollections/scripts/auto_deploy_instagram.py`
   - Email notifiche: `tuaemail@example.com`

### Opzione B: Creazione Manuale

1. **Nuovo Flow**: "Instagram Collections Auto-Deploy"
2. **Trigger**: "When a file is created"
   - Cartella: `C:/Users/TuoNome/OneDrive/Instagram`
   - Filtro: `*.zip`
3. **Azione 1**: Delay 30 secondi (attendi completamento download)
4. **Azione 2**: Run PowerShell/CMD
   ```powershell
   python C:/Users/TuoNome/InstagramCollections/scripts/auto_deploy_instagram.py
   ```
5. **Azione 3**: Send email notification (opzionale)

---

## 📱 Uso Quotidiano

### Aggiornamento Collezioni (Ogni 1-2 Settimane)

1. **Instagram App** → Settings → Your Activity → Download Your Information
2. **Seleziona**: 
   - ✅ Saved posts
   - ✅ Saved collections  
   - Formato: HTML
   - Range: Last 1 year (o personalizza)
3. **Richiedi Download**
4. **Attendi email** (5-60 minuti)
5. **Scarica ZIP** e salvalo nella cartella `C:/Users/TuoNome/OneDrive/Instagram`

**FATTO!** Il resto è automatico:
- Power Automate rileva il file
- Script Python processa i dati
- Deploy su GitHub
- Sito aggiornato in 2-3 minuti

---

## 🔧 Script Python - Riferimento

### `auto_deploy_instagram.py`

**Cosa fa**:
- Trova automaticamente il file ZIP più recente
- Estrae i dati in cartella temporanea
- Parse di `saved_collections.html`
- Genera `index.html` aggiornato
- Clone/pull del repository GitHub
- Commit e push automatico

**Variabili d'ambiente richieste**:
```bash
GITHUB_TOKEN=ghp_your_token_here
```

**Esecuzione manuale**:
```bash
python auto_deploy_instagram.py
```

**Esecuzione con ZIP specifico**:
```bash
python auto_deploy_instagram.py --zip /path/to/instagram_export.zip
```

### `setup_github_token.py`

**Cosa fa**:
- Guida interattiva per creare il token GitHub
- Salva il token in modo sicuro
- Testa validità del token

**Esecuzione**:
```bash
python setup_github_token.py
```

---

## 📊 Monitoraggio

### Log File

I log sono salvati in:
```
C:/Users/TuoNome/InstagramCollections/logs/deploy_YYYYMMDD.log
```

### Verifica Deploy

1. **GitHub**: https://github.com/aletrentin-blip/instagram-collections/actions
2. **Sito Live**: https://aletrentin-blip.github.io/instagram-collections/instagram_collezioni.html

### Email Notifiche

Se configurate in Power Automate, riceverai email per:
- ✅ Deploy completato con successo
- ❌ Errori durante il processo

---

## 🐛 Troubleshooting

### Problema: "GITHUB_TOKEN non impostato"

**Soluzione**:
```bash
python setup_github_token.py
# Rigenera e salva il token
```

### Problema: "Permission denied (git push)"

**Causa**: Token scaduto o permessi insufficienti

**Soluzione**:
1. Vai su https://github.com/settings/tokens
2. Controlla scadenza token
3. Rigenera con permessi `repo` + `workflow`
4. Riesegui `setup_github_token.py`

### Problema: "File ZIP non trovato"

**Soluzione**:
- Verifica che ZIP sia in una delle cartelle monitorate:
  - `~/Downloads`
  - `~/OneDrive/Instagram`
  - `~/Google Drive/Instagram`

### Problema: "Deploy lento / non si aggiorna"

**Causa**: GitHub Pages cache

**Soluzione**:
- Attendi 2-3 minuti per propagazione
- Fai hard refresh: `Ctrl+Shift+R` (Windows) o `Cmd+Shift+R` (Mac)
- Verifica commit su: https://github.com/aletrentin-blip/instagram-collections/commits/main

---

## 🔒 Sicurezza

### Token GitHub

- ⚠️ **MAI condividere** il token
- ⚠️ **MAI committare** `.env` file su Git
- ✅ Il file `.instagram_collections.env` è nella tua home directory (sicuro)

### File `.gitignore`

Il repository include già:
```
*.env
.instagram_collections.env
temp/
*.log
```

---

## 📈 Statistiche

- **Collezioni totali**: 177
- **Post totali**: 4481
- **Ultimo aggiornamento**: Automatico ogni volta che carichi nuovo ZIP
- **Uptime**: 99.9% (hosting GitHub Pages)

---

## 🎨 Personalizzazione

### Modificare Design HTML

Edita il template in `generate_instagram_pages.py`:
```python
def generate_html(collections_data):
    # Modifica CSS qui
    html = '<style>...</style>'
    return html
```

### Aggiungere Filtri/Features

Il file HTML già include:
- ✅ Filtro per collezione
- ✅ Visualizzazione immagini (lazy loading)
- ✅ Link diretti ai post
- ✅ Conteggio post per collezione

---

## 🆘 Supporto

Per problemi o domande:
1. Controlla i log: `logs/deploy_YYYYMMDD.log`
2. Verifica GitHub Actions: https://github.com/aletrentin-blip/instagram-collections/actions
3. Test manuale: `python auto_deploy_instagram.py`

---

## 📝 Note

- Instagram limita export a **1 volta ogni 4 giorni**
- File ZIP può essere **fino a 2GB** per account grandi
- GitHub Pages ha limite **1GB** per repository (sufficiente per HTML)
- Deploy time: 1-3 minuti dalla push

---

## ✨ Credits

Sistema sviluppato con:
- Python 3.x
- BeautifulSoup4 (HTML parsing)
- GitHub Pages (hosting gratuito)
- Power Automate Desktop (automazione)

---

**Ultimo aggiornamento**: 19 Gennaio 2026
