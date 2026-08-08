# MSI

Guida passo passo. Su MSI i nomi delle modalità cambiano parecchio tra serie (Raider, Titan, Creator, Stealth, Vector): dove il nome varia è indicato. La logica resta la stessa.

## 1. Software da mantenere

| Componente | Tenere | Perché |
|---|---|---|
| MSI Center | Sì | Profili prestazioni, ventole, MUX switch. Equivalente di Armoury Crate |
| Driver chipset | Sì | — |
| Driver audio | Sì | — |
| Driver Intel / Wi-Fi / Bluetooth | Sì | — |
| Driver NVIDIA | Sì | Vedi [nvidia.md](nvidia.md) |
| Utility ventole / profili / MUX | Sì | Di solito già incluse in MSI Center, non installarle a parte |

Da rimuovere: trial di antivirus di terze parti, launcher di giochi non usati, **MSI True Color** se non ti serve calibrazione colore specifica.

Nota: MSI Center scarica le funzioni a moduli. Se non vedi la voce che ti serve, aprilo e controlla in **Support → Componenti / Feature Set** che il modulo sia installato.

## 2. Modalità prestazioni

### 2.1 DAILY MODE — procedura

1. Apri **MSI Center** → **Funzionalità / Features** → **User Scenario**.
2. Seleziona **Balanced** (o **Performance** moderato se ti serve più reattività).
3. Modalità GPU: **Hybrid / MSHybrid / Optimized** (il nome varia per modello) — iGPU + NVIDIA on-demand.
4. Windows: `Impostazioni → Sistema → Alimentazione e batteria → Modalità alimentazione` → **Bilanciato**.

### 2.2 SHOW MODE (RTX 3080) — procedura

Eseguire **in quest'ordine**:

1. **Collega l'alimentatore.** La RTX 3080 mobile a batteria è fortemente limitata nel TDP: si perde facilmente il 50-70% delle prestazioni.
2. MSI Center → **Impostazioni / Settings** → cerca **GPU Switch** o **MUX Switch** → imposta **Discrete / dGPU only**.
   Se il tuo modello non ha la voce, non ha il MUX: resta in Hybrid e passa al punto 4.
3. **Riavvia.** Sui MSI con MUX fisico è quasi sempre necessario.
4. MSI Center → **User Scenario** → **Extreme Performance** (su alcuni modelli: **Turbo**).
5. Windows → piano **Prestazioni massime / Ultimate Performance** (vedi [windows.md](windows.md#3-alimentazione)).
6. **Cooler Boost:** attivalo solo se hai già verificato che il rumore sia accettabile per il contesto. In teatro o in eventi silenziosi è spesso troppo rumoroso. **Provalo in prova, non il giorno dello show.**
7. **Verifica** (paragrafo 2.3).
8. **Solo ora** apri il software live.

### 2.3 Verifica che la dGPU sia attiva

```text
Impostazioni → Sistema → Schermo → Impostazioni schermo avanzate → Visualizza informazioni scheda
```

Oppure da PowerShell, con il software live già aperto:

```powershell
nvidia-smi
```

Il processo (`TouchDesigner.exe`, `Resolume Arena.exe`, …) deve comparire nella tabella dei processi. Se la tabella è vuota, l'app sta girando sulla GPU sbagliata: chiudila e riaprila **dopo** il riavvio.

Alternativa: `Gestione attività → Dettagli → Seleziona colonne → Motore GPU`.

## 3. RTX 3080 mobile — note specifiche

- Esiste in più varianti di TDP a seconda del laptop (circa 80W fino a 150W+ con Dynamic Boost). Le prestazioni reali dipendono dal raffreddamento del modello MSI specifico, non solo dal profilo software: due 3080 mobile non sono la stessa scheda.
- **Dynamic Boost:** se presente in MSI Center, lascialo attivo in Show Mode. Sposta potenza tra CPU e GPU in base al carico, utile con TouchDesigner/Notch che hanno carico misto.
- **Prima volta in Turbo su un progetto pesante:** monitora le temperature per almeno 20 minuti. Se vedi throttling sostenuto (clock che cala e resta basso), il problema è quasi sempre pulizia dissipatori/pasta termica.

## 4. Regola non negoziabile sul MUX

Su alcuni MSI il cambio MUX richiede riavvio. Fallo **sempre prima** di aprire il software live, **mai a caldo** con l'app già aperta: l'app resta agganciata alla GPU sbagliata finché non la chiudi e riapri, e in mezzo a uno show non è il momento di scoprirlo.

## 5. Problemi ricorrenti

- **MSI Center non applica i cambi.** Verifica in `services.msc` che i servizi **MSI Center Service** / **MSI Foundation Service** siano in esecuzione, poi riprova.
- **La voce GPU Switch è sparita dopo un update.** Di solito è il modulo MSI Center da reinstallare da Support → Feature Set.
- **Prestazioni basse pur in Extreme Performance.** Controlla nell'ordine: alimentatore collegato, MUX su discrete, riavvio fatto, app effettivamente su NVIDIA, driver Studio aggiornato.
