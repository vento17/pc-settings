# Rete e USB

## Ethernet

Percorso:

```text
Gestione dispositivi → Schede di rete → [Ethernet] → Proprietà
```

### Risparmio energia

Disattivare:

- Consenti al computer di spegnere il dispositivo per risparmiare energia

### Avanzate

Se disponibili, impostare:

- Energy Efficient Ethernet (EEE): Disabled
- Green Ethernet: Disabled
- Auto Disable Gigabit: Disabled
- Speed & Duplex: Auto Negotiation

Interrupt Moderation:

- Enabled per uso generale.
- Disabled solo se si cerca latenza più bassa e si accetta più carico CPU.

Per Art-Net/sACN/OSC, evitare di forzare velocità se non necessario. Auto è più robusto.

## USB

Su molti notebook Windows 11 con Modern Standby la voce “Sospensione selettiva USB” è nascosta o non configurabile da interfaccia. È normale: in quel caso si forza da `powercfg`, vedi [windows.md](windows.md#33-impostazioni-avanzate).

### Metodo pratico

Percorso:

```text
Gestione dispositivi → Controller USB
```

Per ogni voce tipo:

- USB Root Hub
- Hub radice USB
- Generic USB Hub
- Hub USB generico / Hub USB SuperSpeed generico
- Hub USB 3.x

aprire:

```text
Proprietà → Risparmio energia
```

e togliere la spunta a:

```text
Consenti al computer di spegnere il dispositivo per risparmiare energia
```

Se la scheda **Risparmio energia** non esiste su una voce, ignorala: significa che quel dispositivo non espone l'impostazione.

### Verificare lo stato di tutti i dispositivi in un colpo solo

Cliccare device per device è lento e si perde il conto. Questo comando elenca ogni dispositivo che espone la casella e dice se è spuntata (`RisparmioAttivo = True` significa **da togliere**):

```powershell
Get-CimInstance -Namespace root/wmi -ClassName MSPower_DeviceEnable | ForEach-Object {
  $id = $_.InstanceName -replace '_0$',''
  [pscustomobject]@{
    RisparmioAttivo = $_.Enable
    Nome = (Get-PnpDevice -InstanceId $id -ErrorAction SilentlyContinue).FriendlyName
  }
} | Where-Object Nome | Sort-Object -Descending RisparmioAttivo | Format-Table -AutoSize
```

Utile anche come verifica dopo un aggiornamento di Windows: le reinstallazioni di driver rimettono la casella al valore di default.

## Porte COM e adattatori USB-seriale

Qui è dove ci si perde, quindi vale la pena essere precisi. Riguarda Enttec DMX USB, interfacce FTDI, CH340, CP210x, PL2303.

### 1. La categoria non esiste finché non colleghi nulla

Gestione dispositivi **nasconde del tutto** la categoria *Porte (COM e LPT)* quando è vuota. Se non la vedi, quasi sempre non c'è semplicemente nessun dispositivo seriale collegato: non è una voce nascosta da scovare.

Per vedere anche i dispositivi non collegati (fantasmi di adattatori usati in passato):

```text
Gestione dispositivi → Visualizza → Mostra dispositivi nascosti
```

### 2. Sulla COM port la scheda "Risparmio energia" non c'è

La casella vive sul dispositivo USB **padre**, non sulla porta virtuale. Per un FTDI la gerarchia reale è:

```text
Controller USB → USB Serial Converter        ← qui c'è "Risparmio energia"
   └── Porte (COM e LPT) → USB Serial Port (COM3)   ← qui NON c'è
```

Quindi l'impostazione va tolta sul converter e sull'hub che lo regge, non sulla COM.

### 3. Come trovare il dispositivo padre

Due strade:

```text
Gestione dispositivi → Visualizza → Dispositivi per connessione
```

Mostra l'albero reale: dalla COM port risali al converter, all'hub e al controller.

Oppure, se preferisci l'ID esatto:

```text
tasto destro sulla COM → Proprietà → Dettagli → Proprietà: Elemento padre
```

e cerchi quell'ID sotto *Controller USB*.

Da PowerShell, la stessa cosa per tutte le COM presenti:

```powershell
Get-PnpDevice -Class Ports | ForEach-Object {
  [pscustomobject]@{
    Porta  = $_.FriendlyName
    Padre  = (Get-PnpDeviceProperty -InstanceId $_.InstanceId -KeyName 'DEVPKEY_Device_Parent' -ErrorAction SilentlyContinue).Data
  }
}
```

Se non restituisce nulla, non hai porte COM collegate (vedi punto 1).

### 4. FTDI — le due impostazioni che contano davvero

Sugli adattatori FTDI il driver VCP espone un pannello dedicato, ed è lì che si risolvono i problemi di DMX/Art-Net:

```text
Porte (COM e LPT) → USB Serial Port (COMx) → Proprietà
→ Impostazioni della porta → Avanzate
```

- **Selective Suspend:** togliere la spunta. Evita che la porta venga sospesa in un momento di traffico basso e non si risvegli in tempo.
- **Latency Timer:** portarlo da **16 ms** (default) a **1-2 ms**. È la latenza con cui il driver raggruppa i dati prima di consegnarli: 16 ms su un flusso DMX si sentono.

Su Prolific, CH340 e CP210x questo pannello è più povero e le due voci non ci sono: lì contano solo il dispositivo padre (punto 3) e il piano di alimentazione.

### 5. Ordine di intervento

1. Sospensione selettiva USB disattivata nel piano di alimentazione ([windows.md](windows.md#33-impostazioni-avanzate)) — è l'interruttore generale.
2. Risparmio energia tolto sugli hub radice e sugli hub generici.
3. Risparmio energia tolto sul converter USB-seriale (il padre della COM).
4. Su FTDI: Selective Suspend off e Latency Timer a 1-2 ms.

Fare solo il punto 4 senza i primi tre non basta.

## Periferiche live

Collegare prima di aprire il software:

- SSD esterni
- DMX USB
- MIDI controller
- capture card
- interfacce audio
- dongle/licenze
- schede di rete USB

Evitare hotplug non necessario durante lo spettacolo.

Se una periferica finisce sotto un hub esterno o un dock, ricordati che la casella dell'hub conta più di quella della periferica: un hub che va in risparmio energia si porta dietro tutto quello che ha attaccato.
