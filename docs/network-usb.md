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

Su molti notebook Windows 11 con Modern Standby la voce “Sospensione selettiva USB” è nascosta o non configurabile da interfaccia. È normale.

### Metodo pratico

Percorso:

```text
Gestione dispositivi → Controller USB
```

Per ogni voce tipo:

- USB Root Hub
- Hub radice USB
- Generic USB Hub
- Hub USB 3.x

aprire:

```text
Proprietà → Risparmio energia
```

e togliere la spunta a:

```text
Consenti al computer di spegnere il dispositivo per risparmiare energia
```

Se la scheda non esiste, ignorare quella voce.

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
