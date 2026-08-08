# ASUS ROG (Strix / Scar / Zephyrus)

Guida passo passo. I nomi delle voci possono cambiare leggermente tra versioni di Armoury Crate: dove il nome varia è indicato.

## 1. Software da mantenere

| Componente | Tenere | Perché |
|---|---|---|
| Armoury Crate | Sì | Unico modo per cambiare GPU Mode, profili termici e curve ventole |
| Armoury Crate Service | Sì | Senza il servizio, l'app si apre ma i cambi non vengono applicati |
| ASUS Framework Service | Sì | Dipendenza di sistema di Armoury Crate |
| MyASUS | Sì, ma senza avvio automatico | Serve solo per driver update e garanzia |
| Driver Realtek / audio | Sì | — |
| Driver Intel / chipset | Sì | — |
| Driver NVIDIA | Sì | Vedi [nvidia.md](nvidia.md) |
| Aura Sync / RGB | Facoltativo | Solo estetica, nessun impatto sulle prestazioni |

Per togliere MyASUS dall'avvio senza disinstallarlo:

```text
Ctrl+Shift+Esc → Avvio (Startup) → MyASUS → tasto destro → Disabilita
```

## 2. Modalità GPU

### 2.1 DAILY MODE — procedura

1. Apri **Armoury Crate**.
2. Vai su **Home** (o **Dispositivo**, secondo la versione) → **GPU Mode**.
3. Seleziona **Ottimizzato / Optimized**.
4. Riavvia se richiesto.
5. Windows: `Impostazioni → Sistema → Alimentazione e batteria → Modalità alimentazione` → **Bilanciato** o **Prestazioni migliori**.
6. In Armoury Crate → **Modalità operativa / Scenario Profiles** → **Prestazioni (Performance)**.

Risultato: Optimus dinamico, iGPU quando basta e NVIDIA quando serve. Meno consumo, meno calore, più autonomia.

### 2.2 SHOW MODE — procedura

Eseguire **in quest'ordine**, non a caso:

1. **Collega l'alimentatore.** Mai Ultimate a batteria.
2. Armoury Crate → **GPU Mode** → **Ultimate**.
3. **Riavvia.** Obbligatorio: il MUX switch è fisico, non commuta a caldo.
4. Armoury Crate → **Modalità operativa** → **Turbo**.
5. Windows → piano di alimentazione **Prestazioni massime / Ultimate Performance** (vedi [windows.md](windows.md#3-alimentazione) per attivarlo se non compare).
6. **Verifica** che il pannello sia davvero sulla NVIDIA (paragrafo 2.3).
7. **Solo ora** apri TouchDesigner / MadMapper / Resolume.

Perché l'ordine conta: se apri il software prima del riavvio, l'app resta agganciata alla iGPU e non "vede" il cambio finché non la chiudi e riapri.

### 2.3 Verifica che Ultimate sia attivo

Metodo rapido da interfaccia:

```text
Impostazioni → Sistema → Schermo → Impostazioni schermo avanzate → Visualizza informazioni scheda
```

Deve comparire la GPU NVIDIA, non la Intel/AMD integrata.

Metodo da terminale (PowerShell):

```powershell
nvidia-smi --query-gpu=name,driver_version,power.limit --format=csv
```

Con un progetto aperto, `nvidia-smi` deve elencare il processo (`TouchDesigner.exe`, `Resolume Arena.exe`, …) nella tabella dei processi. Se l'elenco è vuoto mentre il software è in esecuzione, l'app sta girando sulla GPU sbagliata.

Alternativa: `Gestione attività → Dettagli → tasto destro sulle colonne → Seleziona colonne → Motore GPU`.

## 3. Profili Armoury Crate

| Profilo | Quando | Note |
|---|---|---|
| Silent | Batteria, uso leggero | Ventole quasi ferme, clock ridotti |
| Performance | Uso quotidiano serio | Miglior compromesso rumore/prestazioni |
| Turbo | Prove, live, rendering, video pesante | Ventole aggressive, clock massimi nel thermal budget |
| Manuale | Solo dopo test | Curve ventole custom |

Prima di usare **Manuale**, fai i test termici e di stabilità: FurMark per la GPU, Cinebench per la CPU, almeno 15-20 minuti ciascuno, controllando che le temperature non vadano in throttling sostenuto. Una curva ventole sbagliata produce instabilità o throttling proprio sotto carico live.

## 4. BIOS

### 4.1 Aggiornamento — prerequisiti

1. Alimentatore collegato.
2. Batteria sopra il 50%.
3. Tutti i software chiusi.
4. Non toccare il PC durante il flash: interromperlo può rendere la macchina non avviabile.

### 4.2 Dopo l'aggiornamento, se qualcosa non torna

```text
F2 all'avvio → Load Optimized Defaults → Save & Exit
```

### 4.3 Impostazioni BIOS rilevanti per il live

Da toccare **solo se hai un problema specifico**, non preventivamente:

- **Fast Boot** — disattivalo se periferiche USB (interfaccia audio, DMX, controller MIDI) a volte non vengono riconosciute all'avvio.
- **Wake on LAN / Wake on USB** — lascia disattivato se non fai wake da remoto: evita risvegli accidentali durante uno show.
- **Secure Boot, opzioni storage/RAID** — non toccare senza un motivo preciso: rischi un sistema non avviabile.

## 5. Problemi ricorrenti

- **Show Mode "non aggancia" la NVIDIA dopo un update Windows.** Il cambio GPU Mode da Armoury Crate a volte fallisce in silenzio. Prima di sospettare l'hardware: `services.msc` → **Armoury Crate Service** → tasto destro → **Riavvia**, poi riprova il cambio e riavvia il PC.
- **Armoury Crate non si apre o è vuoto.** Verifica che **ASUS Framework Service** sia in esecuzione (`services.msc`).
- **Prestazioni sotto le attese in Turbo.** Se il clock cala in modo sostenuto sotto carico, il problema è quasi sempre pulizia dissipatori/pasta termica, non un'impostazione software.

Tieni un log delle versioni driver che funzionano bene con i tuoi progetti in [troubleshooting.md](troubleshooting.md): non tutti gli aggiornamenti NVIDIA sono neutri per TouchDesigner/Notch.
