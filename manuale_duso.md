# LISYclock Config Editor – Manuale d'uso

**Versione 1.11** | Lingua: Italiano

---

## Indice

1. [Panoramica](#1-panoramica)
2. [Requisiti & Avvio](#2-requisiti--avvio)
3. [Operazioni sui file](#3-operazioni-sui-file)
4. [Scheda: Clock](#4-scheda-clock)
5. [Scheda: General](#5-scheda-general)
6. [Scheda: Events](#6-scheda-events)
7. [Scheda: GI LEDs](#7-scheda-gi-leds)
8. [Scheda: Attract Mode](#8-scheda-attract-mode)
9. [Scheda: TTS Settings](#9-scheda-tts-settings)
10. [Scheda: Update](#10-scheda-update)
11. [Scheda: Debug](#11-scheda-debug)
12. [config.txt – Formato & Struttura](#12-configtxt--formato--struttura)
13. [Suggerimenti & Note](#13-suggerimenti--note)

---

## 1. Panoramica

Il **LISYclock Config Editor** è uno strumento per modificare comodamente il file di configurazione `config.txt` del **LISYclock**.

- Applicazione standalone: un singolo file `LISYclock_config_editor.html`, nessun server, nessun passaggio di build
- Basta aprire `LISYclock_config_editor.html` nel browser e iniziare
- Supporta il caricamento/scaricamento diretto sull'hardware LISYclock via IP (HTTP) o USB

![Vista completa dell'editor con file di configurazione aperto](pics/Full%20view%20of%20the%20editor%20with%20an%20open%20configuration%20file.jpg)

---

## 2. Requisiti & Avvio

### Browser

| Browser | Supporto |
|---------|----------|
| **Chrome / Edge** (consigliato) | Supporto completo inclusa la File System Access API (finestra di dialogo nativa apri/salva) e la Web Serial API (connessione USB) |
| Firefox / Safari | Funzionante per la modifica dei file, ma utilizza `<input type=file>` / link di download come fallback; modalità USB non supportata |

### Avvio

1. Aprire `LISYclock_config_editor.html` dalla directory del progetto **localmente** nel browser (doppio clic o `File → Apri`)
2. L'applicazione si avvia immediatamente con una configurazione predefinita vuota

> **Importante:** Per la comunicazione diretta con il LISYclock via IP (scheda "Clock"), il file deve essere aperto tramite `file://` – **non** tramite HTTPS. I browser bloccano le richieste HTTP dalle pagine HTTPS per ragioni di sicurezza. La modalità USB funziona indipendentemente da come il file è stato aperto.

---

## 3. Operazioni sui file

I pulsanti nella **barra di navigazione** (in alto) controllano tutte le operazioni sui file:

| Pulsante | Funzione |
|----------|----------|
| **Apri** | Apre un `config.txt` esistente dal file system locale |
| **Salva** | Scrive la configurazione corrente nel file aperto (attivo solo quando un file è aperto) |
| **Salva con nome…** | Permette di scegliere un nuovo file di destinazione e vi salva la configurazione |
| **Nuova configurazione** | Ripristina tutti i campi ai valori predefiniti (attenzione: le modifiche non salvate andranno perse) |

Il **nome del file corrente** viene visualizzato al centro della barra di navigazione. Finché non è aperto alcun file, appare "— nessun file aperto —".

---

## 4. Scheda: Clock

Questa scheda consente la comunicazione diretta con l'hardware LISYclock. Scegliere tra la **Modalità IP** (WiFi) e la **Modalità USB** (cavo USB).

### Modalità di connessione

Selezionare il tipo di connessione tramite i pulsanti radio in cima alla scheda:

| Modalità | Descrizione |
|----------|-------------|
| **IP Mode** | Connessione all'orologio tramite rete locale e indirizzo IP (richiede WiFi) |
| **USB Mode** | Connessione diretta all'orologio tramite cavo USB (Web Serial API, solo Chrome/Edge) |

### Modalità IP

Inserire l'indirizzo IP del proprio LISYclock (ad es. `192.168.1.42`). L'indirizzo viene salvato nel browser e viene precompilato automaticamente alla successiva apertura.

Un clic su **Connect Clock** verifica la raggiungibilità dell'orologio:

| Indicatore | Significato |
|-----------|-------------|
| Grigio | Nessun test ancora eseguito |
| Verde | Connessione riuscita |
| Rosso | Connessione fallita |

In caso di connessione riuscita vengono visualizzati la **versione del firmware** e la **versione API**. Se la versione API non corrisponde alla versione attesa, viene visualizzato un **avviso** (le funzioni potrebbero comunque essere parzialmente utilizzabili).

### Modalità USB

| Impostazione | Descrizione |
|--------------|-------------|
| **Baud Rate** | Velocità di comunicazione seriale; predefinito: 115200 |
| **Reset Delay** | Tempo di attesa dopo la connessione prima che venga inviato l'handshake; predefinito: 1500 ms |

Fare clic su **Connect USB** per stabilire la connessione. Il punto di stato indica lo stato:

| Indicatore | Significato |
|-----------|-------------|
| Grigio | Non connesso |
| Giallo (lampeggiante) | Handshake in corso — in attesa del reset ESP32 |
| Verde | Connesso |

> **Nota:** L'orologio si riavvia una volta quando si fa clic su *Connect USB*. Questo è normale — il riavvio attiva l'handshake USB. Attendere che lo stato mostri *Connected*.

### Test connessione WiFi (solo modalità USB)

In modalità USB, fare clic su **Test WiFi Connection** per inviare le credenziali WiFi dalla scheda General all'orologio e verificare la connessione. Il punto di stato mostra il risultato.

### Configurazione

| Pulsante | Funzione |
|----------|----------|
| **Upload Config to Clock** | Trasferisce la configurazione corrente (visualizzata nell'editor) all'orologio |
| **Download Config from Clock** | Scarica il `config.txt` dall'orologio e lo apre nell'editor |

### Impostazione dell'ora

Un clic su **Set Clock Time from PC** sincronizza l'ora dell'orologio con la data e l'ora correnti del PC.

### Riavvio

Un clic su **Reboot Clock** riavvia il LISYclock. Una finestra di conferma previene riavvii accidentali.

### Caricamento file

Trasferire qualsiasi file (ad es. MP3, batch) sulla scheda SD dell'orologio.

### Gestione file sulla scheda SD

- Usare **Refresh** per caricare l'elenco file corrente dalla scheda SD
  - L'elenco mostra nome, dimensione e **data** di modifica di ogni file
  - I singoli file possono essere **scaricati**, **rinominati** o **eliminati**

### Log di protocollo (solo modalità USB)

In modalità USB, viene mostrato un pannello di log di protocollo con tutte le comunicazioni seriali tra l'editor e l'orologio.

### Avviso HTTPS

Se l'editor viene aperto tramite un URL HTTPS, il browser blocca tutte le richieste HTTP verso l'orologio (modalità IP). Soluzione: aprire `LISYclock_config_editor.html` **localmente** tramite `file://`.

---

## 5. Scheda: General

Impostazioni generali del LISYclock.

![Scheda General](pics/General%20tab.jpg)

### Impostazioni WiFi

| Campo | Descrizione |
|-------|-------------|
| Abilitato (casella) | Abilita/disabilita il WiFi (`WIFI_ENABLE=yes` / `WIFI_ENABLE=no`) |
| SSID | Nome della rete WiFi |
| Password | Password WiFi (pulsante Mostra/Nascondi disponibile) |

> Le credenziali WiFi vengono salvate in `config.txt`. Usare **Test WiFi Connection** nella scheda Clock (modalità USB) per verificare che le credenziali funzionino.

### Luminosità del display

Valore da **0** (scuro) a **7** (luminosità massima). Corrisponde alla chiave di configurazione `DISP_BRIGHT`.

### Server FTP

| Campo | Descrizione |
|-------|-------------|
| Abilitato (casella) | Abilita/disabilita il server FTP (`#FTP_USER=…` quando disabilitato) |
| Nome utente | Nome di accesso FTP |
| Password | Password FTP |

### Fuso orario

| Campo | Descrizione |
|-------|-------------|
| Abilitato (casella) | Abilita/disabilita la configurazione del fuso orario |
| Stringa POSIX | Fuso orario in formato POSIX |

Esempio per l'Europa centrale con ora legale:
```
CET-1CEST,M3.5.0,M10.5.0/3
```

### Stringhe dei giorni della settimana

Se abilitato, l'orologio mostra i nomi localizzati dei giorni della settimana. Ognuno dei 7 campi (domenica–sabato) può avere al massimo **6 caratteri**. Le voci più corte vengono automaticamente completate con spazi fino a 6 caratteri.

Esempio (Italiano): `Dom`, `Lun`, `Mar`, `Mer`, `Gio`, `Ven`, `Sab`

---

## 6. Scheda: Events

Gli eventi definiscono le azioni temporizzate del LISYclock.

![Scheda Events con più voci](pics/Events%20tab%20with%20multiple%20entries.jpg)

### Tipi di evento

| Tipo | Descrizione | Campo valore |
|------|-------------|--------------|
| **TTS** | Riprodurre testo tramite Text-to-Speech | Testo libero (ad es. "Buongiorno!") |
| **MP3** | Riprodurre un file MP3 dalla scheda SD | Nome file (ad es. `alarm.mp3`) |
| **BATCH** | Eseguire un file batch dalla scheda SD | Nome file (ad es. `script.bat`) |
| **DISPLAY** | Accendere o spegnere tutti i display | Dropdown: `on` / `off` |
| **SYNC_TIME** | Sincronizzare l'orario tramite NTP | Dropdown: 1–10 tentativi |
| **SAY_TIME** | Dire l'ora corrente tramite TTS | Dropdown: `german` / `english` / `italian` |
| **GI_LEDS** | Accendere o spegnere tutti i LED GI | Dropdown: `on` / `off` |
| **ATTRACT_LEDS** | Accendere o spegnere tutti i LED Attract | Dropdown: `on` / `off` |

### Formato orario

L'**orario** di un evento viene specificato come `HH:MM:W` o `HH:MM-GG.MM.AAAA`. `*` sta per "qualsiasi" in ogni campo.

#### Modalità giorno della settimana: `HH:MM:W`

| Campo | Significato |
|-------|-------------|
| `HH` | Ora (0–23), `*` = ogni ora |
| `MM` | Minuto (0–59), `*` = ogni minuto |
| `W` | Giorno della settimana: 0 = domenica, 1 = lunedì, … 6 = sabato, `*` = ogni giorno |

#### Modalità data: `HH:MM-GG.MM.AAAA`

Attiva quando nel campo giorno della settimana è selezionato `*` (data). Il campo data appare quindi in aggiunta.

| Campo | Significato |
|-------|-------------|
| `GG` | Giorno (1–31), `*` = ogni giorno |
| `MM` | Mese (1–12), `*` = ogni mese |
| `AAAA` | Anno (quattro cifre), `*` = ogni anno |

#### Esempi

| Orario | Significato |
|--------|-------------|
| `7:30:1` | Ogni lunedì alle 07:30 |
| `*:*:*` | Ogni minuto, ogni giorno |
| `8:0:*` | Ogni giorno alle 08:00 |
| `12:0-24.12.*` | Ogni 24 dicembre alle 12:00 |
| `*:0-*.*.*` | Ogni ora intera, ogni giorno |

### Controlli

- **+ Add Event** (pulsante in alto a destra): Aggiunge un nuovo evento in fondo alla lista
- **✕** (a destra di un evento): Rimuove quell'evento

---

## 7. Scheda: GI LEDs

Definisce i colori iniziali dei LED GI (Illuminazione Generale).

![Scheda GI LEDs](pics/GI%20LEDs%20tab.jpg)

### Campi per ogni riga LED

| Campo | Descrizione |
|-------|-------------|
| LED | Numero LED (1–64) |
| R / G / B | Valore colore per canale (0–255) |
| Selettore colore | Apre un selettore colore nativo |
| Punto di anteprima | Mostra il colore corrente come un piccolo cerchio colorato |

Il selettore colore e i campi RGB sono sincronizzati: una modifica nel selettore aggiorna i campi RGB e viceversa.

### Controlli

- **+ Aggiungi LED**: Aggiunge una nuova voce LED in fondo alla lista
- **✕**: Rimuove questa voce LED

---

## 8. Scheda: Attract Mode

Configura l'illuminazione dell'Attract Mode (luci animate in standby) in **5 gruppi** (AT1–AT5).

![Scheda Attract Mode con gruppo AT1 espanso](pics/Attract%20Mode%20tab%20with%20group%20AT1%20expanded.jpg)

Ogni gruppo è visualizzato come **pannello accordion richiudibile**.

### Impostazioni per gruppo

| Campo | Descrizione |
|-------|-------------|
| **Frequenza lampeggio** | Intervallo di lampeggio in millisecondi |
| **Attivazione casuale** | Casella: attivare il gruppo casualmente |
| **Rand** | Soglia casuale (0–255); più alto è il valore, meno frequentemente il gruppo viene attivato |
| **Lista LED** | Elenco dei LED in questo gruppo (come GI LEDs: numero, R, G, B, selettore colore) |

- **+ Aggiungi LED**: Aggiunge una nuova voce LED al gruppo
- **✕**: Rimuove una voce LED dal gruppo

---

## 9. Scheda: TTS Settings

Impostazioni per la funzione Text-to-Speech (utilizza il servizio WIT.ai).

![Scheda TTS Settings](pics/TTS%20Settings%20tab.jpg)

| Campo | Valori possibili | Predefinito |
|-------|-----------------|-------------|
| **WIT.ai Token** | Testo libero (chiave API da wit.ai) | — |
| **Voice** | 17 voci (US EN, UK EN, CA EN), ad es. `wit$Cooper` | `wit$Cooper` |
| **Style** | `default` / `soft` / `formal` / `fast` / `projected` | `default` |
| **Speed** | 0–200 | 80 |
| **Pitch** | 0–200 | 80 |
| **Gain** | 0–100 | 30 |
| **SFX Char** | `none` / `chipmunk` / `monster` / `daemon` / `robot` / `alien` | `none` |
| **SFX Env** | `none` / `reverb` / `room` / `cathedral` / `radio` / `phone` | `none` |

> Il token API di WIT.ai si ottiene dopo la registrazione gratuita su [wit.ai](https://wit.ai).

---

## 10. Scheda: Update

La scheda Update offre tre modi per aggiornare il firmware del LISYclock.

### Carica file firmware locale (solo modalità IP)

1. Selezionare un file `.bin` del firmware con il pulsante di selezione file
2. Fare clic su **Update & Reboot** — il file viene trasferito sulla scheda SD come `update.bin` e l'orologio si riavvia automaticamente per applicare l'aggiornamento

### Oppure seleziona da lisy.dev

1. Prima connettersi all'orologio nella scheda Clock (modalità IP) in modo che l'editor conosca la versione hardware
2. Fare clic su **Fetch versions** — appare un menu a tendina con tutti i file firmware corrispondenti da lisy.dev
3. Selezionare la versione desiderata dal menu a tendina
4. Fare clic su **Update & Reboot** — il file viene scaricato da lisy.dev e trasferito all'orologio

> Se è selezionata una versione dal server, ha la precedenza su un file selezionato localmente.

### Flash via USB – installazione completa (solo modalità USB)

Fare clic su **Flash via USB** per scrivere il pacchetto firmware completo (bootloader, firmware binary, tabella delle partizioni e dati OTA) direttamente sull'ESP32 tramite USB. Nessuna connessione WiFi richiesta.

> Solo Chrome/Edge. Durante il flashing viene mostrato un log di avanzamento sotto il pulsante. Non scollegare il cavo USB fino al completamento dell'operazione.

---

## 11. Scheda: Debug

La scheda Debug fornisce una console seriale USB grezza per utenti avanzati e sviluppatori.

### Connessione

Selezionare il **Baud Rate** (predefinito: 115200) e il **Reset Delay** (predefinito: 1500 ms), quindi fare clic su **CONNECT**. Il punto di stato indica lo stato della connessione:

| Stato | Significato |
|-------|-------------|
| Rosso | Non connesso |
| Giallo (lampeggiante) | Handshake in corso — in attesa del reset ESP32 |
| Verde (lampeggiante) | Connesso |

Gli stati dei segnali DTR/RTS sono mostrati nell'angolo in alto a destra.

### Opzioni

| Opzione | Descrizione |
|---------|-------------|
| **Handshake** | Abilita/disabilita il protocollo handshake `0x55` → `OK:READY`. Disabilitare per il monitoraggio diretto grezzo |
| **Boot Logs** | Mostra/nascondi le righe di log di avvio dell'ESP32 |
| **Auto-Scroll** | Scorrimento automatico all'ultimo output |
| **Timestamp** | Aggiunge un timestamp a ogni riga ricevuta |

### Fine riga

Selezionare la terminazione di riga aggiunta ai comandi inviati: `NL (\n)`, `CR+NL`, `CR` o `None`.

### Console

La console mostra tutto l'output seriale dell'ESP32 con righe codificate a colori:
- Verde: dati ricevuti
- Arancione: messaggi di sistema
- Rosso: errori
- Grigio scuro: messaggi di avvio (quando Boot Logs è abilitato)

---

## 12. config.txt – Formato & Struttura

### Formato base

```
KEY=value
```

- Ogni riga contiene una chiave e un valore, separati da `=`
- Un `#` **all'inizio di una riga** disabilita la riga (senza eliminarla)
- Le maiuscole/minuscole dei nomi delle chiavi sono normalizzate nell'editor

### Regole speciali

| Regola | Descrizione |
|--------|-------------|
| Blocco FTP | Nessuno spazio dopo `#`: `#FTP_USER=lisy` |
| Fuso orario | Spazio dopo `#`: `# TIMEZONE="CET-1CEST,…"` |
| Stringhe giorni | Esattamente 6 caratteri, completati con spazi: `Lun   ` |
| `EVENT_SYNC_TIME` | Valore **senza** virgolette: `EVENT_SYNC_TIME=2:0:*,10` |
| `EVENT_SAY_TIME` | Valore **con** virgolette: `EVENT_SAY_TIME=14:12:*,"italian"` |
| Tutti gli altri `EVENT_*` | Valore **con** virgolette: `EVENT_TTS=8:0:*,"Buongiorno!"` |

### Estratto di esempio

```
WIFI_ENABLE=yes
WIFI_SSID="MiaRete"
WIFI_PWD="MiaPassword"
DISP_BRIGHT=5
#FTP_USER=lisy
#FTP_PWD=lisy
# TIMEZONE="CET-1CEST,M3.5.0,M10.5.0/3"
EVENT_SYNC_TIME=2:0:*,10
EVENT_SAY_TIME=14:12:*,"italian"
EVENT_TTS=8:0:*,"Buongiorno!"
```

---

## 13. Suggerimenti & Note

- **Usare Chrome o Edge:** Solo questi browser supportano la File System Access API per le finestre di dialogo native apri/salva e la Web Serial API per la modalità USB. In Firefox e Safari appaiono invece selettori file standard o link di download; la modalità USB non è disponibile.

- **Aprire localmente per l'accesso IP:** Se la comunicazione con il LISYclock in modalità IP non funziona, verificare che `LISYclock_config_editor.html` sia aperto tramite `file://` (localmente) e non tramite `https://`.

- **FTP come alternativa:** Il `config.txt` completato può anche essere trasferito sulla scheda SD dell'orologio tramite FTP (se il server FTP è abilitato nella configurazione).

- **Chiavi sconosciute:** Se l'editor incontra chiavi sconosciute durante il caricamento, le ignora e mostra un banner di avviso con le righe interessate. Non vengono mantenute al salvataggio. Se si hanno estensioni personalizzate nel `config.txt`, eseguirne prima un backup separato.

- **Nessuna funzione annulla:** L'editor non dispone di una funzione annulla. In caso di modifiche accidentali, riaprire il file (senza salvare prima) ripristinerà lo stato precedente.

- **Connessione USB condivisa:** Una connessione USB stabilita nella scheda Clock viene automaticamente condivisa con la scheda Debug e viceversa. Esiste sempre una sola connessione USB simultanea.

---

*Creato per LISYclock Config Editor v1.11*
