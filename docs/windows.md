# Windows

Guida passo passo alle impostazioni di sistema per una macchina da live. Percorsi riferiti a Windows 11.

## 1. Account e accesso

1. Account principale con nome riconoscibile (serve a capire di chi è la macchina quando ce ne sono cinque in regia).
2. PIN Windows Hello: `Impostazioni → Account → Opzioni di accesso → PIN`.
3. **Account amministratore locale di emergenza**, con password diversa e conservata offline:

```text
Impostazioni → Account → Altri utenti → Aggiungi account
→ Non ho le informazioni di accesso di questa persona
→ Aggiungi un utente senza account Microsoft
→ crealo, poi Cambia tipo di account → Amministratore
```

Serve il giorno in cui l'account principale non fa login e sei a due ore dal soundcheck.

## 2. Windows Update

**Uso normale:** tieni il sistema aggiornato, riavvia e verifica la stabilità dopo ogni aggiornamento importante.

**Prima di un live:**

1. Metti in pausa gli aggiornamenti per almeno una settimana:
   `Impostazioni → Windows Update → Sospendi aggiornamenti`
2. Non installare update driver o BIOS nelle 24-48 ore prima dello show.
3. Solo edizioni Pro/Enterprise — blocca il riavvio automatico:
   `gpedit.msc → Configurazione computer → Modelli amministrativi → Componenti di Windows → Windows Update → Non riavviare automaticamente con utenti connessi…` → **Attivata**
   Su Home questa policy non c'è: usa la pausa aggiornamenti.

## 3. Alimentazione

Con alimentatore collegato.

### 3.1 Modalità alimentazione

```text
Impostazioni → Sistema → Alimentazione e batteria → Modalità alimentazione
```

- Daily: **Prestazioni migliori**
- Show: **Prestazioni massime**

### 3.2 Attivare Ultimate Performance

Se il piano non compare, apri **PowerShell come amministratore**:

```powershell
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```

Poi selezionalo da `Pannello di controllo → Opzioni risparmio energia`.

### 3.3 Impostazioni avanzate

```text
Pannello di controllo → Opzioni risparmio energia → Modifica impostazioni combinazione
→ Cambia impostazioni avanzate risparmio energia
```

| Voce | Valore | Perché |
|---|---|---|
| Sospensione → Sospendi dopo | Mai | — |
| Disco rigido → Disattiva disco rigido dopo | 0 (Mai) | Evita lo spin-down su SSD/HDD esterni con i media |
| PCI Express → Risparmio energia stato collegamento | Disattivato | Evita micro-lag su periferiche PCIe/NVMe al cambio di stato |
| Impostazioni USB → Sospensione selettiva USB | Disabilitato | Previene disconnessioni di interfacce audio, DMX/Art-Net, MIDI |
| Schermo → Disattiva schermo dopo | Mai (per show) | — |

Se la voce **Sospensione selettiva USB** non compare (frequente sui notebook con Modern Standby), forzala da PowerShell amministratore:

```powershell
powercfg /setacvalueindex SCHEME_CURRENT 2a737441-1930-4402-8d77-b2bebba308a3 48e6b7a6-50f5-4782-a5d4-53bb8f07e226 0
powercfg /setactive SCHEME_CURRENT
```

Vedi anche [network-usb.md](network-usb.md) per il risparmio energia sui singoli USB Root Hub, che è un'impostazione separata e va disattivata a parte.

### 3.4 Coperchio e pulsante

```text
Pannello di controllo → Opzioni risparmio energia → Specifica comportamento chiusura coperchio
```

Con alimentatore collegato: **Non intervenire**. Un coperchio chiuso per sbaglio non deve mandare in sospensione la macchina a metà show.

## 4. Avvio automatico

```text
Ctrl+Shift+Esc → Avvio     (oppure Impostazioni → App → Avvio)
```

**Disattivare:** Teams, Xbox / Game Bar, Phone Link (Collegamento al telefono), Dispositivi mobili se non usati, Browser Assistant, launcher e updater non necessari (Adobe Updater, Steam, ecc.).

**Lasciare attivi:** Sicurezza di Windows, driver audio, servizi ASUS/MSI per ventole/profili/GPU (Armoury Crate Service, MSI Center Service), driver NVIDIA/Intel essenziali.

## 5. Notifiche e distrazioni durante lo show

1. **Non disturbare / Assistente notifiche** attivo:
   `Impostazioni → Sistema → Notifiche → Non disturbare` → On
   Evita popup di mail e chat sopra il canvas o, peggio, sopra l'output video.
2. **Suoni di sistema di connessione/disconnessione dispositivi** disattivati:
   `Pannello di controllo → Audio → Suoni` → eventi *Connessione dispositivo* / *Disconnessione dispositivo* → **Nessuno**.
   Con molte periferiche USB hot-plug, un "dispositivo scollegato" a volume di sala non è l'ideale.
3. **Screensaver e blocco automatico** disattivati:
   `Impostazioni → Personalizzazione → Schermata di blocco → Screen saver` → Nessuno
   `Impostazioni → Account → Opzioni di accesso → Blocco dinamico` → Off

## 6. Defender

Non disattivare Defender. Aggiungi esclusioni mirate.

```text
Sicurezza di Windows → Protezione da virus e minacce → Gestisci impostazioni
→ Esclusioni → Aggiungi o rimuovi esclusioni → Aggiungi un'esclusione → Cartella
```

Escludi:

- cartelle progetti TouchDesigner / MadMapper
- media show
- cache e render
- SSD esterni usati per contenuti 4K

Esempi:

```text
D:\SHOWS
E:\MEDIA
C:\TD_PROJECTS
```

Le esclusioni riducono la latenza di lettura/scrittura sui file grandi perché Defender non li riscansiona a ogni accesso: si sente soprattutto nel caricamento di media pesanti in TouchDesigner/Resolume. Escludi cartelle di lavoro, non l'intero disco di sistema.

## 7. Rete durante lo show

1. Se usi Art-Net/sACN su cavo, imposta la scheda su profilo **Privato**:
   `Impostazioni → Rete e Internet → Ethernet → Tipo di profilo di rete → Rete privata`
   Il firewall diventa meno restrittivo sul traffico UDP/multicast locale.
2. **Disattiva il Wi-Fi** se usi solo Ethernet per DMX/Art-Net: eviti che Windows tenti roaming o cambio rete a metà show.
3. Verifica che il firewall non blocchi le porte del media server (Art-Net: **6454 UDP**):
   `Windows Defender Firewall → Impostazioni avanzate → Regole connessioni in entrata`

Dettagli su schede di rete e USB in [network-usb.md](network-usb.md).

## 8. OneDrive

**Usa OneDrive per:** documenti leggeri, preventivi, PDF, note.

**Non usare OneDrive per:** media show, cache, video 4K, progetti live pesanti.

Prima di uno show, mettilo in pausa:

```text
icona OneDrive nella tray → ingranaggio → Sospendi sincronizzazione → 8 ore
```

Altrimenti consuma banda e IO disco proprio mentre stai caricando i media pesanti.

## 9. Verifica finale prima di uscire dal Daily

Vedi la [Checklist SHOW MODE](../checklists/show-mode.md).
