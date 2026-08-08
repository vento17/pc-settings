# NVIDIA

Guida passo passo per driver e impostazioni GPU su workstation live.

## 1. Quale driver

**NVIDIA Studio Driver.** Escono meno spesso dei Game Ready ma sono certificati e testati più a fondo su software creativi: TouchDesigner, Notch, Resolume, DaVinci Resolve.

Game Ready va bene solo se ti serve una feature specifica appena uscita. Per uso live, Studio resta la scelta prudente.

## 2. Installazione o aggiornamento driver

1. Apri la **NVIDIA App** (sostituisce GeForce Experience).
2. **Driver** → seleziona **Studio Driver**.
3. Scegli **Installazione personalizzata / Custom**.
4. Spunta **Esegui installazione pulita (Clean Install)** — rimuove residui di profili e impostazioni precedenti, che sono una causa frequente di comportamenti incoerenti.
5. Installa e **riavvia**.
6. Riapri i profili per-app (paragrafo 4): la clean install li azzera.
7. Apri un progetto reale e verifica prima di considerare l'aggiornamento concluso.

**Regola d'oro:** non aggiornare i driver NVIDIA nei giorni immediatamente prima di uno spettacolo. Testa sempre su un progetto reale con margine di qualche giorno, non di ore.

Se un driver nuovo peggiora le cose, disinstallalo e torna alla versione precedente dall'archivio Studio; annota la versione buona in [troubleshooting.md](troubleshooting.md).

## 3. Pannello di controllo NVIDIA — impostazioni globali

Percorso:

```text
Pannello di controllo NVIDIA → Gestisci impostazioni 3D → Impostazioni globali
```

| Impostazione | Valore | Perché |
|---|---|---|
| Processore grafico preferito | Processore NVIDIA ad alte prestazioni | Forza la dGPU anche per app che finirebbero sull'iGPU |
| Modalità gestione energetica | Preferisci prestazioni massime | Evita che la GPU scali i clock in idle tra un frame e l'altro: meno micro-stutter |
| Modalità a bassa latenza | On (Ultra solo se l'app la gestisce bene) | Riduce i frame in coda: utile per input/output realtime |
| Sincronizzazione verticale | Off | Il V-Sync va deciso per progetto e per output (proiettore vs monitor), non forzato globalmente |
| Cache shader | On / Predefinito | Evita ricompilazione shader a ogni avvio, riduce lo stutter iniziale |
| G-Sync / G-Sync Compatible | Off per show e proiettori | Proiettori e molti media server non supportano VRR: rischio flicker o output instabile |
| Qualità filtro texture | Prestazioni elevate | Irrilevante per mapping e live, libera margine GPU |

Al termine: **Applica**.

## 4. Profili per singola applicazione

I profili per-app hanno priorità sulle impostazioni globali e ti proteggono se in futuro cambi i globali. Vale la pena crearli anche se i globali sono già corretti.

Percorso:

```text
Pannello di controllo NVIDIA → Gestisci impostazioni 3D → Impostazioni programma → Aggiungi
```

Crea un profilo per ciascuna app che usi:

- TouchDesigner
- MadMapper
- Resolume
- Unreal Engine
- Blender
- Notch

Per ognuna imposta almeno:

- **Processore grafico preferito** → NVIDIA ad alte prestazioni
- **Modalità gestione energetica** → Preferisci prestazioni massime

In parallelo, imposta la preferenza anche lato Windows (è un secondo livello, indipendente dal pannello NVIDIA):

```text
Impostazioni → Sistema → Schermo → Grafica → [app] → Opzioni → Prestazioni elevate
```

## 5. HAGS, VRR, Game Mode — da testare, non da presumere

Nessuna di queste ha una risposta universale: dipendono da macchina e progetto.

**HAGS (Pianificazione GPU con accelerazione hardware)**

```text
Impostazioni → Sistema → Schermo → Grafica → Impostazioni grafiche predefinite
→ Pianificazione GPU con accelerazione hardware
```

Richiede riavvio a ogni cambio. Su alcune configurazioni riduce la latenza di input, su altre introduce micro-stutter con TouchDesigner/Notch. Testa ON e OFF con un progetto reale e misura, non fidarti della sensazione.

**VRR (Variable Refresh Rate)**

```text
Impostazioni → Sistema → Schermo → Grafica → Frequenza di aggiornamento variabile
```

Utile nel daily su monitor che la supportano. Da disattivare per show con proiettori o media server che non la gestiscono.

**Game Mode**

```text
Impostazioni → Giochi → Modalità gioco
```

In genere lasciabile attivo: limita alcune notifiche a schermo intero. Disattivalo se noti interferenze con overlay o cattura schermo di software specifici.

## 6. Metodo

Annota in [troubleshooting.md](troubleshooting.md) quale combinazione driver / HAGS / VRR / Game Mode funziona meglio **per ogni macchina** e per ogni tipo di progetto. La combinazione buona su ROG non è detto sia quella buona su MSI.

Prima di uno show, verifica in un colpo solo:

```powershell
nvidia-smi --query-gpu=name,driver_version,power.limit,temperature.gpu --format=csv
```
